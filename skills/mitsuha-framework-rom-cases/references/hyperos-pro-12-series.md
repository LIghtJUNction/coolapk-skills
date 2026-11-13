# HyperOS Pro：小米 12 系列新春版

> 本文整理自[70287742](https://www.coolapk.com/feed/70287742)的帖子。ROM 名称为 **HyperOS Pro**，不能与 MitsuhaHyper 官改包视为同一版本。以下适配与功能均为原帖自述，未独立测试。

## 适配与基线

帖子列出的适配机型：Xiaomi 12、Xiaomi 12 Pro、Xiaomi 12S、Xiaomi 12S Pro、Xiaomi 12S Ultra。ROM 基于 **Xiaomi 17 Pro Max OS3.0.300.9.C05 BETA** 移植，并合并 SoraYumeKernel。

## 内核特性

- 修改 `mi_mem_engine`：自动追踪 PID，优先保障系统关键进程的内存使用。
- 优化功耗控制、待机功耗、内存管理、f2fs、loop 和 IO。
- 默认使用 `ssg` I/O 调度器；默认 **16G 虚拟内存**。

## ROM 功能

### AI

解锁设置中的小米澎湃 AI 入口，以及全局 AI 帮写、AI 写作、AI 壁纸、AI 拾音、AI 对话翻译、AI 拾音翻译、AI 同声传译、AI 搜索和 AI 实时字幕。

### 环境与验证声明

原帖称通过其私钥生成 `vbmeta` 与 `vbmeta_system` 镜像，目标是保留 AVB 元数据验证及 FEC；并声称可通过 AVB 校验及各检测软件检查，除 BL 锁状态外不会报错。这是作者的功能声明，未经独立验证。即使 AVB 过检，也不能据此证明系统整体安全。

### 系统优化

屏蔽云控和安全中心以保护隐私；添加节律护眼、90 Hz 刷新率及“1 Hz 高刷”（原帖原词，语义可疑，不能据此认定为已证明的息屏 1 Hz）；Super 分区损伤自愈（FEC）；启用景深壁纸和狂暴引擎；添加 pangu 挂载；移除预装软件、analytics 和 MSA；启用 HDR；关闭 SwapFile 并使用 DM 挂载。

### 线刷工具

原帖称使用 C# 重构 ROM 线刷工具并采用新 UI，支持 Windows、Linux、macOS；以引导式流程说明刷机步骤，界面采用小米澎湃混色与 iOS Liquid Glass 风格。输入材料没有提供工具下载链接或更具体的刷机命令、步骤。

## OkuderaVersion 专属优化

原帖将以下项目列为「OkuderaVersion 新增」：

1. 优化核心资源分配策略。
2. 声称彻底修复基带报错，通信共享不掉卡、不掉信号。
3. 声称解决重启后指纹丢失。
4. 解锁 Widevine L1 证书。
5. ZRAM 配置 1:1 拉满。
6. 启用 dm 映射控制器。
7. 新增 UWB 功能支持。
8. 开启全屏 AOD 显示。
9. 启用 APTX Ada2.2 音频解码。
10. 手机管家增加电池详细信息页。

基带、指纹及 UWB 等均为原帖声明，不是独立验证结果。特别是 UWB 软件功能声明不能证明设备新增了 UWB 物理硬件或保证可用。

## 公开资源与获取信息

- 另一条通用资源入口见[技能主页](../SKILL.md#rom-版本与获取入口)；不能确认它仍提供本旧版包。
- 帖子 70287742 指向旧帖 [https://www.coolapk.com/feed/64298028](https://www.coolapk.com/feed/64298028)。原始链接含分享追踪参数；此处去掉该参数保留 canonical feed URL。被引用帖正文未提供，内容未读，不从链接推断其信息。
- 原帖所述获取方式是：点赞并投币该动态、进入主页并关注、查看主页签名中的代码。未取得该代码，不视为旧版包仍可下载的证明。
