---
name: droidspaces-steamos-arm64-reference
description: 整理 DroidSpaces SteamOS ARM64 的手柄输入、Steam 分支、Decky ARM64 下载、systemd 会话、性能 HUD 和启动动画实验记录。
---

# DroidSpaces ARM64 SteamOS 文档与工具导航

## 用途

这是面向 SteamOS ARM64 容器（DroidSpaces）的双语文档与工具集目录，帮助读者按主题查找项目资料。原帖仅列目录；现已进一步读取仓库八篇中文文档并整理为下列两份参考，保留实际命令、参数和失败记录。仓库自述为 AI 生成，这些属于社区实验资料，尚未在本机或目标手机复现。

## 作者记录的参考环境

- 手机：OnePlus Ace 3
- SoC：骁龙 8 Gen 2
- GPU：Adreno 740
- 手机系统及权限环境：Android + Magisk 或 SukiSU
- 容器：SteamOS ARM64

这些是作者记录的参考环境，不代表所有 ARM64 设备或 Linux/SteamOS-like 环境都适用，也不能据此推断文档中的步骤已经在其他设备验证。

## 文档目录

以下路径均相对于项目仓库根目录：

1. 手柄识别与映射（核心）：`docs/01-gamepad-mapping.zh-CN.md`
2. Steam 各版本差别与缺陷（核心）：`docs/02-steam-versions-and-defects.zh-CN.md`
3. ARM64 上安装 Decky Loader：`docs/03-decky-loader-arm64.zh-CN.md`
4. 容器 systemd 与会话：`docs/04-container-systemd.zh-CN.md`
5. Magisk 输入隔离模块：`docs/05-magisk-module.zh-CN.md`
6. 性能 HUD / Deck 模式：`docs/06-performance-hud-and-deck-mode.zh-CN.md`
7. Steam 启动动画：`docs/07-steam-startup-animation.zh-CN.md`
8. 排障记录：`docs/08-troubleshooting-log.zh-CN.md`

## 获取项目资料

仓库入口：https://github.com/chickenisbeautiful/droidspaces-arm64-solutions

打开仓库后按上面的相对路径查阅对应文档。按问题查阅：[手柄、Steam 分支、Decky ARM64 与 systemd](references/input-steam-decky-systemd.md)；[输入隔离、性能 HUD、动画与排障](references/isolation-hud-animation.md)。路径、用户名、UID、设备节点和旧版构建号都需按实际环境核对。

## 来源

- 项目仓库：https://github.com/chickenisbeautiful/droidspaces-arm64-solutions
- 原帖：[酷安技术帖](https://www.coolapk.com/feed/74600974)
