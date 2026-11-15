# Droidspaces Action、一加 Ace6 HMBIRD GKI、MiUnlockTool、MTK Logo 改图与 Flutter 逆向

整理自酷安社区开发者发布与技术记录（未读图片与外部仓库，未做真机复现）。

## 1. `Droidspaces_Action` 与 Non-GKI 内核构建项目

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74344333)（`imgs=1`，`https://github.com/5kind/Droidspaces_Action`）
  - 原 `5kind/LXC-DOCKER-KernelSU_Action` 更名并同步上游 `ravindu644/Droidspaces-OSS`（`v6.6.0`），支持 3.x–6.12 内核配置生成、修复开机内部错误弹窗与自定义 patch；已发布 Redmi K20 Pro（`raphael`，cgroupv1 + BakaSU）、小米平板 5 Pro 12.4（`dagu`）与 Redmi Note 11T Pro（`xaga`）产物。
- **Non-GKI 维护与 Fork 说明**：[酷安原帖](https://www.coolapk.com/feed/73996361)（`imgs=3`，`JackA1ltman/NonGKI_Kernel_Build_2nd`）— 作者说明因精力有限希望社区 Fork 自行维护新设备分支。
- **小米 9（crDroid 16.0）AI 编译 Droidspaces 内核**：[酷安原帖](https://www.coolapk.com/feed/74207574)（`imgs=7`）— 使用 KernelSU_Next，Debian 13 宝塔面板可用，注明因漏编 `CONFIG_POSIX_MQUEUE` 需按说明文档调整容器配置才能启动 Docker。

## 2. 一加 Ace6（`PLQ110`）6.6.118 GKI 内核：风驰调度 + FolkSU + BTF

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74291469)（`imgs=0`）
- **摘要**：基于 `OnePlusOSS/android_kernel_common_oneplus_sm8750`（`oneplus/sm8750_b_16.0.0_ace_6`）用 Android `clang r510928` 编译，将厂商风驰 HMBIRD 调度（`/proc/hmbird_sched` 全节点）移植回公开树，内置 `FolkSU v0.1.0-beta3`（KernelSU `32787` / UAPI 5），开启 BTF（`/sys/kernel/btf/vmlinux` 6.1MB）为 `scx`（CO-RE）铺路，保留 KASAN/UBSAN/CFI/ShadowCallStack/KASLR，开机加载 701 个官方模块。

## 3. 刷机与解锁工具链（Python 通用工具箱、`MiUnlockTool`、`RmFlashTool`）

- **Python 单文件刷机工具箱 v1.1**：[酷安原帖](https://www.coolapk.com/feed/74243702)（`imgs=0`）— 内置 `adb`/`fastboot`/`payload-dumper-go`，修复 ADB/Recovery/Sideload 互切卡死、加入 `busy` 锁、`init_boot` 3 路径探测、Magisk/APatch/KSU 识别、安全模式重启、`vbmeta` 禁用与部分机型 `oem edl`（拦截华为/荣耀/iPhone）。
- **`MiForge/MiUnlockTool` 多端解锁指南**：[酷安原帖](https://www.coolapk.com/feed/74212914)（`imgs=0`）— 覆盖 Windows/macOS/Linux（含 `externally managed environment` venv 提示）与 Termux（`termux-adb`），提示第三方工具存在账号风控与清数据风险。
- **`RmFlashTool`（Rust）线刷与 9008 教程**：
  - Fastboot 线刷：[酷安原帖](https://www.coolapk.com/feed/73633194)（`imgs=9`）— 提示天玑强解用户需删除解压包 `image/lk.img` 及脚本中 `flash lk` 行，避免使用 C-to-C 线以防断联。
  - 高通 9008 线刷：[酷安原帖](https://www.coolapk.com/feed/74004686)（`imgs=8`）— 需配套 9008 引导与线刷包，建议使用 9008 工程线并先在工具内安装驱动。

## 4. MTK `logo.bin` 改图原理与 Flutter / Rust 逆向笔记

- **MTK Logo 分区修改**：[酷安原帖](https://www.coolapk.com/feed/74193329)（`imgs=9`）— 针对 2023 年 5 月公告前固件利用 `CVE-2023-20696`（新设备用 `CVE-2025-20730`）在 `cert2` 前插入 `0xA0` 伪签名绕过 `lk` 校验；`logo.bin` 结构为头部 + zlib 压缩原始 RGBA 块（无分辨率元数据）+ `cert1` + `cert2`，替换时需重算偏移表。
- **Flutter + Rust APK 逆向笔记**：[酷安原帖](https://www.coolapk.com/feed/73604943)（`imgs=3`）— 指出业务核心位于 `lib/arm64-v8a/libapp.so`，Dart 反编译推荐 `blutter`（`worawit/blutter`）或 `flutterdec`（`caverav/flutterdec`），`flutter_rust_bridge` 后端推荐原生支持 Rust 反编译的 Ghidra；修改可选静态 hex patch 或动态 Frida hook。
