---
name: android-device-cases
description: 按精确机型、代号和固件读取安卓社区刷机、内核与硬件故障案例，保留已报告范围、缺失材料和验证状态。
---

先读取目标设备的型号、代号与完整固件版本，再选择对应案例。案例中的“作者自测”与“本库复现”分别记录；列入案例库不等于已经证明可以刷入。

- 小米 11、venus、M2011K2C 的自编译内核，读取 [小米 11](references/xiaomi-11.md)。
- K90ProMax、Redmi K90 Pro Max 的特定固件引导加载器修改报告，读取 [K90 Pro Max](references/redmi-k90-pro-max.md)。
- 一加 Ace 6T、ColorOS 17 的临时 Root 报告，读取 [Ace 6T](references/oneplus-ace-6t.md)。
- REDMI Turbo 4 的 ColorOS 17 移植版功能差异，读取 [Turbo 4](references/redmi-turbo-4.md)。
- marble、POCO F5、Redmi Note 12 Turbo 的 Droidian 适配，读取 [marble](references/marble-droidian.md)。
- 红米 K80 Pro 的 AVF/Microdroid 测试差异，读取 [K80 Pro](references/redmi-k80-pro-avf.md)。

- 真我 GT8、原帖版本简称 473/583 的 DTBO 刷新率实验，读取 [GT8 DTBO](references/realme-gt8-dtbo.md)。

- 小米 14 Ultra 的可变光圈异常报告，读取 [镜头故障案例](references/xiaomi-14-ultra-aperture.md)，其中尚无验证过的修复方法。

没有匹配案例时继续检索对应型号，不从同品牌或同 SoC 的案例复制分区写入操作。资料只写大版本、缺少制品或步骤在未读评论中时，明确列为待补条件。

这是进行中的案例库，不是全部机型清单。各案例只保留重新整理后的知识与来源，不分发原帖、工具包或设备身份资料。

- 一加 Ace2 的 OxygenOS 14 历史刷机路径，读取 [Ace2 案例](references/oneplus-ace-2-oxygenos14.md)，先处理缺失的固件、快照与分区条件。
- LG G8 的 Pixel 安卓 14 历史资源，读取 [资源入口](references/lg-g8-pixel-android14.md)，未核验下载与安装。

## NikoHyper OS3.0.315.0.WPBCNXM 移植版

- 小米13/13 Pro/13 Ultra：区分 [Free Kalama](references/nikohyper-free-kalama-os3-315.md) 与 [Fast Kalama](references/nikohyper-fast-kalama-os3-315.md)，两帖标注2026-05-01补丁；Free作者评论给出了完整分享地址及公开提取码 `1145`。
- 小米10/10 Pro/10 Ultra：[Fast Kona](references/nikohyper-fast-kona-os3-315.md) 标注2026-06-01补丁，不套用Kalama的内核/功能记录或分享链接。


## 历史硬件规格

[小米2022机型规格](references/xiaomi-2022-specifications.md)补充Civi2、K50至尊版、MIX Fold2、12S/Pro/Ultra的原帖明确字段与历史起售价；不以宣传词代替测量，也不补填原帖缺失配置。


## 补充来源案例

[一加8系列PE Plus及ColorOS Root进度](references/oneplus8-pe-plus-coloros-root.md)分别保留ROM介绍下载页与Root第二版支持；话题标签不等于适配清单。


## 来源补充

- [Motorola X70 Air解锁与临时引导记录](references/motorola-x70-air.md)


## 补充的来源技术记录

- [天玑机型哔哩哔哩滑动卡顿的版本报告](references/source-53493553.md)
- [一加 Ace 7 Pro：BL 解锁与 Root 刷写步骤（原帖记录）](references/source-74820903.md)
- [联想 Y700 无极：BL 解锁、Root、伪回锁与 TEE 处理](references/source-73470098.md)
- [联想拯救者 Y700 四代：112/117 系统 Root 镜像与 9008 刷机包](references/source-70379642.md)


## 补充技术报告

- [MediaTek 离线 Brom/Preloader 实验记录](references/source-74363250.md)
- [DA2 的版本检查与 heapbait 利用条件](references/source-74964177.md)
- [QC Unlock2 与 RPMB 安全性讨论](references/source-73289046.md)
- [vivo 高通机型免拆 BL 与分区刷写动态记录](references/source-72787881.md)


## 补充来源记录

- [小米相机跨机型移植的配置差异与调试难点](references/source-54019298.md)
- [澎湃 OS 智能助理小部件数据恢复](references/source-53040728.md)
- [在 HyperOS 2 桌面启用 HyperOS 3 搜索框](references/source-66986325.md)


## 补充技术资料

- [荣耀 Magic V5（MBH-AN10）固件条目与下载链接](references/source-68034541.md)
- [9008模式与无法开机排查线索](references/source-43905287.md)

- [NikoHyper 小米 10 与小米 13 移植版本及已知问题](references/nikohyper-kona-kalama.md)

- [HyperOS 小米 17 移植 r17/r17.2：机型、问题与资源](references/hyperos-r17-port-variants.md)

- [NexusHyper v2 移植机型、底包与评论下载入口](references/nexushyper-v2-ports.md)

- [HyperOS r16.2 移植机型、底包与资源](references/hyperos-r16-port-variants.md)

- [HyperOS r16.3/r17 系列机型、版本与资源](references/hyperos-r16-r17-additional.md)
