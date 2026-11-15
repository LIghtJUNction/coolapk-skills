# 华为 MatePad 11（2021 / 骁龙 865）主线 Linux 7.3.1 适配实录

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74987806)（`imgs=1`）
- **仓库**：`https://github.com/CoWave-Fall/linux-on-arm-matepad11`
- **基础软件栈**：
  - 引导与内核：`U-Boot 2026.10` + 主线 `Linux 7.3.1`
  - 发行版与桌面：`Arch Linux ARM` + `KDE Plasma 6.8.2`（Wayland 会话）
- **已记录可用硬件能力**：
  - **显示与触控**：双通道 DSI（1600×2560，支持 30/60/120 Hz 切换）；五点触控、双指手势、防误触及休眠唤醒恢复。
  - **背光、供电与功耗**：`RT8555` 驱动支持 10-bit 平滑调光；支持读取电池容量/充放电电压/电流/温度；三档电源模式映射至 CPU Governor 与 GPU 频率阶梯。
  - **无线与外设**：`QCA6391`（`ath11k` + `BlueZ`），官方磁吸键盘与 M-Pencil 3 可正常配对使用。

> 证据边界：未读取配图、评论与 GitHub 仓库最新提交，未在真机复现构建与引导。
