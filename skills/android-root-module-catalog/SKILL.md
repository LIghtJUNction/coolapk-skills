---
name: android-root-module-catalog
description: 查询 Android Root 模块的用途、配置路径、触发参数、机型条件、互斥关系和公开项目资源，覆盖救砖、调度、后台、内存、清理、音效、震动与充电。
---

# Android Root 模块知识与资源

按需求查阅 [分类说明、配置与资源](references/modules.md)。另有 [122 项模块与工具分享目录](references/downloads.md)，保留逐项对应的真实公开地址。其中保留可直接使用的公开项目、下载站和发布页地址，不需要额外数据库。

使用时先定位功能，再检查同节的机型、系统、内核和互斥条件。例如 ct 与 AsoulOpt 不同时开启；cs 的 Android 版本范围和 schedhorizon 内核范围分别检查；音效模块的高通、DAX 路径与扬声器条件不能混用。

需要调整充电统计时，查看 `no_trickle` 文件的存在语义；排查自动救砖时，查看重启次数、超时与 OTA 等待时间；音效配置修改后是否重启也在对应章节说明。

这些是原帖所列版本和行为的历史整理，不表示今天的版本、价格或全部机型均已验证。公开链接已与来源地址核对，未逐一下载文件或实机安装。尚未读取全部图片、评论及链接目标正文。

来源：[模块整合](https://www.coolapk.com/feed/47750576)，整理日期：2026-11-13。

## 管理工具的兼容记录

[爱玩机工具箱更新与 Android 17 启动故障](references/aiwanji-update.md)单独记录管理器行为，不与模块本身的兼容性混淆。

## 完整性检查的历史资料

[TrickyStore 与检查工具的历史记录](references/attestation-history.md)分别保留源码/构建入口、当时故障报告与 TEE 观察的证据限度，不把工具读数当作硬件修复证明。

[APatch Android 14 故障引用](references/apatch-android14-report.md)将酷安内核信息与上游 issue 的一加 6T 环境分开，保留关联资料和未读范围。


## 来源补充

- [模块共存、功耗排查与DNS配置历史](references/modules-and-power-history.md)


## 补充的来源技术记录

- [APatch / FolkPatch 2.2 隐藏环境模块：安装流程与兼容反馈](references/source-74740377.md)
- [KernelSU 与分支 Root 环境隐藏教程（2.2）](references/source-74740293.md)
- [KernelSU、Alpha、APatch 等管理器的环境隐藏模块配置记录](references/source-71807864.md)
- [KernelSU、Alpha 与 APatch 环境隐藏包：0417教程及反馈](references/source-71270068.md)
- [APatch/FolkPatch 环境隐藏教程（0302版）](references/source-70474353.md)


## 补充技术报告

- [Android 禁用系统更新模块的类型与适用范围](references/source-72833999.md)

- [Aether_OptExt 线程分配：配置、拓扑与 eBPF 回退](references/source-72833346.md)
- [外屏、系统特性与显示/存储调校案例](references/flip-screen-and-system-ui-tweaks.md)
- [SUSFS 内核模块（.ko）、PowerStats 调度功耗统计与 Root 变砖排查](references/susfs-lkm-powerstats-and-brick-triage.md)
- [`mtk-bwmod` 路由优先级、AxManager 免 Root 模块、Droidspaces 旧机利用与密钥避坑](references/mtk-bwmod-axmanager-and-hidden-env.md)
- [SUSFS UI、高保真音频 XML、SIGSTOP 墓碑与 WorkBuddy Manager 模块](references/susfs-audio-tombstone-and-workbuddy-modules.md)
