---
name: android-native-core-ipc
description: 设计 Android Kotlin 与 Go Native 核心解耦时，使用共享库核心、PIE 启动壳、Unix socketpair、SCM_RIGHTS 和 VpnService.protect；明确重连与加载边界。
---

# 动态加载 SO 与内核切换

**来源：**[原文《给代码塞点新玩具》](https://yumeyuka.moe/blog/load-so-kernel-switch/index.md)；相关前文：[《让 APK 轻一点》](https://yumeyuka.moe/blog/apk-shrink/index.md)

## 为什么从 JNI Bridge 转向进程壳

YumeBox 初期复用 [ClashMetaForAndroid Core JNI](https://github.com/MetaCubeX/ClashMetaForAndroid/tree/main/core/src)：Kotlin 经 JNI、C/CGO 胶水层调用 Go Core。作者认为这种方式稳定，但新增功能往往需要同时维护 Kotlin、JNI、C 和 CGO，多语言状态与生命周期耦合增加了维护成本。桌面端/Web 面板仍可走内核提供的 [REST API](https://wiki.metacubex.one/api/)。

Root TUN 需求又带来生命周期问题：核心应能独立于 App 继续运行；如果核心依赖 JNI 与绑定 Service，App 被系统回收后，控制连接和状态难以恢复。文章提出让核心作为独立进程运行，由 App 通过本地 IPC 重连和控制。

## 共享库核心与 PIE 启动壳

方案的核心是：Go 内核仍编译为共享库，由一个很小的 PIE 启动壳 `dlopen` 核心，再调用唯一的 `MihomoMain` 入口。这样 Kotlin 不再直接维护大量 Go 导出函数/JNI 接口。

```c
// shell.c：启动壳只负责加载核心并转交入口

void *library = dlopen("libmihomocore.so", RTLD_NOW | RTLD_LOCAL);
int (*main_entry)(int, char **, char *) = dlsym(library, "MihomoMain");
return main_entry(argc, argv, channel);
```

```go
// entry.go：核心业务从这里开始，但不再暴露几十个 JNI 方法

//export MihomoMain
func MihomoMain(argc C.int, argv **C.char, channel *C.char) C.int {
    return C.int(runMain(readArgs(argc, argv), C.GoString(channel)))
}
```

片段展示入口结构，不是完整可编译程序：实际代码还需检查 `dlopen`/`dlsym` 返回值、函数签名与 ABI、参数生命周期、错误传播及 Go/CGO 导出设置。原文没有附完整构建命令或全部 shell 实现。

共享库加载和 PIE 进程执行是两种机制。[Android 10 官方说明](https://developer.android.com/about/versions/10/behavior-changes-10#execute-permission)限制的是以 Android 10/API 29 为目标的非特权应用直接对应用主目录文件调用 `execve()`；不能把原文“只有解压 lib 目录能运行”概括成所有 Android 进程、所有路径的唯一规则。共享库不需要文件的可执行权限位，也不代表 ABI、依赖、SELinux 或加载器命名空间限制消失。原片段只给库名，查找位置仍取决于实际加载器环境。

## socketpair 作为父子进程通道

独立进程后，配置、VPN 建立的 TUN FD，以及 Android `protect()` 请求都需要跨进程传递。文章选择 Unix `socketpair`，启动时创建 `AF_UNIX + SOCK_SEQPACKET` 通道，一端留在 Kotlin，另一端交给 Go 核心：

```c
int channel[2];
if (socketpair(AF_UNIX, SOCK_SEQPACKET, 0, channel) < 0) {
    /* 通信通道创建失败，核心不能继续启动 */
}
```

`SOCK_SEQPACKET` 保留消息边界，因此配置可以分多条消息传递，消息边界得以保留，不需要用流式协议的长度头来识别包边界；但接收缓冲区不足仍可能截断/丢弃内容，必须定义包大小、分块完成标记和异常处理。Kotlin 示例按一次调用发送一条消息：

```kotlin
// Channel.kt：一次调用对应一条消息。
channel.writeMessage(configBytes, offset, size)
```

这条 socket 不监听外部网络端口，仅存在于父子进程间；核心启动后继续保留，以处理需要 Android 侧完成的 App UID 查询和出站 socket 保护。原文未列出 FD 传递、协议版本、消息大小上限及断线恢复实现，落地时需定义这些边界。

## VPN 出站 socket 保护

VPN TUN 会捕获设备流量。若核心连接代理服务器的出站 socket 也被 VPN 再次捕获，流量会回到 TUN 形成回环。文章采用按 socket 保护，而不是把整个 App UID 排除出 VPN：核心在连接前将 FD 通过 `SCM_RIGHTS` 交给 App，App 调用 `VpnService.protect(fd)`。

Go 侧钩子示例：

```go
// hooks.go：在 connect 前把核心 socket 交给 Android 处理

dialer.DefaultSocketHook = func(_, _ string, conn syscall.RawConn) error {
    var protectErr error
    conn.Control(func(fd uintptr) {
        protectErr = rpc.protect(int(fd))
    })
    return protectErr
}
```

Kotlin 侧处理并在 `finally` 关闭接收的 FD 副本：

```kotlin
// CoreProcess.kt：只保护核心传来的 socket，处理后立即关闭副本

private fun protectCoreSocket(fd: Int): String = try {
    if ((context as? VpnService)?.protect(fd) == true) "protected" else "denied"
} finally {
    ParcelFileDescriptor.adoptFd(fd).close()
}
```

代码依赖项目自有 `rpc.protect`、正确的 `SCM_RIGHTS` 收发与 FD 所有权约定。若关闭错误的一端、漏关副本或在 FD 生命周期结束后调用 protect，都可能造成泄漏或失败。示例将非 `VpnService` 情况映射为 `denied`；实际调用方必须据此处理启动失败/网络回环，不能假设保护一定成功。

## 节点预览壳

Android 端若没有持续运行的 Service 拉起真正核心，仍可通过另一个轻量壳读取配置并提供节点预览 API。该预览流程不启动核心进程，也不接管代理流量；真正运行时再使用负责启动核心的壳。预览结果因此不能等同于核心实际启动后的运行时状态。

## 运行时切换内核

不同版本内核保持相同 `MihomoMain` 入口，壳层可以不变；构建不同版本的共享库后，运行时选择相应核心即可。文章称 Android 侧需要单独工作流，实时同步上游源码并构建共享库形态，因为 Android 需要的是共享库，而不是上游提供的普通可执行文件。作者把减少源码改动作为维护目标，主要改动集中在构建时 tag 限制，并通过壳提供进程、TUN 和切换能力。

原文没有提供云端工作流文件、下载地址、版本签名校验或切换事务代码，因此这里只能还原其架构描述，不能据此独立配置自动更新。动态替换核心时需要确保 ABI、入口签名、依赖和数据协议相容，并避免运行中的进程加载到被覆盖的文件；这些实现细节不在原文中。

文章另提到自定义图标扩展流程：网页组件裁切用户图标，经 Cloudflare S3 临时存储并返回链接，前端引导用户创建构建仓库 Issue，再由 GitHub Action 触发构建。原文未给出该存储桶、仓库 Issue 模板或 Action 的公开固定链接，故不补造操作步骤。
## 不能由片段推导出的保证

- 一次 `socketpair` 只建立当时父子进程的通道，并不会自动让重启后的 App 接入原核心。原文没有提供重连端点转交、守护启动或恢复协议，因此“独立运行并成功重连”尚不能据此验证。
- Go 钩子片段没有处理 `conn.Control(...)` 本身返回的错误；实际实现还需传递这层错误，不能只检查回调内的 `protectErr`。


本技能来自酷安原帖所链接的公开站点文章，单独作为外链知识整理；不算作酷安帖子正文覆盖。正文已读，配图未读。除单独注明的本地验证外，未运行项目、安装扩展或下载发行包。
