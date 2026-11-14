# HyperOS r16.3 与 r17 系列额外机型记录

各帖均为 Android 16 移植版本；下表机型、底包和更新为原帖声明，未下载、刷机或复测。

| 来源 | 版本和移植基线 | 正文列出的底包 |
|---|---|---|
| [69691088](https://www.coolapk.com/feed/69691088) | r16.3；小米 13 OS3.0.2.0.WMCCNXM；补丁 2025-11-01 | K60 OS2.0.220.0.VMNCNXM；K60 Pro OS2.0.210.0.VMKCNXM；小米 13 OS3.0.2.0.WMCCNXM；K70 Pro OS3.0.7.0.WNMCNXM；另有 OS3.0.3.0.WMBCNXM，机型原文标“？” |
| [69797002](https://www.coolapk.com/feed/69797002) | r17；小米 17 Ultra OS3.0.10.0.WPACNXM；补丁 2025-12-01 | K60 OS2.0.220.0.VMNCNXM；K60 Pro OS2.0.210.0.VMKCNXM；小米 13 OS3.0.2.0.WMCCNXM；小米 13 Pro OS3.0.3.0.WMBCMXM；K70 Pro OS3.0.7.0.WNMCNXM |
| [69820474](https://www.coolapk.com/feed/69820474) | r17；同上 17 Ultra 基线和补丁 | K50 Ultra OS2.0.209.0.VLFCNXM；小米 12S Ultra OS2.0.208.0.VLACNXM；Note 12 Turbo OS2.0.210.0.VMRCNXM |
| [70055181](https://www.coolapk.com/feed/70055181) | r17.5；小米 14 OS3.0.300.3.WNCCNXM Beta；补丁 2026-01-01 | K60 OS2.0.220.0.VMNCNXM；K60 Pro OS2.0.210.0.VMKCNXM；K70 Pro OS3.0.7.0.WNMCNXM；小米 13 OS3.0.2.0.WMCCNXM |
| [70204140](https://www.coolapk.com/feed/70204140) | r17.6；同上小米 14 基线和补丁 | K60 OS2.0.220.0.VMNCNXM；K60 Pro OS2.0.210.0.VMKCNXM；小米 13 Pro OS3.0.3.0.WMBCNXM；小米 14 Pro OS3.0.8.0.WNBCNXM |

69797002 的小米 13 Pro 后缀 `WMBCMXM` 按原文保留，是否笔误未核实；69691088 的问号不能自行补成特定机型。各帖均提及 5.10 内核设备不支持 LHDC。r17 帖警告移除 Kiwi 浏览器可能导致浏览器数据丢失；r17.5 帖称天气与相册不会自动覆盖，需从 `/product/data-app` 安装相应应用。r17.6 帖明确称不支持从官方 3.0.300.5 直接刷入，其 eSIM 适配也分为支持、待测试与不可用，不应扩大到所有机型。

原帖要求解锁 Bootloader、使用对应机型包，称不支持从解密包直接刷入；r16.3 帖另要求 A 槽。帖子列有移除温控、签名验证等改动，刷入前应核对设备、包及恢复方式并备份。

## 下载入口

- 69691088、69797002、69820474 正文给出 [OpenList 入口](https://oss.alioss.us.kg/139)、[139 云盘](https://yun.139.com/shareweb/#/w/i/2s2xRUuKPXCw9) 和 [123 云盘](https://www.123684.com/s/lUXDVv-724Wh)；r17 两帖称目录为 `/机型+ROM/OS3.0.a16/r17`。
- 70055181 正文及 70204140 作者评论给出 [OpenList 入口](https://oss.1mxy.cn/139)、同一 [139 云盘](https://yun.139.com/shareweb/#/w/i/2s2xRUuKPXCw9) 和 [123 云盘](https://www.123684.com/s/lUXDVv-724Wh)，目录分别为 `/机型+ROM/OS3.0.a16/r17.5` 与 `/机型+ROM/OS3.0.a16/r17.6`。

原帖称 r17 后 ZIP 名末十位对应 MD5 末十位；这是截断校验线索，不等于完整可信校验值。未发现公开提取码，页面和文件未验证；图片及完整评论未读取。

## r16.3：K50 Ultra / 12S Ultra / Note 12 Turbo

[69701000](https://www.coolapk.com/feed/69701000)称基于小米 13 的 OS3.0.2.0.WMCCNXM、Android 16，补丁 2025-11-01；底包分别为 K50 Ultra OS2.0.209.0.VLFCNXM、小米 12S Ultra OS2.0.208.0.VLACNXM、Note 12 Turbo OS2.0.210.0.VMRCNXM。作者称调整容量策略并优化线刷脚本路径；电池效果和设备兼容性未独立验证。帖子列出 5.10 内核设备不支持 LHDC，要求仅刷对应机型、解锁 Bootloader，并称刷入仅限 A 槽，不支持从解密包直接刷入。刷机前确认实际包和恢复路径并备份。

正文称下载地址移至评论，已采集评论未找到作者提供的对应下载链接；原帖写目录 `/你的机型/OS3.0.a16/r16.3`，不代表具体文件可用。图片和完整评论未读取。
