---
name: android-root-architecture
description: 通用介绍 Android 启动镜像结构及 Magisk、KernelSU、APatch、ShiroSu 的实现机制与选型核对项。
---

# Android 启动镜像与 Root 机制

本文概述启动镜像的组成、早期启动过程，以及几种常见 Android root 方案的机制。具体设备布局和项目支持情况会随设备、固件和项目版本而变，操作前应以设备当前固件和项目官方资料为准。

## 启动镜像与 init 阶段

Android 12 的通用启动镜像可包含 GKI 内核和通用 ramdisk。Android 13 新推出的设备可将通用 ramdisk 独立放入 `init_boot`，此时 `boot` 放内核；升级到新 Android 版本的旧设备不一定采用这种布局。因此不能仅凭 Android 版本推断分区内容：先核对设备分区表、固件镜像和 [Android 通用启动分区说明](https://source.android.com/docs/core/architecture/partitions/generic-boot)。

ramdisk 是启动时使用的内存文件系统，里面的文件通常以 cpio 归档保存，归档可能经过压缩。它不是“未压缩的压缩包”。设备启动时，内核启动 ramdisk 中的 first-stage `init`；之后进入 system init 阶段，Android init 继续处理服务和系统启动。first-stage init 与 `/system/bin/init` 的关系取决于设备启动布局和实现，不应简化成所有设备都由 ramdisk init 直接查找并启动同一路径。

### 检视已有镜像副本

仅对已取得的镜像副本进行检视，不直接操作设备分区。先确认当前环境确实有 `magiskboot`，且它在 `PATH` 中；工具来源及参数以所用版本的帮助信息为准。在空的工作目录放入镜像副本后执行（输出会写入当前目录）：

```sh
magiskboot unpack boot.img
```

输出内容随镜像格式而异，可能包含内核，也可能没有内核；不要假定每个镜像都有相同文件。按 [Magisk 工具文档](https://topjohnwu.github.io/Magisk/tools.html)，默认 `unpack` 会自动解压组件；`-n` 才跳过解压。因此本例输出的 `ramdisk.cpio` 应按已解压 cpio 归档检查，不应把镜像内组件的压缩状态直接套到输出上。此示例只用于查看副本，不包含刷写或提权操作。

## 项目机制概览

### Magisk

Magisk 的 `magiskinit` 替换早期 init，负责早期挂载、服务注入、SELinux policy 处理，并恢复原始 init。后续拦截方式依架构而异；`/data/magiskinit` 是一种回退路径，不是通用固定位置。`magiskd` 在 `post-fs-data` 阶段启动。运行时文件位于基于 tmpfs 的路径（由 `magisk --path` 查询，例如 `/sbin` 或 `/debug_ramdisk`，其下有 `.magisk`）；模块和授权数据库等持久数据仍位于 `/data/adb`。参见 [Magisk 工作原理](https://topjohnwu.github.io/Magisk/details.html)。

### KernelSU

KernelSU 的 root 能力依托内核。其 GKI 路线通过构建包含 KernelSU 的内核实现；LKM 路线则保留内核并加载内核模块，通常需要修补含 ramdisk 的启动镜像：若 ramdisk 单独位于 `init_boot`，使用对应镜像；否则按设备布局核对 `boot`。KernelSU 官方资料目前说明 v3.0 不再提供或支持官方 GKI 镜像发行，当前制品为 LKM；设备是否适用要核对内核 KMI、固件安全补丁、分区布局和回滚要求，不能照搬其他设备的镜像。参见 [KernelSU 安装文档](https://kernelsu.org/guide/installation.html)。

### APatch

APatch 是内核模式 root 方案，依赖 KernelPatch 对已编译内核二进制进行补丁与 hook。其 APM 与 KPM 是不同类型的模块机制。评估时应核对目标内核和设备支持、补丁方式、模块类型及对应版本文档；不要把 KernelPatch 与名称相近的其他 kpatch 项目混为一谈。参见 [APatch 机制说明](https://apatch.dev/what-is-apatch.html)及 [KernelPatch 项目](https://github.com/bmax121/KernelPatch)。

### ShiroSu（SSU）

[2026-05-31 开发文章](https://oom-wg.dev/blog/ssu-nt)描述 ramdisk 用户空间方案：以 seccomp 用户通知拦截进入 system init 的 `execve`，讨论路径地址分析与 ASLR。它依赖早期启动权限，用户通知本身不会授予 root；实验失败原因与跨设备兼容性未证实。

项目入口：[0.1.1 beta](https://oom-wg.dev/blog/ssu-0-1-1-beta)、[网页前端](https://su.shiror.ren/)。此处未读取 beta 正文、验证前端修补或当前设备支持。

## 进程语义与 seccomp

`fork()` 创建子进程，子进程取得独立 PID；`execve()` 用新程序映像替换当前进程映像，成功时 PID 不变。fork 后的子进程是否能继续执行，需按具体代码、进程状态、安全策略和内核行为分析。

seccomp 过滤机制早于 Linux 5.0；不能把整个机制与用户通知的版本要求混为一谈。[Linux man-pages](https://man7.org/linux/man-pages/man2/seccomp.2.html)明确将 `SECCOMP_RET_USER_NOTIF` 和 `SECCOMP_FILTER_FLAG_NEW_LISTENER` 标为 Linux 5.0 引入。这也不等于任何 5.0+ Android 内核都支持该项目，仍需核对配置与所需接口。经典过滤可用 BPF 限制系统调用；BPF 过滤器不能直接解引用调用参数中的指针。user notification 允许监督者接收过滤事件并作响应，但可用性取决于内核 API、配置和具体实现。查阅 [内核 seccomp 文档](https://docs.kernel.org/userspace-api/seccomp_filter.html)，并核实目标设备的内核版本、配置和所需接口。

## 选型前核对

- 设备实际采用的启动分区布局，以及 ramdisk 所在镜像。
- 项目当前官方支持状态、设备和内核兼容条件、所需 KMI 与安全补丁级别。
- 是否依赖内核重编译、二进制补丁或加载内核模块，以及模块机制的差别。
- SELinux、启动链和回滚要求；逐项按设备及项目文档核实，不根据单篇开发文章推定。
- 目标方案如何处理授权、持久数据和运行时文件，及其对应文档说明。

不同方案的机制各有取舍；应按设备条件和维护需求选择，不能从机制描述推出稳定性或隐藏 root 能力的排名。
来源：[酷安原帖](https://www.coolapk.com/feed/72167115)。正文已读，5张配图及评论未读；本技能纠正原文并补充上列官方资料，没有执行设备命令、解包或刷写测试。

## 补充案例

- [免解 BL 临时 Root（GhostLock / CVE-2026-64560）与工程 abl 案例](references/temp-root-ghostlock-and-abl-cases.md)
- [GhostLock 592、Timer Root、MTK `lk.img` 解锁与 Fastbootd 工具案例](references/temp-root-592-timerroot-and-lk-unlock.md)
- [Android-x86 适配、Play 商店 Systemize 与临时 Root 断网修复](references/android-x86-systemizer-and-selinux-fixes.md)
- [CVE-2026-98260 漏洞分析、Shell 临时 Root 台、`bat2sh`、GBK FUSE 死锁与定制容器内核](references/cve-98260-ldpreload-bat2sh-and-gbk-fuse.md)
- [Droidspaces Action、一加 Ace6 HMBIRD GKI、MiUnlockTool、MTK Logo 改图与 Flutter 逆向](references/droidspaces-action-hmbird-rmflashtool-and-flutter-re.md)
- [KernelSU-Next / SukiSU / FinaSU AnyKernel3（5.10–6.18）与内核版本隐藏](references/ak3-ksun-sukisu-finasu-and-susfs-hiding.md)
- [Root 隐藏方案、ColorOS 17 分身抓包、9008 救砖与 iQOO/红米镜像归档](references/root-hiding-proxypin-9008-and-bl-unlock.md)
- [`rust-kit` / `rust-hook` 与高通平台第三方 ROM 基带处理笔记](references/rust-kit-and-qualcomm-baseband-notes.md)
- [短帖汇总：KSU 防检测工具箱、Shizuku-NEXT、98260 提权准备与高通 410 随身 WiFi](references/short-ksu-shizuku-98260-and-410wifi.md)
- [免解 BL 临时 Root 矩阵、春秋检测 4.5.8 逆向、开机动画路径与 DeviceSpoofer](references/ghostlock-bootanim-and-devicespoofer.md)
- [GhostLock (CVE-2026-43499) 兼容矩阵、FinaSU、CC 刷机工具、sidekey/ota-block 与 iQOO BL 校验链](references/ghostlock-cve-43499-finasu-ccflash-and-bl-chain.md)
