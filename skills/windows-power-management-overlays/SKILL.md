---
name: windows-power-management-overlays
description: Windows 电源计划与电源模式
---

# Windows 电源计划与电源模式

来源：[电源管理说明](https://blog.xzr.moe/archives/352/)

Windows 可通过系统电源设置调整频率策略、升频积极程度、负载分配和能效倾向。理解时要区分电源计划、电源模式 overlay 与计划选项：它们相关，但不是同一个设置层。

可用只读命令检查配置示例：

```powershell
powercfg /qh OVERLAY_SCHEME_MAX
powercfg /queryprofile
```

这些命令用于查看配置，不会修改电源参数。历史版本中的观察不能直接当作当前版本或所有硬件的通用结论。不同处理器、固件和负载下的合适策略可能不同；本文不提供统一的“最佳参数”。注册表值未逐项核验，因此不据此给出修改命令。

核验范围：已读文章开头、电源 overlay 与场景章节；其余内容未作完整核验。原帖图片未读，评论未读。

来源：[酷安原帖](https://www.coolapk.com/feed/49866275)。本稿未执行设备修改、工具安装、包校验或测量；配图和评论仍未读。
