---
name: android-device-cases
description: 按精确机型、代号和固件读取安卓社区刷机与内核案例，保留已报告范围、缺失材料和验证状态。
---

先读取目标设备的型号、代号与完整固件版本，再选择对应案例。案例中的“作者自测”与“本库复现”分别记录；列入案例库不等于已经证明可以刷入。

- 小米 11、venus、M2011K2C 的自编译内核，读取 [小米 11](references/xiaomi-11.md)。
- K90ProMax、Redmi K90 Pro Max 的特定固件引导加载器修改报告，读取 [K90 Pro Max](references/redmi-k90-pro-max.md)。
- 一加 Ace 6T、ColorOS 17 的临时 Root 报告，读取 [Ace 6T](references/oneplus-ace-6t.md)。
- REDMI Turbo 4 的 ColorOS 17 移植版功能差异，读取 [Turbo 4](references/redmi-turbo-4.md)。
- marble、POCO F5、Redmi Note 12 Turbo 的 Droidian 适配，读取 [marble](references/marble-droidian.md)。
- 红米 K80 Pro 的 AVF/Microdroid 测试差异，读取 [K80 Pro](references/redmi-k80-pro-avf.md)。

- 真我 GT8、原帖版本简称 473/583 的 DTBO 刷新率实验，读取 [GT8 DTBO](references/realme-gt8-dtbo.md)。

没有匹配案例时继续检索对应型号，不从同品牌或同 SoC 的案例复制分区写入操作。资料只写大版本、缺少制品或步骤在未读评论中时，明确列为待补条件。

这是进行中的案例库，不是全部机型清单。各案例只保留重新整理后的知识与来源，不分发原帖、工具包或设备身份资料。
