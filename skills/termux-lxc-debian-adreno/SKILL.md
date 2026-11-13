---
name: termux-lxc-debian-adreno
description: 查阅 K40p/骁龙888 上 Termux 与 Debian 12 LXC 的配置笔记，包含 X11 临时目录、FUSE、Adreno/DRM/ION 透传、命令和原帖配置纠错。
---

# Termux：Debian LXC、X11 与 Adreno 透传

用于已有支持 LXC 内核及工具的设备。按宿主命令、容器配置和容器内命令分别查阅；设备主次号、容器名与 X11 显示号需要一致。原帖不是从零安装教程，也没有证明所有 Adreno 设备都已硬件加速。

详见 [配置、命令与已核对的差异](references/details.md)。

来源：[原帖](https://www.coolapk.com/feed/69224474)。整理日期：2026-11-13。此处记录原帖环境与行为，未做实机验收；尚未读取全部图片、评论和外链目标。
