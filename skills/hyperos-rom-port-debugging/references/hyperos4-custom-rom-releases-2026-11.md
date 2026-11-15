# 小米 / 红米 HyperOS 4.0 官改 ROM 2026-11 发布记录

本记录整理自 6 篇面向小米与红米机型的 HyperOS 4.0（Android 17）官改 ROM 更新说明，原帖图片与网盘链接未读取，未在本库实机验证。

## 机型与版本清单

| 机型范围 | 标注版本 | 安卓与补丁 | 来源 |
|---|---|---|---|
| 小米 18 Pro / 18 Pro Max；小米 17 Ultra | `4.0.29.0` 正式版；`4.0.6.0` 正式版 | HyperOS 4.0 | [74869249](https://www.coolapk.com/feed/74869249) |
| 小米平板 8 / 9；小米平板 8 Pro | `4.0.12.0` 正式版；`4.0.6.0` 正式版 | HyperOS 4.0 | [74869202](https://www.coolapk.com/feed/74869202) |
| 小米 17 / 17 Pro / 17 Pro Max | `4.0.6.0` 正式版 | HyperOS 4.0 | [74869119](https://www.coolapk.com/feed/74869119) |
| 小米 17 / 17 Pro / 17 Pro Max / 17 Ultra | `OS4.0.6.0` 正式版 | Android 17，安全补丁 `2026-10-01` | [74836773](https://www.coolapk.com/feed/74836773) |
| REDMI K90 Ultra；K90 Pro Max / K90 | `4.0.8.0` 正式版；`4.0.6.0` 正式版 | HyperOS 4.0 | [74869073](https://www.coolapk.com/feed/74869073) |
| 红米 K100；REDMI K100 Pro；K100 Pro Max | `4.0.8.0`；`4.0.6.0`；`4.0.7.0` 正式版 | HyperOS 4.0 | [74869019](https://www.coolapk.com/feed/74869019) |

## 共性特性与分支差异

- 均提供**纯净版**（无高级设置，接近官方体验）与**高级版**（含高级设置）两种分支。
- 分区与升级条件：保留 `data` 加密与 `erofs` 分区格式，支持官方保数据升级刷入；原帖特别注明**天玑平台不支持官方保数据升级**。
