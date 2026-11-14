# GKI 内核集成 SukiSU 的历史构建记录

[酷安教程](https://www.coolapk.com/feed/66512180)以 Android 13、5.15 GKI 为例介绍源码同步、Clang、SukiSU 和 KPM；它不是所有设备通用的构建配方。设备的 Android/内核分支、ABI、构建工具和启动镜像需先核对。

## 获取与构建

原帖建议 Ubuntu 20.04/22.04/24.04 或 WSL2，安装 Git、repo、编译器、构建工具及内核依赖。原帖中的 manifest URL 把 `-b` 连进地址；对照 [AOSP 构建说明](https://source.android.com/docs/setup/build/building-kernels)，可将分支参数独立写为：

```sh
repo init -u https://android.googlesource.com/kernel/manifest -b common-android13-5.15-lts -m default.xml --depth=1
repo --trace sync -c --no-clone-bundle --no-tags --fail-fast -j16
```

分支只是原帖示例，应先验证对应设备和 manifest。原帖提到从 [Android Clang 归档](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/+archive/refs/heads/main/clang-r547379.tar.gz)获取工具链，并检查构建配置所引用的版本；不能因文章列出某个版本就断言所有内核支持。原帖建议清空 `check_defconfig`，这会改变配置检查，应先理解目标分支的验证流程，不应默认照做。

原帖使用旧版 `BUILD_CONFIG=common/build.config.gki.aarch64 build/build.sh`；AOSP 说明 Android 14 及以上不支持 `build.sh`，可用的分支应按上游选择 Kleaf/Bazel 或旧流程。原帖加 `sudo`，并非上游示例的必要部分。输出路径以实际构建日志为准。

## SukiSU 与 KPM

原帖给出 `CONFIG_KALLSYMS`、`CONFIG_KALLSYMS_ALL`、`CONFIG_KPROBES`、`CONFIG_HAVE_KPROBES`、`CONFIG_KPROBE_EVENTS`、`CONFIG_KSU`、`CONFIG_KPM` 等配置。配置是否存在、依赖是否满足取决于源码版本；[SukiSU 集成文档](https://github.com/SukiSU-Ultra/SukiSU-Ultra/blob/main/docs/zh/guide/how-to-integrate.md)当前列出 main 分支的 `CONFIG_KPROBES=y`、`CONFIG_EXT4_FS=y` 等要求，并说明 `CONFIG_KSU` 可内置或构建模块。

教程中 setup 命令的引号不完整；上游文档给出的地址是 [setup.sh](https://raw.githubusercontent.com/SukiSU-Ultra/SukiSU-Ultra/main/kernel/setup.sh)。阅读并核对该脚本、分支和上游说明后再决定运行。原帖称编译后的 `Image` 使用 [KPM 工具目录](https://github.com/SukiSU-Ultra/SukiSU_patch/tree/main/kpm)中的 `patch_linux` 修补得到 `oImage`，但没有在本文提供可核验的工具版本、参数或实测；不要根据这一描述假定任意二进制均可直接运行。

本文只核对公开文档与原帖文本，未下载工具链、编译或启动设备；原帖图片和完整评论未读。
