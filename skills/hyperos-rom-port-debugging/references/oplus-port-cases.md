# HyperOS 3 移植实验记录：按故障整理的处理线索

> 本文重组自原帖，记录作者环境中的移植实验方法与观察，不构成通用修复，也未经独立验证。指纹坐标示例来自一加 13；属性、资源和目录布局需要以目标 ROM 与设备实际内容为准。原帖没有提供固件版本、网盘链接或可下载文件，以下所列资源文件名无法据此取得。

## 引导与桌面启动

### 卡在引导

原帖建议补齐小米账号相关文件，并以 `mtd` 为关键词查找；另提到替换用于跳过引导的文件，但没有给出文件名、来源或下载链接。因此，替换文件的具体内容和适用版本缺失。

### 桌面崩溃

作者记录的尝试是在 `odm/build.prop` 加入以下属性：

```properties
ro.miui.region=cn
ro.miui.product.home=com.miui.home
ro.apex.updatable=true
```

这只是原帖中的环境配置记录，不代表这些属性能独立解决其他 ROM 的桌面问题。

## 设备身份与本地设备参数

### 机型识别卡住

原帖通过 `odm/build.prop` 设置产品和认证字段。`机型代号`、`设备名称`、`系统版本`、`机器型号` 均为需要按目标设备填写的占位说明，不是可直接使用的值：

```properties
ro.product.odm.brand=Xiaomi
ro.product.odm.device=机型代号
ro.product.odm.manufacturer=Xiaomi
ro.product.odm.marketname=设备名称
ro.product.odm.name=机型代号
ro.product.brand_for_attestation=Xiaomi
ro.product.name_for_attestation=机型代号
ro.product.device_for_attestation=机型代号
ro.product.manufacturer_for_attestation=Xiaomi
ro.odm.build.version.incremental=系统版本
ro.product.manufacturer=Xiaomi
ro.product.brand=Xiaomi
ro.product.model=机器型号
ro.product.name=机型代号
```

这些字段会影响设备身份呈现；原帖没有提供具体目标机型取值或固件版本。

### 本地设备参数文件

原帖记录的文件位置与建议元数据如下：

| 路径 | 原帖记录 |
|---|---|
| `/system/bin/devinfonew` | mode `755` |
| `/system/etc/init/devinfonew.rc` | mode `644` |
| `/product/etc/device_features/QQQiu_devInfoNew.json` | 原帖建议 `777`，并称 owner/group 均为 `root`；要求保留文件名和 JSON 格式 |

这些权限说法是原帖针对其环境的记录，不是跨分区通用设置。整理或移植时应保留原始 owner、mode 与 SELinux 元数据；不能据此对整目录统一 `chmod`。

原帖还提到编辑 `/system/priv-app/HTMLViewer` 内的 DEX，搜索 Base64 字符串 `aHR0cHM6Ly9wb3J0LnNlYy5taXVpLmNvbQ==`，并将其改为 `1` 后保存，注明“不签名”。原文没有说明 APK 的准确文件名、工具版本或修改后验证方式，因而此处只保留为作者记录的实验步骤。

## 指纹、触感与按键

### 超声波指纹识别入口与坐标

原帖针对超声波指纹列出以下 `odm/build.prop` 属性，并说明坐标及尺寸需要自行调整；示例以一加 13 为例：

```properties
ro.hardware.fp.fod.c=true
ro.hardware.fp.fod=true
persist.vendor.sys.fp.vendor=true
persist.vendor.sys.fp.fod.location.X_Y=628,2412
persist.vendor.sys.fp.fod.size.width_height=184,184
persist.vendor.sys.fp.fod.us.target=616,2388,824,2616
persist.vendor.sys.fp.fod.delay.fingerdown.ms=20
```

这些坐标和尺寸只应视为该示例的实验值，且需目标面板与指纹硬件本身支持。

### 设置页触感开关

