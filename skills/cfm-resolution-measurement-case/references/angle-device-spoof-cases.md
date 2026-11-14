# 穿越火线手游开启 ANGLE 的命令与帧率报告

原帖针对包名 `com.tencent.tmgp.cf` 给出以下命令，用于选择 ANGLE 图形驱动：

```sh
settings put global angle_gl_driver_selection_pkgs com.tencent.tmgp.cf;settings put global angle_gl_driver_selection_values angle
```

关闭 ANGLE 时删除这两个全局设置项：

```sh
settings delete global angle_gl_driver_selection_pkgs;settings delete global angle_gl_driver_selection_values
```

原帖报告开启 ANGLE 后局内多次大幅掉帧，并不建议开启。开关前后的帧率截图未读取，因此无法记录具体帧率数值或测试条件。

**来源与证据边界：**命令和掉帧结论来自原帖，未独立验证。

## 全局设置与实际驱动的区别

以下为整理时补充的只读检查，可在修改前记录原值：

```sh
settings get global angle_gl_driver_selection_pkgs
settings get global angle_gl_driver_selection_values
```

原帖的put命令会替换整个全局值，delete也会清除整个键；已有其他应用条目时，不能把删除两键当作保留原配置的精确回滚。[ANGLE Android文档](https://chromium.googlesource.com/angle/angle.git/+/HEAD/doc/DevSetupAndroid.md)说明多应用包名与驱动值用等长逗号列表一一对应，设置跨重启保留。

选择项写入成功并不证明驱动已经加载。该开发文档的侧载ANGLE APK流程要求可调试应用或root，并有单独驱动包选择；系统内置ANGLE则可能不同。原帖没有说明使用哪种来源，不能补造已安装APK或通用开关步骤。官方用ANGLE日志确认加载；未读截图时也不能填帧率数字。


来源：[原帖67524031](https://www.coolapk.com/feed/67524031)。


# 穿越火线手游高级图像选项模块与体验报告

原帖介绍一个用于 Magisk/KernelSU 的机型伪装模块，目标机型为“红魔电竞平板 3 Pro”，用途是解锁《穿越火线：枪战王者》（CFM）中的高级图像设置，包括“超清”和“极清”选项。

作者报告其设备为红米 Note 11T Pro，从最低档“性能”切换到最高档“极清”时，未观察到功率、帧率或耗电变化；这是单设备体验报告，原帖未给出测试条件或测量数据，不能推广为其他设备上的结论。

公开分享页：<https://wwkb.lanzouw.com/ifqLH36l4yti>。

**来源与证据边界：**模块用途及体验来自原帖，未独立验证。原帖没有提供提取码，也未提供模块版本信息。

来源：[原帖67342399](https://www.coolapk.com/feed/67342399)。
