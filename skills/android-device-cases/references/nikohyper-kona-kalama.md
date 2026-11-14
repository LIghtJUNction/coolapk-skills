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

## 补充的 Kona 和 Kalama 版本

下表为各发布帖所称版本与关键差异；未实机测试，图片与完整评论未读取。Kona 的列出机型均为小米 10（UMI）、10 Pro（CMI）、10 Ultra（CAS），部分帖还写“等”，不能据此补全其他机型。

| 来源 | 原帖版本与基线 | 更新或边界 |
|---|---|---|
| [67226656](https://www.coolapk.com/feed/67226656) | Kona，OS3.0.0.25.WOBCNXM，基于小米 15 Pro | 声称修复线刷脚本、WPA3 与 tethering 相关问题；列出指纹和 LHDC 为问题 |
| [67674949](https://www.coolapk.com/feed/67674949) | Kona，标题 OS3.0.17.0.WPBCNXM，正文基于 OS3.0.16.0.WPBCNXM | 原帖两个版本号不同，保留差异；列出指纹和 LHDC 问题 |
| [68532647](https://www.coolapk.com/feed/68532647) | Kona，OS3.0.30.0.WPBCNXM | 作者列出指纹和 LHDC 问题 |
| [69091281](https://www.coolapk.com/feed/69091281) | Kona，OS3.0.34.0.WPBCNXM | 声称同步官方更新；列出指纹和 LHDC 问题 |
| [69351054](https://www.coolapk.com/feed/69351054) | Kona，OS3.0.36.0.WPBCNXM | 声称更新 6.2 相机并修复 10 Pro/Ultra 长焦问题；指纹和 LHDC 仍列为问题 |
| [68691067](https://www.coolapk.com/feed/68691067) | Kona，OS2.0.216.2.VOBCNXM，基于 OS2.0.216.0.VOBCNXM | 称同步部分 OS3 修改，提供高级设置与纯净两版；正文未给具体下载地址 |
| [70701283](https://www.coolapk.com/feed/70701283) | Kalama，OS3.0.303.5.WPBCNXM | 列出小米 13/Pro/Ultra，称调整线程、SystemUI 优先级与温控；效果、功耗及稳定性未实测 |
| [73259691](https://www.coolapk.com/feed/73259691) | Kalama，正文 OS4.0.0.20.WPBCNXM | 列出小米 13/Pro/Ultra，称合并 5.15.211 内核、修复 NFC 和指纹支付；标题版本写法与正文不同，以实际包为准 |

原帖对内核、调度、充电和硬件功能的描述均为发布者报告，不能据此保证兼容或性能提升。上文项目文档入口未核验具体文件，其中 68691067、70701283、73259691 正文没有给出具体下载链接，不应假设它们一定在同一目录。

## 后续 OS3 与 OS4 发布帖

下列版本、功能和问题均为原帖声明，未下载或实机验证；Kalama 指帖子列出的小米 13（fuxi）、13 Pro（nuwa）、13 Ultra（ishtar），Kona 指小米 10（UMI）、10 Pro（CMI）、10 Ultra（CAS）。

| 来源 | 机型与版本 | 原帖关键信息 |
|---|---|---|
| [71554234](https://www.coolapk.com/feed/71554234) | Kalama；标题/基线 OS3.0.306.4.WPBCNXM，正文开头却写 OS3.0.9.0.WPBCNXM | 称支持官方扩容电池；13、13 Pro、13 Ultra 底包分别 OS3.0.304.0.WMCCNXM、OS3.0.306.0.WMBCNXM、OS3.0.302.0.WMACNXM。关于本机显示原容量及健康度读数的说法未验证 |
| [71947594](https://www.coolapk.com/feed/71947594) | Kalama；OS3.0.309.0.WPBCNXM | 底包分别为 13 OS3.0.307.0.WMCCNXM、13 Pro OS3.0.309.0.WMBCNXM、13 Ultra OS3.0.305.0.WMACNXM；电池续航说法仅为待测传闻 |
| [70296176](https://www.coolapk.com/feed/70296176) | Kalama；标题 OS3.0.300.8.WPACNXM，正文开头 OS3.0.300.8.WPBCNXM，所述移植基线 OS3.0.300.7.WPACNXM | 三处版本不同；底包 13 OS3.0.3.0.WMCCNXM、13 Pro OS3.0.4.0.WMBCNXM、13 Ultra OS3.0.4.0.WMACNXM |
| [73435943](https://www.coolapk.com/feed/73435943) | Kalama；OS4.0.0.25.XPBCNXM | 称修复景深壁纸、相册文字提取及 AI 模型下发 |
| [73584471](https://www.coolapk.com/feed/73584471) | Kalama；标题 OS4.0.0.32.XPBCNXM，正文和基线写 OS4.0.0.32.WPBCNXM | 称同步官方更新；版本冲突待核对实际包 |
| [73436038](https://www.coolapk.com/feed/73436038) | Kona；OS4.0.0.25.XPBCNXM | 称因底包限制刷入前需格式化 data；列出指纹、基带等修复声明 |
| [73584515](https://www.coolapk.com/feed/73584515) | Kona；OS4.0.0.32.XPBCNXM | 同样称需格式化 data，并列出指纹、基带等项目；原文未逐项提供测试结果 |
| [74141099](https://www.coolapk.com/feed/74141099) | Kona；OS4.0.17.0.XEOCNXM，基于小米 18 Pro Max | 称修复 LHDC、小爱唤醒及 Wi-Fi 断连；附 [NekoFODService 相关帖](https://www.coolapk.com/feed/74086391?s=OThmN2ZiZjQzNTU5Y2FnNmFjMGNkY2N6a1664)，关联帖未读取 |

版本冲突应以设备和实际包的可靠信息核验，不能自行替换字段。格式化 data 会删除数据；原帖的范围声明不等于其他 ROM 也适用，刷入前须确认包和恢复方式并备份。这些正文没有具体下载直链；图片及完整评论未读取。

## 其他 NikoHyper 版本补充

以下按原帖正文记录，适配与修复为作者声明，未独立下载、刷机或实测。Kalama 指小米 13/13 Pro/13 Ultra，Kona 指小米 10/10 Pro/10 Ultra；图片和完整评论未读取。

| 来源 | 系列与版本 | 特定条件或更新 |
|---|---|---|
| [70184932](https://www.coolapk.com/feed/70184932) | Kona OS3.0.300.7.WPACNXM | 基于小米 17 Ultra；称调整 swappiness 为 180、修复人脸录制；另附[小米 13 系列关联帖](https://www.coolapk.com/feed/70160728?s=MDYzMzQyOTEzNTU5Y2FnNjk4ODkyY2R6a1595)，关联帖未读 |
| [69817533](https://www.coolapk.com/feed/69817533) | Kalama OS3.0.10.0.WPACNXM | 基于小米 17 Ultra；称更新 F2FS、调度及 SukiSU；部分功能放在未读取图片中 |
| [70937224](https://www.coolapk.com/feed/70937224) | Kalama OS3.0.305.0.WPBCNXM | 称调整 ZRAM、内存占用与附加模块 |
| [71080697](https://www.coolapk.com/feed/71080697) | Kalama OS3.0.305.4.WPBCNXM | 称同步官方更新及优化功耗；没有具体测试结果 |
| [71301468](https://www.coolapk.com/feed/71301468) | Kalama OS3.0.306.0.WPBCNXM | 称 13 Ultra 底包更新至 3.0.303、支持官方扩容电池；AOD 修复留待下一版 |
| [72794285](https://www.coolapk.com/feed/72794285) | Kalama OS3.0.315.1 / 315.2.WPBCNXM | 称基于 315.0 / 315.2；13、13 Pro、13 Ultra 底包分别为 OS3.0.308.0.WMCCNXM、OS3.0.310.0.WMBCNXM、OS3.0.306.0.WMACNXM；部分内核说明在未读图片中 |
| [73358851](https://www.coolapk.com/feed/73358851) | Kalama OS4.0.0.23 | 标题和基线写 XPBCNXM，正文开头写 WPBCNXM；保留差异待核实 |
| [73359702](https://www.coolapk.com/feed/73359702) | Kona OS4.0.0.23.XPBCNXM | 称移除 SukiSU、需自行修补 Root；称因底包限制需格式化 data，且列出硬件修复项目但未逐项给出测试结果 |

格式化 data 会删除数据，刷机前应先核对实际设备、文件和恢复方式并备份。上述正文没有具体 ROM 下载直链，不能把其他版本资源推定为本版文件。
