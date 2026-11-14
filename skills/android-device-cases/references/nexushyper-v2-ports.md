# NexusHyper v2 移植版：机型、底包与下载入口

以下为发布帖及其作者评论的历史记录，适配、下载与刷机均未独立验证。不要把同一版本的不同机型包互刷。

## v2.1.2：小米 17 Pro 基线

[70700498](https://www.coolapk.com/feed/70700498)、[70697081](https://www.coolapk.com/feed/70697081)、[70697075](https://www.coolapk.com/feed/70697075) 三帖均称基于 Xiaomi 17 Pro 的 Android 16、OS3.0.304.2.WBLCNXM，安全补丁 2026-02-01。正文分别列出：

| 来源 | 机型与底包 |
|---|---|
| 70700498 | K70 Pro：OS3.0.7.0.WNMCNXM；小米 14 Pro：OS3.0.8.0.WNCCNXM；小米 17：OS3.0.44.0.WPCCNXM；K90 Pro Max：OS3.0.23.0.WPMCNXM |
| 70697081 | K60 Pro：OS2.0.210.0.VMKCNXM；小米 13：OS3.0.2.0.WMCCNXM；小米 13 Pro：OS3.0.3.0.WMBCNXM |
| 70697075 | K60：OS2.0.220.0.VMNCNXM；Note 12 Turbo：OS3.0.1.0.VMRCNXM；K50 Ultra：OS2.0.209.0.VLFCNXM |

第一帖的 14 Pro 底包后缀 `WNCCNXM` 按原文保留，未核对是否为笔误。三个帖子称加入 MiClaw，但作者评论称可能因缺少内测权限返回 403，需自行部署 API；并建议天气无法打开时卸载后从应用商店安装。正文列出多项修复并称无剩余 bug，这是作者报告，不能当作无故障保证。

每篇已采集到的作者评论都列出 [Alist](https://oss.1mxy.cn/139)、[139 云盘](https://yun.139.com/shareweb/#/w/i/2tZwpFGRUATp1) 和 [123 云盘](https://www.123684.com/s/lUXDVv-724Wh)，称 MD5 留在单独文件；未读取该文件或核验具体下载包。部分较早评论另给 [139 云盘旧入口](https://yun.139.com/shareweb/#/w/i/2s2xRUuKPXCw9)，其中一篇称文件名末十位可与 MD5 末十位对照，这不能替代完整可信校验值。评论未提供公开提取码，入口当前可用性未知。

## v2.0.0：小米 14 Pro 基线

[70292778](https://www.coolapk.com/feed/70292778)称基于 Xiaomi 14 Pro 的 OS3.0.300.5.WNBCNXM Beta，Android 16，补丁 2026-01-01。列出的底包为 K60 OS2.0.220.0.VMNCNXM、K60 Pro OS2.0.210.0.VMKCNXM、K70 Pro OS3.0.7.0.WNMCNXM、小米 14 Pro OS3.0.8.0.WNBCNXM。帖子特别称该版本不支持从官方 3.0.300.5 直接刷入；这限制了同帖较宽泛的官方包直升声明。

作者称 5.10 内核机型不支持 LHDC；eSIM 对小米 13、Note 12 Turbo、K50 Ultra、K60 Pro 标为支持，小米 13 Pro、K70 Pro、12S Ultra 标为待测试，K60 标为不可用。它们只是该帖报告，不能推广到别的版本。正文称下载在评论，已采集评论中未发现可对应的下载地址。

## 共同边界

帖子要求解锁 Bootloader，仅使用对应机型包，并称不支持从解密包直接刷入。v2.1.2 的 70700498 帖特别称小米 17 与 K90 Pro Max 仅支持线刷；不要把其他机型的卡刷方式套用过去。帖子列有移除温控及签名验证等改动，具体适用性与效果未复测。刷入前核对真实设备、对应包及恢复方式并备份数据。图片和完整评论均未读取。
