# MitsuhaHyper 版本与包名对照

> 以下是帖子明列的适配机型、版本、包名和构建耗时转录。Android 版本、安全补丁和发布日期是原帖提供的信息，未独立验证；包名及兼容性也未在本整理中实测。

| 来源 | 帖子标注的适配机型 / 代号 | ROM 版本 | 包类型 | 包名 | 构建耗时 | 原帖基线与日期 |
|---|---|---|---|---|---|---|
| [74457981](https://www.coolapk.com/feed/74457981) | Redmi K80（zorn） | OS4.0.0.13.XOKCNXM | Hook | `HFHook_Pure_REDMIK80_encrypted_OS4.0.0.13.XOKCNXM_8ae5cca4_Mitsuha.zip` | 0h 16m 23s | Android 17.0；安全补丁 2026-09-01；发布 2026-09-24 |
| [74457292](https://www.coolapk.com/feed/74457292) | Redmi K80 Pro（miro） | OS4.0.0.10.XOMCNXM | Hook | `HFHook_Pure_REDMIK80Pro_encrypted_OS4.0.0.10.XOMCNXM_7a4a5bf5_Mitsuha.zip` | 0h 24m 31s | Android 17.0；安全补丁 2026-09-01；发布 2026-09-22 |
| [74457291](https://www.coolapk.com/feed/74457291) | 正文写 Redmi K90 Ultra（warsaw）；标题却写「Xiaomi 15 系列」 | OS4.0.0.15.XHPCNXM | Hook | `HFHook_Pure_REDMIK90Ultra_encrypted_OS4.0.0.15.XHPCNXM_357d3185_Mitsuha.zip` | 0h 26m 31s | Android 17.0；安全补丁 2026-09-01；发布 2026-09-22 |
| [74445160](https://www.coolapk.com/feed/74445160) | Redmi K100 Pro（athens） | OS4.0.4.0.XPICNXM | Hook | `HFHook_Pure_REDMIK100Pro_encrypted_OS4.0.4.0.XPICNXM_d0a73b41_Mitsuha.zip` | 0h 22m 58s | Android 17.0；安全补丁 2026-10-01；发布 2026-10-17 |
| [74445160](https://www.coolapk.com/feed/74445160) | Redmi K100 ProMax（songyuan） | OS4.0.4.0.XGNCNXM | Hook | `HFHook_Pure_REDMIK100ProMax_encrypted_OS4.0.4.0.XGNCNXM_fc5beafb_Mitsuha.zip` | 0h 27m 32s | Android 17.0；安全补丁 2026-10-01；发布 2026-10-17 |
| [74445154](https://www.coolapk.com/feed/74445154) | Redmi K90 ProMax（myron） | OS4.0.4.0.XPMCNXM | Hook | `HFHook_Pure_REDMIK90ProMax_encrypted_OS4.0.4.0.XPMCNXM_9b4ef1a5_Mitsuha.zip` | 0h 23m 45s | Android 17.0；安全补丁 2026-10-01；发布 2026-10-17 |

## 明确的标题与正文冲突

74457291 的标题称「Xiaomi 15 系列」，但正文适配机型、代号、包名和标签指向 Redmi K90 Ultra（warsaw）。此处保留冲突记录；不得据标题推断该包适配小米 15，也不能在缺少独立证据时判定其适配小米 15。

## 版本与功能备注

- 74457981 更新说明称修复部分机型开启状态栏功耗显示时的系统界面崩溃，刷新间隔为 0.5–10 秒；同帖还提及歌词在线获取、双排状态栏信号显示、横屏隐藏前摄环形电量、关闭三合一图标后恢复原生图标，以及整包校验后复用、刷写加快、进度不再卡在 46%。这些是原帖描述，未独立验证。
- 74457292、74457291 也列出相同的在线更新、歌词与状态栏更新摘要。框架完整功能及重启规则见「Mitsuha Framework：功能与操作说明」。
- K100系列与K90 ProMax的OS4.0.4正文没有列出整首歌词、框架/HookManager在线更新及整包复用等更新摘要，不把K80系列这些条目当作这两个版本的逐项证明。
- 以上帖子未提供可核验的公开包下载链接；本表只记录包名，不表示已取得或验证安装包。

## 新增小米15、17与Redmi K90发布包

以下字段逐条依据正文包名行；同一帖列出两个补丁或发布日期却未逐机映射时，不自行分配。均为发布帖声明，未下载或实机验证。

| 来源 | 机型与代号 | ROM版本 | 类型 | 文件名 | 构建耗时 | 原帖共同基线 |
|---|---|---|---|---|---|---|
| [原帖74457288](https://www.coolapk.com/feed/74457288) | Xiaomi 15 Pro（haotian） | OS4.0.0.17.XOBCNXM | Hook | `HFHook_Pure_Xiaomi15Pro_encrypted_OS4.0.0.17.XOBCNXM_70b4da32_Mitsuha.zip` | 构建 0h 25m 45s | Android 17.0 · 安全补丁 2026-09-01 · 发布 2026-09-30 / 2026-09-22（含斜杠的日期未逐机对应） |
| [原帖74457288](https://www.coolapk.com/feed/74457288) | Xiaomi 15（dada） | OS4.0.0.19.XOCCNXM | Hook | `HFHook_Pure_Xiaomi15_encrypted_OS4.0.0.19.XOCCNXM_87d29cd9_Mitsuha.zip` | 构建 0h 26m 13s | Android 17.0 · 安全补丁 2026-09-01 · 发布 2026-09-30 / 2026-09-22（含斜杠的日期未逐机对应） |
| [原帖74457288](https://www.coolapk.com/feed/74457288) | Xiaomi 15 Ultra（xuanyuan） | OS4.0.0.16.XOACNXM | Hook | `HFHook_Pure_Xiaomi15Ultra_encrypted_OS4.0.0.16.XOACNXM_9a929536_Mitsuha.zip` | 构建 0h 27m 4s | Android 17.0 · 安全补丁 2026-09-01 · 发布 2026-09-30 / 2026-09-22（含斜杠的日期未逐机对应） |
| [原帖74457280](https://www.coolapk.com/feed/74457280) | Redmi K90（annibale） | OS4.0.4.0.XPKCNXM | Hook | `HFHook_Pure_REDMIK90_encrypted_OS4.0.4.0.XPKCNXM_ab2e9074_Mitsuha.zip` | 构建 0h 19m 31s | Android 17.0 · 安全补丁 2026-10-01 · 发布 2026-10-17 |
| [原帖74448619](https://www.coolapk.com/feed/74448619) | Xiaomi 17（pudding） | OS4.0.4.0.XPCCNXM | Hook | `HFHook_Pure_Xiaomi17_encrypted_OS4.0.4.0.XPCCNXM_5c6fe279_Mitsuha.zip` | 构建 0h 24m 31s | Android 17.0 · 安全补丁 2026-10-01 / 2026-09-01 · 发布 2026-10-17 / 2026-09-22（含斜杠的日期未逐机对应） |
| [原帖74448619](https://www.coolapk.com/feed/74448619) | Xiaomi 17 ProMax（popsicle） | OS4.0.4.0.XPBCNXM | Hook | `HFHook_Pure_Xiaomi17ProMax_encrypted_OS4.0.4.0.XPBCNXM_b3d25876_Mitsuha.zip` | 构建 0h 25m 45s | Android 17.0 · 安全补丁 2026-10-01 / 2026-09-01 · 发布 2026-10-17 / 2026-09-22（含斜杠的日期未逐机对应） |
| [原帖74448619](https://www.coolapk.com/feed/74448619) | Xiaomi 17 Ultra（nezha） | OS4.0.4.0.XPACNXM | Hook | `HFHook_Pure_Xiaomi17Ultra_encrypted_OS4.0.4.0.XPACNXM_6a72166c_Mitsuha.zip` | 构建 0h 26m 55s | Android 17.0 · 安全补丁 2026-10-01 / 2026-09-01 · 发布 2026-10-17 / 2026-09-22（含斜杠的日期未逐机对应） |
| [原帖74448619](https://www.coolapk.com/feed/74448619) | Xiaomi 17 Pro（pandora） | OS4.0.4.0.XBLCNXM | Hook | `HFHook_Pure_Xiaomi17Pro_encrypted_OS4.0.4.0.XBLCNXM_dd324f84_Mitsuha.zip` | 构建 0h 29m 2s | Android 17.0 · 安全补丁 2026-10-01 / 2026-09-01 · 发布 2026-10-17 / 2026-09-22（含斜杠的日期未逐机对应） |
| [原帖74448619](https://www.coolapk.com/feed/74448619) | Xiaomi 17 Max（byron） | OS4.0.0.22.XAFCNXM | Hook | `HFHook_Pure_Xiaomi17Max_encrypted_OS4.0.0.22.XAFCNXM_21409c58_Mitsuha.zip` | 构建 0h 28m 47s | Android 17.0 · 安全补丁 2026-10-01 / 2026-09-01 · 发布 2026-10-17 / 2026-09-22（含斜杠的日期未逐机对应） |

## 较早功能帖没有完整包名时

- [74401010](https://www.coolapk.com/feed/74401010)标题写Xiaomi15，正文写RedmiK90Ultra；不按标题补出小米15兼容性。
- [74401009](https://www.coolapk.com/feed/74401009)正文写RedmiK90；[74401008](https://www.coolapk.com/feed/74401008)列小米15/Pro/Ultra；两帖无完整包名和具体系统版本。
- [74397626](https://www.coolapk.com/feed/74397626)标题标OS4.0.2，正文列RedmiK100Pro/ProMax，无完整包名。
- 这四篇的模板功能说明未列在线整首歌词、框架与HookManager在线更新、整包复用等新增摘要；不因同为MitsuhaFramework就将较新条目倒灌给它们。
