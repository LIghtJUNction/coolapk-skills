---
name: hyperos-rom-port-debugging
description: 按设备条件、分区和故障分析 HyperOS 3 移植记录，查找具体属性、APK 与 overlay 路径、指纹坐标示例、缺失依赖和互斥方案。适用于米系联发科与 OPlus 移植案例的资料整理和离线排障。
---

# HyperOS 移植故障：从设备条件定位修改项

先确认目标设备、原厂底包、移植源、Android 基础版本及故障日志，再定位文件。这里的两组社区记录没有完整固件版本清单，也未做独立实机复现；一加 13 的指纹坐标与米系联发科的配置不能跨机型直接套用。

## 分区与故障入口

| 目标 | 已记录的具体入口 | 详解 |
|---|---|---|
| DPI、模糊、桌面入口 | `/product/etc/build.prop` 的 `ro.sf.lcd_density`、`persist.miui.density_v2`、`persist.sys.background_blur_*` 与 `ro.config.miui_desktop_mode_enabled` | [米系联发科记录](references/mediatek-hyperos3.md) |
| 设备特性与刷新率菜单 | `/product/etc/device_features/` 中的设备代号、`fpsList`、`support_max_fps`；值须来自实际面板能力 | 同上；[OPlus 记录](references/oplus-port-cases.md)另列分辨率菜单 |
| 人脸、语音、相机和界面 | `Biometric`、`VoiceTrigger`、`MiuiCamera.apk` 及设备 overlay；来源 APK 的版本、权限和依赖需要成套核对 | [具体路径和 8 项语音权限](references/mediatek-hyperos3.md) |
| 工作台和性能分级 | `MiuiDesktopModeTile.isAvailable()`、`SettingsFeatures`、`framework-ext-res.apk` 的 CPU/GPU 列表、`MiuiBooster.jar` 的 RAM 阈值 | [方法名与阈值](references/mediatek-hyperos3.md) |
| 指纹、触感和三段键 | 超声波与光学短焦分别配置；光学打光节点 `/sys/kernel/oplus_display/notify_fppress`；按键节点 `/proc/tristatekey/tri_state` | [两组坐标、路径及缺口](references/oplus-port-cases.md) |
| 自动亮度与开机偏暗 | `displayconfig`、`dumpsys display`、`AospFrameworkResOverlay.apk` 和 `MiuiFrameworkResOverlay.apk` | [原帖明确互斥的方案](references/oplus-port-cases.md) |
| NFC 与语音唤醒失败 | 区分 NFC 芯片和文件组合；语音唤醒曾出现重启及升级后失效 | [ST54L 参数、缺失文件与失败记录](references/oplus-port-cases.md) |

## 整理和排查方法

1. 在解包工作副本中对照原机文件定位差异，记录原有 owner、mode 和 SELinux 元数据。不要把统一 `755/644` 或 JSON `777` 当作跨分区权限规则。
2. 分清属性开关、菜单显示、框架代码、HAL/驱动和实际硬件能力。修改设备名称、RAM 分类或刷新率菜单不会增加内存或面板模式。
3. 一次分析一类故障，把原帖声称、当前日志和实测结果分别记录。互斥的亮度方案不能合并；同一个属性在不同指纹方案中的值也不能混用。
4. 保留原始键名与拼写疑点。`fasle`、`dobly` 等原文值尚未核实；`O1_asic` 与 `O1-asic` 也不能自动视为同一个代号。
5. 依赖不全时列出材料缺口：Rust 运行时缺具体库清单；`devinfonew`、`XMNfcNci.zip`、`人脸指纹入口.zip` 等缺来源包或下载链接。保留实际文件名，不编地址、密码或兼容性。
6. 区分 APK 最低签名方案版本和完整签名、证书验证。原帖的 `getMinimumSignatureSchemeVersionForTargetSdk` 修改不能称为取消全部校验，详见联发科记录中的 AOSP 对照。

两份参考包含具体键值、路径、方法、失败现象和资源缺口；无需私有采集编号或数据库即可阅读。


## 补丁工程的当前文档

另读了 OnePlus15-Port-Script 的当前 README，整理为[补丁工作目录、元数据与失败语义](references/patch-workspace.md)。它描述显式选择模块、底包与原包分区映射、身份快照和单补丁回退；不能反推旧版 ROM 包已包含全部当前模块。

## 结构对比线索

- [ROM 二次打包对比中提到的文件与组件](references/rom-repackaging-comparison-evidence.md)
