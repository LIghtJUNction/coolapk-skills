# Marble 刷入 Linux（Droidian）

来源：原帖 https://www.coolapk.com/feed/74340899

## 系统与设备支持

Droidian 是 Debian 分支，使用 Phosh 和 Wayland。可安装 Debian 软件包（`.deb`）；桌面界面可能需要适配，原帖提到可以强制调整应用 scale，但没有提供具体设置方法或参数。

原帖称，此设备使用的非主线 Linux 内核版本为 **5.10.236**，对 Marble 适配较好。原帖发布时称教程已更新，设备可以正常刷入和使用。以下硬件与功能支持情况均为原帖报告：Wi-Fi、蓝牙、麦克风、扬声器、前后摄像头、NFC、MTP、快充，以及 Waydroid Android 应用。

原帖提到可安装 WorkBuddy 桌面端、CC Switch、Docker、OpenClaw，也可安装 Waydroid。Waydroid 环境可能需要修复；部分应用会因检测到 root 而拒绝运行，原帖建议采用 Magisk Waydroid 方案隐藏环境。原帖未提供这些软件的安装命令、版本或修复步骤。

支持插卡单卡使用；原帖指出必须重启 `ofono.service` 和 `ModemManager.service` 才能使用，但没有给出重启命令。

## 刷入步骤

目标设备：Redmi Note 12 Turbo（Marble）。以下为原帖给出的流程；原帖未提供 BL 解锁及“双清”的具体操作细节。

1. 解锁 BL，并执行双清。
2. 刷入 HyperOS 3 ROM 包；原帖建议优先使用线刷。
3. 系统包刷完后进入 recovery。下载并安装 **DFE NEO v2 2.6.0 BETA 1**。原帖强调必须使用文件名为 `arm64-v8a-en-DFE-NEO-2.6.0-beta-1.zip` 的包：先通过 ADB 将包推送到 `/tmp`，再在 recovery 的 Install 中选择该包，并按照项目原帖中的选项操作。随后双清，再次重启到 recovery（不是系统）。

   推送命令可据原帖描述写为：

   ```sh
   adb push arm64-v8a-en-DFE-NEO-2.6.0-beta-1.zip /tmp
   ```

   此命令中的本地文件路径需按下载位置调整；原帖只指定了设备端目录 `/tmp`。
4. 在 recovery 中打开 ADB sideload，将 Droidian ZIP 包 sideload 到设备。原帖没有提供该 ZIP 的文件名或 sideload 命令。
5. 如果重启后无法进入系统，返回 fastboot 刷入 `boot.img`。原帖称刷入 boot 后即可进入 Droidian，但没有给出 boot 镜像的下载地址或 `fastboot` 命令。

## 双系统与资料边界

该项目支持双系统启动。作者个人认为没有必要，倾向于直接在 Droidian 中使用 Android 环境；这属于个人使用取舍，不是刷机要求。

原帖中的“详细请看查看链接”“教程原帖：查看链接”没有在采集正文中展开成可读 URL；图片可能包含更多说明，但图片内容未提供，故不据此补充步骤或参数。Droidian ZIP、`boot.img`、DFE NEO 包的实际下载位置及 recovery 选项应查阅下列公开项目与教程页面。

## 公开项目与教程

- Droidian Marble 适配项目：https://github.com/droidian-marble/adaptation-xiaomi-marble
- Marble Droidian 教程：https://halhadus.rocks/projects/droidian-for-marble.html

## 项目说明的历史核对

2026-11-14重新核对[项目README](https://github.com/droidian-marble/adaptation-xiaomi-marble)时，指纹、自动亮度和双SIM列为不可用，双SIM注明已禁用。这与社区笼统概述的“功能能用”需要分开记录；不是本库实机测试，也不代表此后项目状态。项目身份为POCO F5 / Redmi Note12 Turbo，具体固件和恢复材料仍需按设备核对。
