---
name: android-cached-app-freezer
description: 理解AOSP cached app freezer的默认策略、设备支持检测、缓存状态、历史冻结条件与只读检查。
---

# Android AOSP cached app freezer：如何理解默认策略

[相关 AOSP 提交 078f090](https://github.com/aosp-mirror/platform_frameworks_base/commit/078f090dedc2244f2c3bef73df6b75fceb3cef7c) 的提交说明是“Enable Freezer by default”。该提交把 `DEFAULT_USE_FREEZER` 从 `false` 改为 `true`，但运行时 `mUseFreezer` 初始化为 `false`，随后由 `updateUseFreezer()` 检查 `isFreezerSupported()` 与配置；不支持时仍设为 `false`。因此它说明的是该提交中的默认策略，不能据此断言所有 Android 12 及以上设备或当前 ROM 都默认启用。

可按提交提供的测试命令只读检查设备设置：

```sh
adb shell dumpsys activity settings | grep use_freezer=true
```

原帖描述：缓存应用处于 cached 状态一段时间后才可能尝试冻结；前台活动、服务等可能使进程不再处于该状态。应用的进程状态和后台服务状态并不相同，不能把“后台运行”直接等同于“已缓存”。原帖提到的十分钟是该历史帖所述条件；这里没有核对对应版本的完整实现，不能将其视为所有版本固定阈值。原帖还称冻结需要满足安全条件，冻结失败时可能杀进程，并称它不等同于墓碑后台。这些属于作者对当时 AOSP 行为的描述，不构成对所有版本和厂商 ROM 的保证。

来源：[Coolapk 动态 37234522](https://www.coolapk.com/feed/37234522)、[Coolapk 动态 37119561](https://www.coolapk.com/feed/37119561)。