原帖的操作对象是 `system_ext/priv-app/Settings.apk`：使用 DEX++ 编译，定位类 `com/android/settings/utils/SettingsFeatures` 中的方法 `isSupportSettingsHaptic(Landroid/content/Context;)Z`，把 `const/4 v0, 0x0` 改为 `const/4 v0, 0x1`。作者称此举用于恢复设置中的触感选项界面；原帖没有记录 APK 重打包和运行验证细节。

### 指纹打光：光学短焦实验

原帖描述通过内核节点 `/sys/kernel/oplus_display/notify_fppress` 写入 `0` 和 `1` 控制打光，并给出以下属性供其环境尝试：

```properties
persist.vendor.sys.fp.vendor=goodix
ro.hardware.fp.fod.c=true
ro.hardware.fp.fod=true
persist.vendor.sys.fp.fod.delay.fingerdown.ms=20
ro.hardware.fp.fod.touch.ctl.version=1.0
ro.hardware.fp.mievent=true
persist.vendor.sys.fp.fod.location.X_Y=509,2296
persist.vendor.sys.fp.fod.size.width_height=199,199
```

原帖称这些属性可补入任意 `build.prop`，但没有给出具体适配分区、节点权限或目标设备验证记录；坐标值也不能视为通用值。

### 三段式按键

原帖指出 OPlus 三段式开关状态可从 `/proc/tristatekey/tri_state` 读取，不同拨片位置返回三个数字，并建议自行编写脚本处理。没有给出这三个返回值的映射或脚本实现。

### 线性震动

原帖记录开机后执行：

```sh
setprop sys.haptic.motor linear
```

也给出在任意 rc 文件中于启动完成后设置属性的片段：

```rc
on property:sys.boot_completed=1
    setprop sys.haptic.motor linear
```

随后还需补入原包中含 `sys.haptic` 内容的属性；原帖未列明完整属性集合，并另称删除 `sys.haptic.motor=linear` 和 `sys.haptic.version=2.0`。这些描述之间的配置关系未解释完整，故不能推定缺失属性或整理成可直接套用的配置。

## 显示、亮度、分辨率与刷新率

### 144 Hz 与分辨率菜单

原帖建议在 `/product/etc/device_features/机型代号.xml` 找到 `120` 对应项并改为 `144hz`，另在机型配置中加入：

```xml
<integer-array name="screen_resolution_supported">
    <item>1264</item>
    <item>1080</item>
</integer-array>
<integer-array name="fpsList">
    <item>144</item>
    <item>60</item>
</integer-array>
```

`1264`、`1080`、`144` 和 `60` 是原帖列出的配置值，不代表面板、显示驱动或设备硬件具备这些模式。软件属性不能增加设备本身不支持的刷新率或分辨率。

### 护眼色彩

原帖只给出方向性思路：补齐 `odm` 和 `vendor` 中的 `displayfeature`、依赖文件与驱动文件，再补充 prop 条目或 AOSP 接口。没有给出文件清单、来源包、版本或具体接口实现，内容仍未完成，不能据此补造依赖。

### 自动亮度与开机亮度偏暗：两种互斥处理

原帖把下面步骤作为自动亮度实验方案：

1. 注释 `odm` 分区 `build.prop` 中的 `ro.vendor.oplus.sensor.high_pwm_rgb=true`。
2. 将 `vendor/etc/displayconfig` 下的文件复制到 `product/etc/displayconfig`。
3. 执行 `dumpsys display`，找到 `uniqueId='local:屏幕id'` 中的屏幕 ID。
4. 从刚复制到 `product/etc/displayconfig` 的文件中选取体积最大的那个。原帖此处只说改成取得的屏幕 ID，未明确编辑内容还是改文件名；后续段落明确给出了 `display_id_屏幕id.xml` 文件名格式，不能擅自补出 XML 内部字段。

另一个独立处理是删除 `product/overlay` 下的 `AospFrameworkResOverlay.apk` 和 `MiuiFrameworkResOverlay.apk`，原帖称其用于修复开机动画暗。原帖明确说，这个开机偏暗处理与自动亮度方案不能同时使用；不能将两者合并。

