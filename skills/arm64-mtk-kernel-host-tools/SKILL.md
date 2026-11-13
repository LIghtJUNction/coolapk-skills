---
name: arm64-mtk-kernel-host-tools
description: 处理 ARM64 主机编译 MTK 内核时 x86_64 宿主工具不兼容，区分主机编译成功与目标设备启动。
---

# 在 ARM64 环境编译 MTK 内核时处理 x86_64 主机工具

## 适用范围

原帖以 Redmi K40 Gaming 的 MTK 内核源码 `ares-r-oss` 为例，讨论在 ARM64 环境编译时遇到的主机工具架构问题。发帖中提到的机型只是案例，不代表其他 MTK 设备适配。作者明确表示没有 K40 Gaming 设备，无法验证编译产物能否启动。

## 识别架构不匹配

源码树可能带有 x86_64 预编译工具，例如：

- `scripts/dtc/dtc_overlay`
- `scripts/dtc/ufdt_apply_overlay`
- `tools/build` 下的 `cpio`

这些 x86_64 二进制不能直接作为 ARM64 主机上的可执行工具使用。应先检查源码树对应版本的工具源码，并优先按该版本重新编译 ARM64 主机工具；原帖举例指出 `scripts/dtc` 和 `tools/build` 下分别有相关源码。

原帖没有给出具体编译脚本或完整命令，需自行按源码版本和构建系统确认，不能把其他版本的步骤直接套用。

## 原帖提及的替代做法及限制

作者早期提到两种临时替代思路：为 ARM64 的 `cpio` 创建软链接，或使用 GNU cpio 重新准备 ARM64 版本并放入源码树、设置可执行权限（原帖提及 `chmod 755`）。这些是作者的替代建议，不构成工具 ABI、参数或版本兼容保证；GNU cpio 也不一定能替代源码树所需的特定实现。

## 验证边界

成功在 ARM64 主机编译，不等于内核能在目标设备启动。原作者没有目标设备，无法完成开机验证；对 MTK 设备操作应先确认对应源码、配置和设备支持情况。

## 来源

Coolapk 原帖：[酷安技术帖](https://www.coolapk.com/feed/53270823)

源码示例：<https://github.com/MiCode/Xiaomi_Kernel_OpenSource/tree/ares-r-oss>

原帖提及的视频：https://b23.tv/nDeeFZz
