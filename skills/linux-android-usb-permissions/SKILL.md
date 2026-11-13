---
name: linux-android-usb-permissions
description: 排查 Linux 上 Android ADB/Fastboot 的 USB 权限与规则问题，区分设备模式、基础规则、android-udev 额外规则和未验证的断连归因。
---

# Linux 上的 Android USB 权限排查

[原帖 68865939](https://www.coolapk.com/feed/68865939)报告 Linux 刷回备份时中断，将连接不稳定归因为缺少 `android-udev`。这是个案归因，帖子没有设备型号、发行版版本或复现日志，不能由“能列出设备但刷写失败”确定同一原因。

## 先分清权限与模式

- 在目标模式分别观察 `adb devices` 或 `fastboot devices`；一个模式可见不证明另一个模式的 USB 权限也正确。
- 查看 `lsusb` 中的实际 VID/PID。设备切到其他模式或换移植系统后可能使用不同标识，规则需要覆盖实际标识。
- `adb devices` 的 `no permissions` 指向权限/规则排查，`unauthorized` 则应处理手机上的调试授权，不能混作同一个问题。
- Arch 系发行版的 systemd 提供基础 Android udev 规则；设备未被覆盖时，可按发行版方式安装 `android-udev` 额外规则。规则修改后重新加载并重新连接设备，必要时检查 `adbusers` 组。以上整理依据 [ArchWiki 的 ADB/udev 说明](https://wiki.archlinux.org/title/Android_Debug_Bridge)，本轮未修改主机规则或组成员。

原帖引用的旧说明将基础规则起点写成 systemd v258；本轮读取的 ArchWiki 仍说明基础/额外规则的区别，但不在本库单独验证该历史版本起点。不要因此断言新版 systemd 已覆盖所有设备。

## 原帖命令与资料边界

原文写作 `abd reboot fastboot`，存在命令名拼写问题；不能把它当成原样可执行的 ADB 命令。原帖另提及 `fastboot erase boot`，这是擦除操作，**不是 USB 可靠性测试步骤**，不放入排查流程。

原帖提供的 [中文指南](https://arch.icekylin.online/app/common/android.html)按链接保留，本轮没有读取该页面或其修改记录。没有执行刷写、擦除、安装包、重载规则或设备操作；也没有验证安装额外规则是否解决原作者的中断。