### 刷新率相关属性集合

原帖提供了一个混合音频、触控、色彩、面板和刷新率行为的长属性集合。它并非全部都是刷新率开关。为避免将其中每项误称为刷新率参数，按功能线索分组保留原始键值；原帖未定义或存在拼写疑点的键值照录：

| 原帖属性 | 原值 | 分组说明 |
|---|---:|---|
| `ro.vendor.touchfeature.type` | `183` | 触控 |
| `ro.vendor.audio.spatializer.support.speaker` | `true` | 音频空间效果 |
| `vendor.audio.useXiaomiSpatializer` | `true` | 音频空间效果 |
| `audio.spatializer.pose_predictor_type` | `2` | 音频空间效果 |
| `audio.spatializer.prediction_duration_ms` | `50` | 音频空间效果 |
| `persist.vendor.audio.effectimplenter` | `dolby` | 音频；原帖键名拼写为 `effectimplenter`，含义未核实 |
| `persist.vendor.audio.spatializer.enable` | `false` | 音频空间效果 |
| `ro.vendor.audio.dolby.spatial.profile` | `dynamic` | 音频空间效果 |
| `ro.vendor.audio.karaoke.enable` | `true` | 音频 |
| `ro.surface_flinger.enable_frame_rate_override` | `false` | SurfaceFlinger 帧率覆盖 |
| `ro.vendor.colorpick_adjust` | `true` | 色彩 |
| `ro.vendor.all_modes.colorpick_adjust` | `true` | 色彩 |
| `ro.vendor.display.type` | `oled` | 显示类型 |
| `ro.vendor.xiaomi.bl.poll` | `true` | 背光相关 |
| `ro.vendor.histogram.enable` | `true` | 显示处理 |
| `ro.vendor.cabc.enable` | `fasle` | CABC；原帖值拼为 `fasle`，不校正 |
| `ro.vendor.bcbc.enable` | `true` | 显示处理 |
| `ro.vendor.fps.switch.default` | `true` | 刷新率切换 |
| `ro.vendor.display.default_fps` | `60` | 默认刷新率 |
| `ro.vendor.sre.enable` | `true` | 显示处理 |
| `ro.vendor.gcp.enable` | `1` | 显示处理 |
| `ro.vendor.localhbm.enable` | `true` | 局部高亮 |
| `ro.vendor.video_box.version` | `2` | 视频显示处理 |
| `ro.vendor.soft_backlight.enable` | `true` | 背光 |
| `ro.vendor.hbm_backlight.enable` | `true` | 高亮背光 |
| `ro.vendor.display.nature_mode.enable` | `true` | 显示模式 |
| `ro.vendor.display.ai_disp.enable` | `true` | 显示处理 |
| `ro.vendor.standard.video.enable` | `true` | 视频显示处理 |
| `vendor.display.use_layer_ext` | `0` | 显示合成 |
| `ro.vendor.display.mi_calib.enable` | `true` | 显示校准 |
| `ro.vendor.thermal.dimming.enable` | `true` | 温控调光 |
| `sys.displayfeature_hidl` | `false` | DisplayFeature 接口 |
| `sys.displayfeature.entry.enable` | `true` | DisplayFeature 入口 |
| `persist.vendor.dc_backlight.enable` | `false` | DC 调光 |
| `persist.vendor.dc_backlight.threshold` | `0` | DC 调光阈值 |
| `vendor.display.enable_fb_scaling` | `1` | 显示缩放 |
| `ro.vendor.display.touch.idle.enable` | `true` | 触控空闲策略 |
| `ro.vendor.display.idle_default_fps` | `60` | 空闲刷新率 |
| `ro.vendor.display.idle_default_fps.support` | `true` | 空闲刷新率支持标记 |
| `ro.vendor.display.fod_monitor_default_fps` | `120` | 指纹监测刷新率 |
| `ro.vendor.fps.switch.thermal` | `true` | 温控刷新率切换 |
| `vendor.display.disable_3d_adaptive_tm` | `0` | 显示处理 |
| `ro.vendor.display.dolbyvision.support` | `true` | 显示格式支持标记 |
| `ro.vendor.display.dynamic_refresh_rate` | `120,90,60,30:100,60,5` | 动态刷新率参数 |
| `ro.vendor.display.primary_idle_refresh_rate` | `60,10:528` | 主屏空闲刷新率参数 |
| `persist.vendor.disable_idle_fps` | `true` | 空闲帧率策略 |
| `persist.vendor.disable_idle_fps.threshold` | `528` | 空闲帧率阈值 |
| `ro.vendor.display.ltpo.sync.tp` | `false` | LTPO/触控同步 |
| `vendor.display.disable_stc_dimming` | `1` | 调光策略 |
| `ro.vendor.display.switch_resolution.support` | `1` | 分辨率切换支持标记 |
| `ro.vendor.display.skip_idle_for_90hz` | `true` | 90 Hz 空闲策略 |
| `ro.vendor.mi_sf.skip_90hz_forVrrVote` | `true` | MI SurfaceFlinger/VRR 策略 |
| `ro.vendor.display.standard_d75_enable` | `true` | 显示色彩模式 |
| `ro.vendor.display.mode_change_optimize.enable` | `true` | 显示模式切换 |
| `ro.vendor.mi_sf.ltpo.support` | `true` | LTPO 支持标记 |
| `sys.rhythmic_mode_default_level` | `50` | 显示模式参数 |
| `ro.vendor.display.dobly_dpu.enable` | `true` | 显示处理；原帖键名为 `dobly`，不校正 |
| `ro.vendor.display.update_dbi_by_temperature` | `true` | 温度相关显示处理 |
| `ro.vendor.mi_sf.aod_mode_ddic_refresh_rate` | `10` | AOD 刷新率 |
| `ro.vendor.mi_sf.enable_tp_idle_automode` | `true` | 触控空闲自动模式 |
| `ro.vendor.mi_sf.enable_automode_for_maxfps_setting` | `true` | 最大帧率自动模式 |
| `ro.vendor.mi_sf.supported_automode_maxfps_list` | `60,120` | 自动模式帧率列表 |
| `ro.vendor.mi_sf.support_automode_for_normalfps` | `true` | 普通帧率自动模式 |
| `ro.vendor.mi_sf.skip_promtionfps_for_90hz` | `true` | 原帖键名为 `promtionfps`，含义未核实 |
| `ro.vendor.mi_sf.set_gradient_idle_timer_ms` | `50000` | 空闲计时 |
| `ro.vendor.mi_sf.support_gradient_idleframerate` | `true` | 空闲帧率策略 |
| `vendor.mi_sf.enable_second_refresh_rate_overlay` | `1` | 刷新率 overlay |
| `ro.vendor.display.framework_thermal_dimming` | `true` | 温控调光 |
| `ro.vendor.mi_sf.support_barrage_recogn_althm` | `true` | 原帖键名为 `althm`，含义未核实 |
| `ro.vendor.mi_sf.support_aidl_display_config` | `true` | 显示配置接口 |
| `ro.vendor.display.kernel_recoder_enable` | `true` | 原帖键名为 `recoder`，含义未核实 |
| `vendor.display.boost_cpu_layer_number_threshold` | `10` | 合成性能策略 |
| `ro.vendor.display.video_or_camera_fps.support` | `true` | 视频/相机帧率支持标记 |
| `ro.vendor.display.set_fps_stat_timer_ms` | `250` | 帧率统计周期 |
| `debug.sf.set_idle_timer_ms` | `110` | SurfaceFlinger 空闲计时 |
| `persist.sys.miui_resolution` | `1080,2400,420` | MIUI 分辨率参数 |
| `vendor.display.disable_cwb_call` | `0` | 显示处理 |
| `vendor.display.reserved_writeback_config` | `1` | 显示回写配置 |
| `vendor.display.force_tonemapping` | `1` | 色调映射 |

