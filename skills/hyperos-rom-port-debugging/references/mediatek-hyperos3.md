# HyperOS 3 移植实验记录：米系联发科设备的分区修改

> 本文整理的是原作者在米系联发科设备上的移植实验记录，原帖说明其他机型不保证适用。内容仅涉及解包后的工作副本，不代表通用修复，也未经过独立验证。修改前应保留原机对应文件及其 owner、mode、SELinux 等元数据；原帖提到的统一权限设置不适合作为跨分区通用规则。

## Product：显示、桌面与设备特性

### DPI 与高级材质

在 `/product/etc/build.prop` 中找到 `ro.sf.lcd_density` 和 `persist.miui.density_v2`，将各自的 `440` 改为原机对应值。原帖称此项用于修复 DPI。

若要尝试开启高级材质，原作者建议加入以下属性：

```properties
persist.sys.background_blur_supported=true
persist.sys.background_blur_status_default=true
persist.sys.background_blur_mode=0
persist.sys.background_blur_version=3
```

这些属性只影响软件配置，不能增加设备原本不支持的硬件能力。

### 低内存标记与工作台入口

原帖建议从 `/product/etc/build.prop` 删除以下两项，以去除其所称的低内存限制：

```properties
ro.config.low_ram.threshold_gb=99
ro.config.low_ram.version=2
```

为让工作台显示小窗，原作者建议添加：

```properties
ro.config.miui_desktop_mode_enabled=1
```

### `device_features` 与人脸配置

原作者的做法是从 `/product/etc/device_features/` 选取一个配置文件，改为本机代号命名，并在文件内容中将原设备代号替换成本机代号（原帖示例为 `redmi pad` 改为 `yunluo`）。这属于设备配置移植实验，原帖没有给出可适用于其他设备的映射表。

在该文件中搜索 `fps` 后，作者建议按屏幕实际能力修改这些条目：

```xml
<integer name="support_max_fps">90</integer>
<integer name="smart_fps_value">90</integer>
<integer name="defaultFps">60</integer>
<integer-array name="fpsList">
    <item>90</item>
    <item>60</item>
</integer-array>
```

示例中的 `90`、`60` 及列表内容不是通用默认值。只应填入原机屏幕支持的刷新率；属性本身不会让硬件获得新的刷新率。

对于 `support_tee_face_unlock`，原作者建议把：

```xml
<bool name="support_tee_face_unlock">true</bool>
```

改为 `false`。随后原帖另列出完整人脸识别相关 APK 的放置方法，见下文；这些步骤是作者环境中的组合实验记录。

### 语音唤醒 APK 与权限

原作者建议在 `/product/etc/permissions/privapp-permissions-product.xml` 的 `</privapp-permissions>` 与 `</permissions>` 之间加入针对 `com.miui.voicetrigger` 的权限声明：

```xml
<privapp-permissions package="com.miui.voicetrigger">
    <permission name="android.permission.START_FOREGROUND_SERVICES_FROM_BACKGROUND" />
    <permission name="android.permission.WRITE_SECURE_SETTINGS" />
    <permission name="android.permission.CAPTURE_AUDIO_HOTWORD" />
    <permission name="android.permission.MANAGE_SOUND_TRIGGER" />
    <permission name="android.permission.MANAGE_USERS" />
    <permission name="android.permission.INTERACT_ACROSS_USERS" />
    <permission name="android.permission.BLUETOOTH_PRIVILEGED" />
    <permission name="android.permission.READ_PHONE_STATE" />
</privapp-permissions>
```

然后在 `/product/priv-app/` 下创建 `VoiceTrigger` 目录，放入从联发科机型 OS2（A15）提取的同名 APK。原帖未提供来源包、具体版本或下载链接。

### 人脸识别 APK

原作者记录的后续步骤是：在完成前述 `device_features` 修改后，在 `/product/app/` 下创建 `Biometric` 目录，放入从 `n12tp` 的 OS3 提取的同名 APK。原帖未提供该 APK 的来源包、版本或下载链接，无法据此补出文件地址。

### 相机、圆角、状态栏与认证型号

原帖记录了以下替换方式，均要求使用对应原机文件：

- 用原包替换 `/product/priv-app/MiuiCamera/MiuiCamera.apk`。
- 用原机文件替换 `/product/overlay/DevicesAndroidOverlay.apk` 和 `/product/overlay/DevicesOverlay.apk`，作者将其用于桌面圆角和状态栏显示。
- 从原机提取 `/product/overlay/SettingsRroDeviceTypeOverlay.apk`，替换移植包中的同名文件，以处理设置页中的认证型号显示。

原帖没有提供这些文件的独立下载链接或版本信息。

### 默认锁屏壁纸与开机动画

原作者建议将壁纸文件命名为 `lock_wallpaper`，并放入以下两个位置：

- `/product/media/customize/lock_wallpaper`
- `/product/media/theme/default/lock_wallpaper`

若不存在 `customize` 目录，原帖建议创建该目录。其提到的 `755 root root` 是原作者环境中的权限记录，不应视为适用于所有分区的权限规则；应保留目标环境原有的 owner、mode 和 SELinux 元数据。

开机动画实验步骤是：在 `/product/media/` 中删除 `bootanimation01.zip` 至 `bootanimation05.zip`，再放入 `bootanimation.zip`。原帖未附动画文件来源。

### 工作台 8GB 门槛

