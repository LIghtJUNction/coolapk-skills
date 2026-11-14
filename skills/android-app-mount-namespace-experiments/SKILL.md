---
name: android-app-mount-namespace-experiments
description: 分析Android应用挂载命名空间、继承ART/framework文件句柄、Zygisk进程选择与挂载检测实验的版本差异时使用。
---

# Android应用挂载视图与文件句柄实验

读取[art-fd-rebind实验记录](references/art-fd-rebind.md)。将Oplus overlay隔离和ART JAR句柄重开分别处理；同次开机、同版本检测器与相同开关才有可比性。

原帖分别涉及主进程、AppZ父进程和普通isolated服务，不能只检查主进程就认定所有检查进程视图一致。路径白名单、inode/flags/offset和失败回滚是来源实现条件，不得简化为任意路径FD替换。记录只覆盖PHK110、Android16和Native Detector7.7.0，Experimental/Custom ROM检测未开启。
