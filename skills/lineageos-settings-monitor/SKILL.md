---
name: lineageos-settings-monitor
description: 使用设置项监视器v8的公开下载与ap0h提取码，区分LineageOS专有设置、任务栏命令及界面刷新变化。
---

# 设置项监视器 v8 与 LineageOS 专有设置项

设置项监视器 v8 增加对 LineageOS 专有设置项变化的监视。原帖以任务栏为例，称其由 LineageOS 专有设置项 `system enable_taskbar` 控制。监视器还调整为在屏幕大小变化或切换深色模式时不再自动重启应用，并称现可获取所有设置项的值，修正部分设置项显示“无法获取”的问题；另有 UI 微调。

原帖给出的读写示例：

```sh
settings --lineage put system enable_taskbar 1
```

该命令用于开启任务栏。原帖还说明 LineageOS 专有设置项可用 `settings --lineage` 命令读取和写入。

公开分享页：<https://wwkb.lanzouw.com/b02dyzgri>，提取码：`ap0h`。

**来源与证据边界：**功能与命令来自原帖文字，未独立验证。原帖提供公开提取码 `ap0h`。

`--lineage`是对应ROM工具的扩展，不能当作所有AOSP系统均提供的参数；原帖未给完整LineageOS版本和启动器条件。`1`是该帖开启任务栏示例，不保证所有设备具有相同UI。


来源：[原帖67342931](https://www.coolapk.com/feed/67342931)。
