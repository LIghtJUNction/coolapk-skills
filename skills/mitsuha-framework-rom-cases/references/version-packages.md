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
