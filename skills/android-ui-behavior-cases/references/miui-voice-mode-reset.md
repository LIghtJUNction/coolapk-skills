# 老版音质音效：人声模式回退为智能

[原帖 74866624](https://www.coolapk.com/feed/74866624)报告：选择“人声”，进入抖音后再返回，模式变回“智能”。作者给出的属性值为：

```properties
persist.vendor.audio.misoundasc=false
persist.vendor.audio.misound.disable=true
```

这是属性键值，不是可以直接执行的 shell 命令。原帖未给出写入位置、设置工具、是否重启或撤销方法，不能补造成特定模块安装脚本。

原帖明确排除小米10S，没有列出其他机型或完整系统版本。本库没有设置属性或验证修复，不将“老机型”扩大为所有 MIUI/HyperOS 设备。
