---
name: android-change-triage
description: 审核安卓刷机、Root、OTA 与救砖教程的适配条件和恢复路径；用于把社区案例转成具体设备的操作方案。
---

先确认设备型号与代号、系统完整版本、基带和引导加载器状态，再判断教程是否适配。不能用营销名相近或 Android 大版本相同代替适配证据。

1. 读取当前设备信息及教程的明确适配范围。标出原帖自测、作者推测、未解答求助和自己的推断。
2. 列出会改变的分区、是否清数据、降级限制、原始镜像来源以及能否进入恢复环境。没有匹配恢复材料时，不从其他机型复制写分区命令。
3. 先验证备份可读，再把具体操作、成功标志和失败后的恢复步骤写在一起。
4. 完成后验证启动、通信、指纹、分享等本次变更可能影响的功能。单纯成功开机不能证明功能完整。

## 阅读依据与边界

- [三星升级案例](https://www.coolapk.com/feed/71072046)：正文限制特定起始版本，并报告回退清数据及分享功能故障。提炼为“完整构建号与回退代价必须保留”，不把该帖子当作当前刷机指南。
- [路由固件案例](https://www.coolapk.com/feed/69083165)：同型号不同固件版本存在适配差异；仅借用版本判断方法，不沿用其镜像。
- [指纹故障求助](https://www.coolapk.com/feed/74255214)：只有症状与猜测，没有已验证修复结论。不能生成所谓“恢复工具修复指纹”的确定操作。
- [特定设备引导加载器修改案例](https://www.coolapk.com/feed/74915679)：正文包含清数据及写分区操作，并把其他系统版本写成理论兼容。仅提炼“自测条件不能扩大为全版本支持”；相对界面位置、字节值与该案例的内核选择见参考，不转成跨设备通用偏移。

2026-11-13 已读以上文字正文；图片、评论及外链未读，未实机复现。涉及实际命令时另查对应项目当前文档与设备支持说明。


## 具体案例与公开资源

[按设备区分的 Android 更新与解锁案例](references/model-specific-update-cases.md)保留原帖已提供的数值、版本、操作与资源入口；均按社区报告和具体条件理解。

- [欧加真系 SukiSU Ultra / AK3 Root 与环境隐藏流程线索](references/oplus-sukisu-ak3-root-workflow.md)
- [Recovery、Cube 恢复与双机刷机/Root 实操案例](references/recovery-cube-dual-phone-flashing-cases.md)
- [第三方 ROM（EvoX / LineageOS 24）与 TWRP 16.0 适配记录](references/custom-rom-and-twrp16-port-cases.md)
- [特定机型刷机、解工厂锁、GSI、9008 救砖与无障碍读屏实测](references/legacy-factory-unlock-gsi-9008-and-accessibility.md)
- [EXT4 / EROFS 镜像格式解析与一加 Ace 6T 风驰内核调校](references/ext4-erofs-formats-and-ace6t-tuning.md)
- [阿权 Lite 官改 ROM 刷机规范与华为 P30 短接 COM 1.0 降级解锁流程](references/aquan-lite-rom-and-p30-com10-downgrade.md)
