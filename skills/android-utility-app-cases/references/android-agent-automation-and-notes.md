# Android 智能体（Nyral / Orion）、定时自动化（AddinTimer）与轻量化工具案例

本参考整理酷安社区中关于国产 ROM 智能体后台保活架构（Nyral）、云端/本地双模式 Agent（Orion Agent）、现代 Android 复刻定时自动化（AddinTimer）、青龙 ncmm 控制端（NCMM 管家）、多平台取件码聚合（蜉蝣·取件通）及本地优先笔记（墨集笔记）的实现思路与边界。

---

## 1. Nyral（`io.github.aixtin.nyral`）：国产 ROM 智能体后台生存架构

- **来源**: https://www.coolapk.com/feed/74783689
- **开源仓库**: `github.com/aixtin/Nyral`
- **四个核心工程实践**:
  1. **软地基架构（能力与通道解耦）**: 组合六类半官方通道——无障碍服务、隐形输入法、悬浮窗、本地调试服务、通知/气泡确认壳、前台服务——作为在 ColorOS / HyperOS / MIUI 激进后台回收下的生存底座。
  2. **定时岗位机制**: 基于 `AlarmManager.setExactAndAllowWhileIdle` 穿透 Doze 模式，配合续期重排、任务会话绑定、预授权白名单与审计日志，实现无人值守定时唤醒。
  3. **跨厂商灵动岛接入**: 将 `targetSdk` 升至 `36`，遵循 Android 16 原生 **Live Updates API**（`promoted notifications`），使任务进度以标准通知通道上屏 ColorOS 流体云与 HyperOS 超级岛，无需悬浮窗权限或厂商白名单。
  4. **真机端到端验证**: 采用“修改 cron + UI 停用/启用触发真实闹钟”与“DebugServer 注入 + 通知/db 三方取证”进行回归验证。

---

## 2. Orion Agent：云端/本地双模式 Agent

- **来源**: https://www.coolapk.com/feed/74510180
- **核心取舍**: 针对本地 AI 易在锁屏或切后台后被国产系统杀后台中断长任务的问题，提供云端算力模式（轻量 1 核 1G / 高性能 2 核 4G）与本地模式一键切换，支持锁屏、切后台或关闭 App 后继续在云端跑定时/批量任务。

---

## 3. AddinTimer（`com.addintimer`）：Android 15 复刻版定时自动化

- **来源**: https://www.coolapk.com/feed/74977730
- **适配范围**: 最低 Android 8.0，实测兼容 Android 15（原 2015 年旧版因 `minSdk=4` 无法在现代系统安装运行）；保留定时开关 Wi-Fi/蓝牙/飞行模式、定时短信电话、整点报时、情景模式切换，采用 Material 3 深色界面，去验证并支持 Root 与 LSPosed。

---

## 4. NCMM 管家：青龙面板 `ncmm` 移动端控制壳

- **来源**: https://www.coolapk.com/feed/74530813
- **边界与隐私**: 仅为 `@3899` 开源 Go 项目 `ncmm`（参考 `github.com/3899/ncmm/blob/main/docs/qinglong.md`）的安卓前端壳；面板地址、密码、Cookie 均仅保存在本地，仅申请网络权限。

---

## 5. 蜉蝣·取件通：多平台取件码聚合与 AI 通知抓取

- **来源**: https://www.coolapk.com/feed/75012398
- **机制与系统联动**:
  - 聚合拼多多（支持一主二副 3 账号）、淘宝/菜鸟、京东取件码与身份码；针对抖音、小红书等无法登录抓取的应用，通过 DeepSeek/OpenAI 自定义大模型解析通知中的地址与取件码（API Key 仅存私有目录）。
  - 支持流体云/超级岛上屏、桌面小组件，并通过 Shizuku / Stellar 与后台加锁增强轮询保活。

---

## 6. 墨集笔记：本地优先 + WebDAV 同步设计

- **来源**: https://www.coolapk.com/feed/74862837
- **定位**: 无广告无推送无开屏弹窗，数据默认保存在本机，按需自行配置 WebDAV 同步，已上架手机端应用商店及鸿蒙端。