原作者记录了两个 smali 修改点：

1. 在 `/product/app/MIUISystemUIPlugin/MIUISystemUIPlugin.apk` 中查找 `miuidesktop`，进入 `miui.systemui.quicksettings/MiuiDesktopModeTile` 并搜索 `isAvailable`。原文展示的方法片段中，`const/4 v0, 0x0` 后没有展示返回指令；其说明是将该常量改为 `0x1`，片段中还给出 `.registers 1`、`return v0` 与 `.end method`。因原帖片段不完整，应以实际反编译方法结构为准，不能直接套用残缺代码。
2. 在 `/system_ext/priv-app/Settings/Settings.apk` 的 `com.android.settings.utils/SettingsFeatures` 中搜索 `total`。定位到 `DeviceLevel->TOTAL_RAM:I` 相关逻辑后，原作者建议把所示 `const/16 v5, 0x8` 改为 `0x1`。

这些是原作者针对其移植包的实现记录，不代表单改门槛即可让所有设备获得工作台能力。

## System_ext：性能等级与桌面动画

### 性能分级资源列表

在 `/system_ext/framework/framework-ext-res/framework-ext-res.apk` 的 `resources.arsc` 中，原作者提到检查 `android.miui/array/array` 下的列表：

- `high_device_cpu_list`、`high_device_gpu_list`
- `middle_device_cpu_list`、`middle_device_gpu_list`
- `low_device_cpu_list`、`low_device_gpu_list`

原帖示例称设备 CPU 代号为 `mt6789` 时，GPU 代号也按 `mt6789` 处理，并在 middle 列表中找到后与 high 列表中的代号交换。示例代号分别写成 `O1_asic` 和 `O1-asic`，两种拼写不一致，不能据此认定二者等价；实际资源列表需先核对。作者还特别说明保存退出时不要选择签名。

### 低内存版本的桌面动画限制

在 `/system_ext/framework/MiuiBooster.jar` 中，原作者建议搜索 `total`，进入 `com.miui.performance/DeviceLevelUtils` 的 `getDeviceLevelForRAM()`。其记录的两个阈值修改为：第一处 `const/4 v1, 0x4` 改为 `0x1`；第二处 `const/4 v1, 0x6` 改为 `0x4`。该记录用于其环境中的低内存版本桌面动画限制实验。

## Mi_ext：版本标识与设备代号

在 `/mi_ext/etc/build.prop` 中：

- `ro.mi.os.version.incremental` 后的内容是原作者所称的系统版本号，可按目标移植环境需要修改。
- 将 `ro.product.mod_device` 等号后的内容改为本机代号，是原帖记录的另一项尝试，用于处理其所称的“玄学问题”。

## Mi_ext 与 System：Rust 运行时（未完成）

原作者建议将 `/mi_ext/product/etc/permissions/hyperos.rustruntime_v3.xml` 重命名为 `hyperos.rustruntime_v3_v4.xml`，并按文件原有格式加入：

```xml
<library name="hyperos.rustruntime.v4"
         file="/system_ext/framework/hyperos.rustruntime.jar" />
```

原帖明确表示还需要补充库文件，但不知道具体需要哪些；只记录了其观察到库文件需放在 `system/lib64` 才能正确加载。依赖库清单、来源包和版本均缺失，因此该运行时补库方案仍未完成。

## System：APK 签名方案版本检查

原帖记录的修改位置是 `/system/framework/services.jar` 内的 `classes.dex`、`classes2.dex`：搜索 `getMinimumSignatureSchemeVersionForTargetSdk`，并称将相关 `invoke-static` 至 `move-result` 指令替换为 `const/4 v*, 0x0`，其中寄存器编号应按实际反编译结果确认。

原帖另要求保存后不重新签名；目标系统的实际校验约束仍需核对。

该方法名对应 AOSP `ApkSignatureVerifier.java` 中的 `getMinimumSignatureSchemeVersionForTargetSdk`，其职责是决定目标 SDK 所需的最低 APK 签名方案版本。把这里的修改描述为“完全取消系统签名或证书校验”并不准确；该修改不能保证移除所有校验。此处只说明对应概念与原帖声称的修改位置，不代表已核验整套闭源 ROM 的验证流程。

AOSP 参考：[ApkSignatureVerifier.java](https://android.googlesource.com/platform/frameworks/base/+/refs/heads/main/core/java/android/util/apk/ApkSignatureVerifier.java)

## System：I/O 启动优化属性

原作者注明需搭配支持该功能的内核，并建议在 `/system/build.prop` 写入或调整：

```properties
persist.sys.stability.iorapEnable=true
ro.iorapd.enable=true
```

原帖给出的第二项原值是 `ro.iorapd.enable=false`，建议改为 `true`。属性配置本身不保证内核或系统组件支持该功能。

## 资料缺口与适用范围

原帖没有提供网盘链接、固件版本或可下载的配套文件。文中提到的 OS2（A15）、OS3、`n12tp` 来源 APK，以及壁纸、开机动画和各类原机 overlay 均没有公开下载地址；不要据此推造文件网址。所有修改都应理解为作者在米系联发科设备上的移植实验记录，具体文件与原机元数据需由实际工作副本核对。

来源：[酷安技术帖 72865819](https://www.coolapk.com/feed/72865819)。正文已按知识主题重组，未读取实机固件，也未进行刷机验收。
