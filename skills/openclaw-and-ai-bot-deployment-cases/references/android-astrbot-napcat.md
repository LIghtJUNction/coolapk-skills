# Android 上的 AstrBot 与 NapCat：PRoot Ubuntu 应用方案

## 应用架构与功能
原帖介绍的 Android 应用使用 PRoot Ubuntu 运行 AstrBot 与 NapCat，界面采用 Kotlin 和 Jetpack Compose。应用负责日常启停、日志、备份与更新；AstrBot、NapCat 的业务配置仍通过各自自带的 WebUI 完成。另有角色人设预设、后台保活操作和通知栏状态查看。

原帖将常见部署描述为 AstrBot 搭配 NapCat（或 Snowluma），用于扩展 QQ 对话能力；提到可运行于 Windows、Linux 小主机或云服务器，通常采用 Docker 或原生部署。云服务器配置稍高时成本较高，作者称至少需要 2 核 2 GB，并建议创建 swap 以缓解内存空间不足。小型 ARM 设备例子包括 N1（2+8 配置）、RK3566（4+8 配置、可扩展 SATA）和 RK3568；这些设备通常需刷入 Armbian 或飞牛 NAS 等 Linux 系统，存储可通过 USB 扩展。上述是原帖列举的部署经验，不是兼容性测试结论。

## 原帖报告的 Android 应用情况
作者称初版已完成基础功能，Android 8 以上可用且无需 root。应用约 700 MB（含 Ubuntu 系统）；启动两个项目并加载一些插件时，内存占用约 800 MB。此为作者报告，原帖未列出测试手机型号、系统版本、测量方法或持续运行测试数据。

项目仓库：<https://github.com/JunWan666/astrbot-for-android>（原帖链接）。原帖称项目采用 MIT 许可。

## 证据边界
原帖附有 9 张图片，图片内容未读取；正文未提供具体安装步骤、版本号或性能测试方法。本文只记录正文明确描述的架构、功能和作者报告的数据。

来源：[74636303](https://www.coolapk.com/feed/74636303)（图片与评论未读，未在本库实机复现）。

