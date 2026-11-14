# NikoHyper 小米 10 与小米 13 移植版本记录

以下是三个历史发布帖中的兼容和问题声明，未下载、刷机或独立验证。项目的[腾讯文档入口](https://docs.qq.com/document/DV3R2Vkd4bEt2dXR6)由原帖提供；文档正文、具体包直链和现时获取条件未核验。

## 小米 10 系列：Kona

[OS3.0.25.0.WPBCNXM](https://www.coolapk.com/feed/68174845) 和 [OS3.0.32.0.WPBCNXM](https://www.coolapk.com/feed/68973264) 两帖均称基于 Xiaomi 17 Pro Max 移植，列出 Xiaomi 10（UMI）、10 Pro（CMI）和 10 Ultra（CAS）。前者标注安全补丁 2025-10-01，后者标注 2025-11-01；这些日期均是原帖声明。

两帖都列出指纹与蓝牙 LHDC 为已知问题；相机、外放、NFC、人脸和基带可用属于作者报告，并非本整理的测试结论。两版各有纯净版与白羊高级设置版，后者提供状态栏歌词、功耗指示器等设置。帖子称杜比全景声需附加模块；OS3.0.25 帖警告 10/10 Pro 开启可能出现爆音。OS3.0.32 帖列出 Android 16 的连接至 Windows、AI 录音与笔记，并称 Android Auto 仍待测试。

## 小米 13 系列：Kalama

[OS3.0.0.25.WOBCNXM](https://www.coolapk.com/feed/67226549) 声称基于 Xiaomi 15 Pro 移植，列出 Xiaomi 13（fuxi）、13 Pro（nuwa）和 13 Ultra（ishtar），安全补丁 2025-08-01。帖子称内核合并至 5.15.191，并有预装无 KSU 的 NikoKernelX 与另行获取的 SukiSU 变体；不能把这当作所有后续版本的内核配置。作者列出指纹、相机和基带等可用，未列出明确故障清单，未列故障不等于无故障。

上述适配、解密、音效与性能均为帖子自述；每台设备刷入前需核对精确型号、目标版本及实际包说明。图片和完整评论未读取。

## 小米 13 系列后续版本

以下四帖均称基于 Xiaomi 17 Pro Max（popsicle）移植，列出的目标为 Xiaomi 13（fuxi）、13 Pro（nuwa）和 13 Ultra（ishtar）；均为作者声明，未独立刷机验证。

| 来源 | 移植基线 | 原帖安全补丁 | 原帖更新要点 |
|---|---|---|---|
| [68161547](https://www.coolapk.com/feed/68161547) | OS3.0.25.0.WPBCNXM | 2025-10-01 | ZRAM 大小设为内存的 1.5 倍；声称合并 Linux 5.15.195、添加高通 PPS 快充 |
| [68532560](https://www.coolapk.com/feed/68532560) | OS3.0.30.0.WPBCNXM | 2025-10-01 | 引入 WM Shell 优化属性；列出各设备底包版本 |
| [68969999](https://www.coolapk.com/feed/68969999) | OS3.0.32.0.WPBCNXM | 2025-11-01 | 列出 13 Ultra/Pro 的新底包；加入连接至 Windows；称更新 SukiSU 4.0 与 SUSFS 2.0.0 |
| [69091256](https://www.coolapk.com/feed/69091256) | OS3.0.34.0.WPBCNXM | 2025-11-01 | 称同步官方更新 |

OS3.0.30 帖列出 13、13 Pro、13 Ultra 底包分别为 OS2.0.211.0.VMCCNXM、OS2.0.215.0.VMBCNXM、OS2.0.207.0.VMACNXM；作者建议从更高底包迁入时双清，并推测旧底包可能绕过电池锁容云控，但仅称其 13 Ultra 未遇到锁容，不能证明其他设备或环境有同样效果。双清会清除数据，刷入前应备份并核对具体包的说明。

OS3.0.32 帖另列 13 Ultra 底包 OS2.0.210.0.VMACNXM、13 Pro 底包 OS2.0.219.0.VMBCNXM；Android Auto 明确由作者留待用户自行测试。各帖配图及完整评论未读取，具体包文件和下载条件仍以项目文档为准。
