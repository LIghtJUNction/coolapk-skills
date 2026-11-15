# KernelSU-Next / SukiSU / FinaSU AnyKernel3（5.10–6.18）与内核版本隐藏

整理自酷安社区开发者发布记录（未读配图与外部网盘/GitHub 发布文件，未做实机刷入验证）。

## 1. 通用与一加 AnyKernel3 构建版本索引

| 分支与版本 | 覆盖内核/系统 | 通用 AK3 特性 | 一加 AK3 特性 | 来源 |
|---|---|---|---|---|
| `KernelSU-Next-33355` | 5.10–6.18（A12–16） | ZRAM 增强、BBG 防格机、SUSFS `v2.4.0`（官方无内置时配合 SUSFS/KPN 模块） | — | [酷安原帖](https://www.coolapk.com/feed/75007606) |
| `SukiSU-41150` | 5.10–6.18（通用 A12–16 / 一加 A15–16） | ZRAM 增强、BBG、SUSFS `v2.3.0` | BBG、BBR、SUSFS `v2.3.0` | [酷安原帖](https://www.coolapk.com/feed/74883625) |
| `SukiSU-41140` | 5.10–6.12（通用 A12–16 / 一加 A15–16） | 同上 | 同上 | [酷安原帖](https://www.coolapk.com/feed/74853607) |
| `SukiSU-41100` | 5.10–6.12（通用 A12–16 / 一加 A15–16） | 含 KPM 功能 | BBG、BBR、SUSFS `v2.3.0` | [酷安原帖](https://www.coolapk.com/feed/74714849) |
| `FinaSU-33155` | 5.10–6.12（通用 A12–16 / 一加 A15–16） | ZRAM 增强、BBG、SUSFS `v2.3.0`（KPM 需 KPN 模块） | 同上 | [酷安原帖](https://www.coolapk.com/feed/74789367) |
| `FinaSU-33090` | 5.10–6.12（通用 A12–16 / 一加 A15–16） | 同上（自带 0 宽修复） | 同上（自带 0 宽修复） | [酷安原帖](https://www.coolapk.com/feed/74721084) |
| `FinaSU-33084` | 5.10–6.12（通用 A12–16 / 一加 A15–16） | 同上（引用 `5ec1cff/FuseFixer` 0 宽修复） | 同上 | [酷安原帖](https://www.coolapk.com/feed/74706471) |

## 2. 刷入与“内核版本 / 构建日期”隐藏通用流程（按发布者说明）

- **刷入与回退**：刷前务必备份原厂 `boot`（GKI/AK3 修改 `boot`，LKM 修补 `init_boot`，GKI 转回 LKM 需刷回原 `boot`）；LKM 驱动版本不匹配时可在管理器工作页执行「直接安装修补并重启」对齐；AK3 在管理器直刷失败可用爱玩机工具箱等第三方工具刷入。通用核标明适用小米/红米/vivo/iQOO/联想/红魔（不含欧加真/魅族），一加 AK3 基于一加官方内核源码编译且仅支持官方系统。
- **双槽核对隐藏版本串**：用工具（如爱玩机工具箱）将当前 `boot` 镜像刷入对侧槽位（如当前在 `a` 则刷入 `b`），在 `ReKernelFlasher` 读取对侧槽位的内核版本与构建日期字符串后填入 SUSFS 伪装配置。
