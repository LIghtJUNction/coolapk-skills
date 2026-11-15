---
name: mobile-linux-evidence
description: 评估 Termux、手机 Linux、LXC 和 Docker 社区方案的可复现性与性能证据，再制定验证步骤。
---

先区分运行环境：安卓应用内终端、用户空间环境、依赖自定义内核的容器、替换原系统的发行版。记录具体设备、内核、软件版本、依赖与源码提交；不能仅按“手机跑 Linux”合并步骤。

编译失败先对照依赖版本和原项目构建环境，定位第一处实际错误。帖子已报告原构建步骤失效时，先复现并找出版本差异，不直接继续传播旧命令；“理论全设备通用”不能替代驱动和内核适配证据。

对性能主张，要求相同设备、相同任务、相同数据和相近运行条件。分别量启动耗时、空闲占用、任务耗时与峰值资源；不能从一种指标推出所有指标都更快。保存自己的测量条件，区分作者主观体验和可核对数据。

## 依据与缺口

- [构建环境变化案例](https://www.coolapk.com/feed/71647644)：正文报告依赖变化后无法复现，但未提供新的完整步骤。此处只提炼版本锁定与复现检查，不提供不存在的修复命令。
- [手机容器体验](https://www.coolapk.com/feed/71087785)：正文同时出现未测试的性能判断、启动体验和资源占用对比；配套内核仅明确一个机型。不能直接得出 LXC 比 Docker 更快，图片数据与链接教程仍待读取。
- [VPS 学习体验](https://www.coolapk.com/feed/74730521)：是概念解释与产品体验，缺少部署配置和验收证据，未作为可执行部署知识收入。

2026-11-13 已读文字；图片、外链、评论未读，未编译内核或运行性能测试。实际实现前读取相关项目当前官方构建说明。


## 具体案例与公开资源

[Android Vulkan、Droidspaces 与 VPS 资源案例](references/android-vulkan-lxc-resources.md)保留原帖已提供的数值、版本、操作与资源入口；均按社区报告和具体条件理解。

- [OPPO Reno7 运行 7.3 主线 Linux 分区与刷机线索](references/oppo-reno7-mainline-linux.md)
- [OpenClaw Pocket 离线模块、Droidspaces LKM（`.ko`）与内核提权分析](references/openclaw-pocket-and-droidspaces-lkm.md)
- [华为 MatePad 11（2021 / 骁龙 865）主线 Linux 7.3.1 适配实录](references/matepad11-mainline-linux-7-3.md)
- [小米 13 主线双系统、小米 9 Linux UEFI（Docker）与 K20 Pro Ubuntu Chroot](references/fuxi-cepheus-and-k20pro-linux.md)
- [小米平板 6S Pro/5 Linux 生态、LG V30 postmarketOS、ZeroPanel 与树莓派 4B](references/sheng-nabu-lgv30-pmos-and-zeropanel.md)
- [`linux-warsaw` 主线 Linux 7.3-rc6 DTS 上游适配进展](references/linux-warsaw-mainline.md)
- [OPPO Reno7 开源内核、UEFI 移植排障与 Kali 6.1 主线 Linux 进度](references/oppo-reno7-uefi-kali-mainline.md)
- [iPad Pro M4 本地模拟运行 macOS (Henge) 配置与内存权限排障](references/ipad-m4-henge-macos-vm.md)
