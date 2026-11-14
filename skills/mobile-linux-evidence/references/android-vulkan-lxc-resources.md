# Android Vulkan、Droidspaces 与 VPS 案例

## android-vulkan-bridge

原帖：[天玑硬件加速说明](https://www.coolapk.com/feed/71647644)；项目：[kde-yyds/android-vulkan-bridge](https://github.com/kde-yyds/android-vulkan-bridge)。作者称 Arch 更新太快，项目提供的 pkgconfig 已无法复现；作者计划视反馈决定是否另写教程。原帖没有提供可复现的新教程或编译命令。原帖称“理论所有设备通用”，这不是实测兼容范围。

## Droidspaces、内核教程与 OpenClaw

原帖：[Droidspaces 与一加 13 体验](https://www.coolapk.com/feed/71087785)。

- Droidspaces rootfs 构建项目的 release：[ravindu644/Droidspaces-rootfs-builder releases](https://github.com/ravindu644/Droidspaces-rootfs-builder/releases)。
- Droidspaces 内核编译教程入口：[酷安 feed 70968992](https://www.coolapk.com/feed/70968992)。
- 原帖提供的内核项目：[7568168/oppo_oplus_realme_sm8750](https://github.com/7568168/oppo_oplus_realme_sm8750)。原帖限定为“仅一加 13”实测内核。
- OpenClaw 中文教程入口：[酷安 feed 70583110](https://www.coolapk.com/feed/70583110)。原帖称依据教程用 CCTV8 添加“0宽字符”（原文字面，原帖称会影响文件搜索性能但不知具体多少）、开了 KPM，并涉及 Droidspaces 运行相关内容。KPM 和编译所需具体参数没有给出；不据此编造 CONFIG 名称或命令。
- 作者称 Termux 与 Droidspaces 的资源占用“都差不多”，并描述启动体验；图示数据未在正文中给出。作者关于 LXC 比 Docker 性能强的说法明确承认“没测过”，属于原理推断，不能推广为性能结论。

## VPS 学习场景

原帖：[Docker、Nginx 与 HTTPS 学习体验](https://www.coolapk.com/feed/74730521)。帖子用 VPS 场景说明，将服务部署到远端后可通过网址访问，电脑关机后远端服务仍可运行。这只能提炼为远端服务与本机生命周期不同；帖子没有实际部署、持续运行或 24 小时可用性验证，也不作为作者产品体验或个人信息来源。