原帖还要求执行 `dumpsys display` 获取 `uniqueId='local:屏幕id'` 中的数字，并将其替换到 `product/etc/displayconfig/display_id_屏幕id.xml` 文件名中。这里的 `屏幕id` 是说明性占位文字，实际 ID 未提供。上述集合来自作者记录，未提供目标系统版本、属性生效情况或硬件兼容清单。

## 相机、基带、NFC 与人脸入口

### 微信安全模式

原帖建议删除 `odm` 中的 `androidx.camera.extensions.impl.fake.jar`。未说明其对应版本、依赖关系或删除后的影响，不能据此推定其他系统也应删除。

### 无基带

原帖提出删除 `system_ext/priv-app/qcrilmsgtunnel`。未提供目标平台、文件版本或删除验证记录。

### NFC

原帖第一组建议属性为：

```properties
ro.vendor.nfc.repair=1
ro.vendor.nfc.wallet_fusion=1
ro.vendor.nfc.secure_display_optim=1
ro.vendor.nfc.mitouch=1
ro.vendor.nfc.phonecase=1
```

原帖称可补入任意 `build.prop`，并提及开启 NFC 2.0 页面及从网盘文件 `XMNfcNci.zip` 补文件到 `/system/app`。但原帖没有网盘链接、文件内容或版本信息，因此该 ZIP 无法从来源取得，也不能确认适用性。

