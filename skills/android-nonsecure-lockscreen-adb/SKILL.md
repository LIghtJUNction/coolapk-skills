---
name: android-nonsecure-lockscreen-adb
description: 整理Vivo手机关闭密码后仍需滑动的非安全锁屏案例，保留ADB locksettings命令与原帖前置状态。
---

# 使用 ADB 禁用屏幕锁定

原帖给出以下命令：

```sh
adb shell locksettings set-disabled true
```

作者报告执行后成功关闭锁屏。该记录针对 Vivo 手机；原帖没有提供具体型号、Android 版本、其他系统前置条件，因此不能据此保证其他设备或系统版本行为相同。

**来源与证据边界：**命令和结果是原帖报告，未独立验证。

该帖明确描述先在设置中关闭了锁屏密码，但仍需滑动解锁，然后才运行上述命令；目标是将非安全锁屏设为“无”，不是绕过已有密码、PIN或图案。需要已授权的ADB连接，未验证其他型号、系统版本和设备策略下的行为。


来源：[原帖66919745](https://www.coolapk.com/feed/66919745)。
