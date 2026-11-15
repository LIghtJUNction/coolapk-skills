# ZRAM、双 ZRAM 与 OEM 后台保活测试记录

本记录整理自 3 篇社区实测与反馈动态，图片与外部模块文件未读取，未在本库实机复现。

## 1. 后台保活模块 4.2.0 与 SCENE 9.1 测试口径 [69682547](https://www.coolapk.com/feed/69682547)

- **测试定义**：使用 SCENE 9.1 保活测试（伪随机应用打开顺序，统计“应用存活数量 / 应用启动数量”）；通过标准为减去 ZRAM 占用后内存使用率达到 `140%+`（原帖说明约相当于非 Root 模式下的 `160%+`），且测试过程未冻结/禁用 `Athena` 等系统自带管理组件。
- **4.2.0 测试版变更**：提升 `moto x70 air`、`meizu 20 pro`、`Xiaomi 15`、`realme gt neo`、`realme gt neo7se` 后台保活能力；支持为 `oneplus 11/12`、`realme gt neo/neo5`、`meizu 20 pro` 等内核低于 `6.6` 且未使用 CHP / 双 ZRAM 的设备配置对应策略。

## 2. 系统版本对 ZRAM 与后台策略的反馈

- **一加 12（ColorOS 16）**[69586379](https://www.coolapk.com/feed/69586379)：原帖报告更新 ColorOS 16 后双 ZRAM 被废弃，拓展内存可正常生效。
- **小米 15（HyperOS 3 vs HyperOS 2）**[68557160](https://www.coolapk.com/feed/68557160)：原帖报告 8G / 12G 内存老机型升级 HyperOS 3 后杀后台更激进，且流畅度与 HyperOS 2 差异不大。
