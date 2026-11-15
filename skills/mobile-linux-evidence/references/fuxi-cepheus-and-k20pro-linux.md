# 小米 13 主线双系统、小米 9 Linux UEFI（Docker）与 K20 Pro Ubuntu Chroot

整理自酷安社区开发者移植与实验记录（未读图片与外部仓库全量文档，未在真机复现）。

## 1. 小米 13（`fuxi` / SM8550）：LineageOS 24.0 + 主线 Linux 7.3.0 双系统

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74643102)（`imgs=2`，仓库 `https://github.com/ilovecnxx/fuxi-linux-build`，tag `v0.1.0-dualboot-20261027`）
- **架构与引导**：Android（LineageOS 24.0）位于槽位 `b`，Ubuntu 26.10 根文件系统位于 `linux` 分区 `/dev/sda33`（ext4，100 GB）；正常开机进 Android，恢复模式启动 `recovery_b` 中的自编译纯主线 `Linux 7.3.0`（内嵌 `801,847 B` 厂商 `fuxi.dtb` 作 ABL 引导诱饵），支持无电脑双系统切换。
- **硬件矩阵**：SM8550 8 核 SMP、内存、UFS G4（HS-G5 暂缓）、`simpledrm + fbcon` 文本控制台（`tty1`）、USB 串口 `/dev/fuxi-acm`（initramfs 8 秒调试窗）及 SSH over USB（`10.42.0.2`）可用；触摸、Wi-Fi/蓝牙、GPU 桌面、音频/相机/通话暂不支持。

## 2. 小米 9（`cepheus`）：Linux UEFI + Wi-Fi / USB NCM + Docker 服务器

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74729048)（`imgs=2`，上游 `https://github.com/qaz6750/linux-xiaomi-cepheus-uefi`）
- **改进与刷入**：基于上游 UEFI 镜像构建项目修复 Server 版 Wi-Fi 与 USB NCM 网卡并编入 Docker；要求 MIUI `12.5.6` 底包，Fastboot 下通过命令行运行 `flash-windows.bat /uefi`（勿双击），完成后电脑识别 NCM 网卡，SSH 登录 `172.16.42.1`（`root` / `1234`，内置 `leijun` 熄屏 / `jinfan` 亮屏）。蓝牙、传感器、蜂窝、NFC、摄像头不可用，适合纯无头服务器。

## 3. 红米 K20 Pro：KernelSU 模块静默运行 Ubuntu Chroot + AstrBot

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74595233)（`imgs=5`）
- **工程取舍与调度控制**：放弃开机脚本解压导致的卡顿与 `rm -rf "$CHROOT_LINK"/*` 误删符号链接目标坑点，改为刷入仅准备底包、点模块 Action 再安装，开机仅拉起服务（`127.0.0.1:6185`，约 1GB rootfs + 200+MB 内存）；安装与运行置于 `background` cpuset、`nice 19`、`SCHED_IDLE`、`ionice` 空闲及 `oom_score_adj 300`。
