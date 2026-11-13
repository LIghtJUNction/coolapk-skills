---
name: mt-terminal-kimi-native-runtime
description: 理解 ARM64 安卓 MT 终端安装 Kimi Code 的原生运行时方案，包含公开脚本分享、Bionic/解释器依赖、目录和版本验证。
---

# MT 终端中的原生 Kimi Code 运行时

[原帖安装脚本分享](https://pan.baidu.com/s/1abhvtCedLVSvrLTi4bbqNA?pwd=6666)，公开提取码 `6666`。本次未下载脚本，原帖没有给出具体文件名、脚本版本、校验值或运行命令，因此这里不编造 `curl | sh`、`npm install` 或解压执行步骤。

## 为什么不能只复制 Node

原帖针对 **ARM64 安卓手机**。其解释是 Termux 的 ARM64 程序同样使用 Android Bionic 和 `/system/bin/linker64`，但还要处理共享库依赖、写死的目录与脚本解释器。单独复制 Node 可执行文件并不等于依赖齐备。

## 脚本按原帖描述的工作顺序

1. 检查 ARM64 架构与必要命令。
2. 从 Termux 引导包取出 `xz`、`liblzma`，解决 MT 终端解包 `.deb` 的问题。
3. 提取 Termux 官方 Node.js、npm、ripgrep 及依赖库到 `~/.node-android`，不安装整个 Termux 应用。
4. 设置 PATH、动态库搜索路径、临时目录和证书；用 `/system/bin/sh` 启动脚本，处理原先不可用的 `#!/usr/bin/env node`。
5. 将 `libcares.so` 内硬编码的 Termux DNS 配置路径改为 MT 私有目录，剩余位置按原帖用 `00` 填充。原帖未给出偏移、替换字节或库版本，不能据此手工对任意库打补丁。
6. 先检查 Node、npm、ripgrep、DNS、HTTPS，全部通过后才安装 npm 版 Kimi Code。
7. 生成新的 Kimi 启动 shell，再检查版本。

## 位置和检查

| 位置或命令 | 用途 |
|---|---|
| `~/.node-android` | 原帖脚本解出的原生运行时及依赖 |
| `~/.kimi-code` | 原帖称不会删除的旧目录；不代表所有运行状态都不改变 |
| `~/mt-kimi-install.log` | 完整安装日志 |
| `/system/bin/linker64` | 文中说明的 Android 动态链接器 |

在脚本已经成功安装并配置启动入口后，原帖使用：

```sh
kimi --version
```

原帖称方案不刷系统、不需要 proot/chroot，但没有给出最低 Android/MT 版本、非 ARM64 支持或各设备的成功清单。`kimi --version` 成功仅能确认启动入口，不能代替 API 登录、实际请求和完整功能验证。

来源：[原帖](https://www.coolapk.com/feed/74723798)。整理日期：2026-11-13。原帖经验未做实机复现，图片与评论未全部读取；公开分享页及文件内容未逐一验证。