原帖另称红米 K 系列是否适用第一组方案未知，并给出另一组配置作为替代尝试：

```properties
ro.vendor.se.type=eSE,HCE,UICC
ro.vendor.nfc.selectcard=1
ro.vendor.nfc.repair=1
ro.vendor.nfc.dispatch_optim=2
ro.vendor.nfc.disable.i2c=false
ro.vendor.se.info=18-1010212
ro.vendor.se.chip.model=ST54L
ro.vendor.nfc.wallet_fusion=1
ro.vendor.nfc.secure_display_optim=1
ro.vendor.nfc.debounce_field_on=1
ro.vendor.nfc.field_on_debounce=800
nfc.fw.is_downloading=false
ro.vendor.nfc.default.conf=st54l_conf.txt
ro.vendor.nfc.RF.conf.items=8
ro.vendor.nfc.customRF=1
ro.vendor.nfc.mitouch=1
```

同一段还提到将 `pangu` 补到 `system`、删除 `NQNfcNci`、将 `nfc_st` 补到 `product/app`；没有给出这些文件的来源包、版本或完整路径结构，步骤仍不完整。

### 人脸识别与指纹入口

原帖称从网盘文件 `人脸指纹入口.zip` 补文件到 `vendor/etc/permissions/`。原帖未提供该 ZIP 的下载链接或内容，无法确定文件清单与版本。

## 语音唤醒

原帖将替换 `VoiceTrigger.apk` 作为语音唤醒尝试，并建议补入 `vendor/etc/` 下以 `XiaoAi` 开头的文件。作者记录曾出现可录入、可唤醒但重启后失效，后来更新澎湃版本后功能彻底失效；这是该实验环境的结果，不构成其他设备的复现结论。

对没有自定义唤醒词的场景，原帖还列出：

```properties
ro.vendor.audio.soundtrigger.wakeupword=2
```

原帖没有给出 APK、`XiaoAi` 文件或依赖库的来源与版本，相关资源信息缺失。

## 评论补充范围

另读到的 12 条最新评论中，有读者询问 `XMNfcNci.zip` 的网盘位置或反映 NFC 文件缺失，未出现下载地址或公开提取码。对于魅族 21 Pro 的超声波兼容性提问，作者也未确认适配；不能把该机型加入支持清单。这里只检查了该页，未穷尽历史评论。


来源：[酷安技术帖 72525211](https://www.coolapk.com/feed/72525211)。正文已按知识主题重组，未读取实机固件，也未进行刷机验收。
