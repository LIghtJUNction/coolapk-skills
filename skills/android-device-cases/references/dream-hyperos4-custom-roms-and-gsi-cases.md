# 追梦官改 HyperOS 4.0、HackiOS 双引导与冷门机 GSI/随身 WiFi 案例

整理自酷安社区发布与实测记录（未读配图与外部网盘包，未做真机验证）。

## 1. 追梦官改 HyperOS 4.0 ROM 系列功能与适配机型

- **覆盖版本与来源**：
  - Redmi Turbo 5 Max（`4.0.3.0`）/ K90 Max（`4.0.5.0`）：[酷安原帖](https://www.coolapk.com/feed/74646405)
  - 小米 18 Pro / 18 Pro Max（`4.0.27.0`）/ K90 Ultra（`4.0.4.0` / `4.0.6.0`）：[4.0.4.0](https://www.coolapk.com/feed/74646121)、[4.0.6.0](https://www.coolapk.com/feed/74760082)
  - Redmi K100 Pro Max / K100 Pro / K90（`4.0.5.0`）：[酷安原帖](https://www.coolapk.com/feed/74646037)
  - 小米 17 Max（`4.0.3.0`）/ 小米 17 Ultra / K90 Pro Max（`4.0.5.0`）：[酷安原帖](https://www.coolapk.com/feed/74645920)
  - 小米 17 / 17 Pro / 17 Pro Max（`4.0.5.0`）：[酷安原帖](https://www.coolapk.com/feed/74645851)
  - 小米 15（`4.0.0.22`）/ 15 Pro（`4.0.0.19`）/ 15 Ultra（`4.0.0.18`）：[酷安原帖](https://www.coolapk.com/feed/74645799)
  - 小米平板 9 / 平板 8（`4.0.11.0`）/ 平板 8 Pro（`4.0.5.0`）：[酷安原帖](https://www.coolapk.com/feed/74645542)
- **共同特性与机型例外**：分纯净版与高级版，保持 Data 加密与 EROFS 分区、支持官方保数据升级（注：天玑机型不支持官方保数据）、卡线刷二合一（骁龙高通部分系列及平板 6/小米 17/K90/K100 内置 TWRP），刷机可选无 Root / Magisk Alpha / KSU / KSU-Next / SukiSU Ultra / APatch / ReSukiSU；注 `17 Pro / 17 Pro Max` 因破解主题壁纸会导致背屏应用失败而暂取消主题壁纸破解。

## 2. `HackiOS`（Project Treble 双引导）、冷门机 GSI 与随身 WiFi

- **`HackiOS` 非临时启动双系统**：[酷安原帖](https://www.coolapk.com/feed/74786621)（`imgs=6`，已在 Infinity X3.12 与龟龟澎湃 4.0 测试）— 支持通过 Root 模块或「音量上 + 电源键」切回 iOS 且不丢数据。
- **梵沐 PF31（`MT6750V`）LineageOS 18.1 `phhGSI`**：[酷安原帖](https://www.coolapk.com/feed/75044318)（`imgs=7`）— 移植原厂 FM 收音与 MTK `logo` 关机充电动画，修复最小宽度 341 下锁屏日期省略号及 WLAN“无法访问互联网”提示。
- **小米 CC9e GSI 无法联网/变砖排障**：[酷安原帖](https://www.coolapk.com/feed/74201950)（`imgs=2`）— 作者记录以国行 MIUI 11 正式版为底包刷 GSI 无法联网，改用 EU 版 MIUI 底包后成功启动。
- **影腾 `uz901 v1.6` 随身 WiFi 自改包**：[酷安原帖](https://www.coolapk.com/feed/74313721)（`imgs=5`）— 去控并移植 Web 与 AT 功能，强提示刷前必须核对固件版本号完全一致，否则变砖需编程器救砖。
