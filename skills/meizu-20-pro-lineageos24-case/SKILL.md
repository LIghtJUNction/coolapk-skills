---
name: meizu-20-pro-lineageos24-case
description: 查询魅族20Pro m2391非官方LineageOS24的20261016构建、Flyme固件记录、功能变化和下载。
---

# MEIZU 20 Pro（m2391）：非官方 LineageOS 24

## 版本信息

帖子将此版本描述为 LineageOS 24、Android 17，构建日期为 2026-10-16；帖子列出的固件版本为 Flyme 12.6.0.0A。ROM 可不安装 Google 应用使用；如需 Google 服务，帖子建议自行安装与系统版本匹配的 GApps，但没有提供 GApps 下载地址。

LineageOS 是基于 Android 开源项目（AOSP）的社区开源系统。基础源码可见 [LineageOS GitHub](https://github.com/LineageOS)，贡献入口为 [Gerrit Code Review](https://review.lineageos.org)。

## 帖子报告的功能状态

作者列出的正常使用项目包括：相机、指纹识别、蓝牙、高刷新率、AOD 息屏显示、NFC、WLAN、双击唤醒（DT2W）、防误触、振动、自动亮度、抬手亮屏、VoLTE / IMS，以及 SELinux Enforcing。

作者称当时“暂无已知问题”，并欢迎反馈。这只是作者在帖子中的报告，不保证没有 Bug；本文没有进行实机验证。

## 2026-10-16 更新内容

- **音频：**音频服务、HIDL 接口实现及配套工具库由源码编译，并随 AOSP 更新。
- **蓝牙：**补齐 A2DP 音频输出。
- **显示：**默认刷新率调整为 120 Hz，修复 ADFR。
- **指纹：**补齐 IFAA、Soter 组件，修复指纹权限。
- **Recovery：**切换为 1080p 显示。
- **内核：**固定使用 `clang-r450784e` 分支构建。

## 下载

帖子提供的 SourceForge 下载地址：

<https://sourceforge.net/projects/meizu-m2391-lineage/files/24.0/lineage-24.0-20261016-UNOFFICIAL-m2391.zip/download>

帖子没有提供安装步骤，因此本文不补写刷机命令。下载链接仅按帖子记录；没有下载文件或验证设备兼容性。

## 来源

- [Coolapk 帖子 74396751](https://www.coolapk.com/feed/74396751)
