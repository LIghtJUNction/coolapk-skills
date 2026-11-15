# AI 搓本地应用、Hermes 辅助小米 6 刷机与 OpenClaw 成本教训

整理自酷安社区开发者与用户实践复盘（未读配图，结论按原帖实测记录）。

## 1. AI 辅助构建 3 款本地 Android 小工具（含液态玻璃性能取舍）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/72670489)（`imgs=9`）
- **实践要点**：作者借助 AI（约 10 亿 token）开发了极简音乐播放器、接入 MiMo TTS API 的声音克隆/配音壳、以及免 Node.js 本地 JSON 角色卡聊天应用 `Aura Live`；指出实时模糊叠层“液态玻璃”视觉方案在兼容性和功耗上压力较大，正式版倾向保守渲染方案以降低功耗。

## 2. Hermes AI Agent 辅助小米 6 刷机（Android 15/16 与 4.4 内核 Root 坑点）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/72208720)（`imgs=9`）
- **实测流程与翻车复盘**：通过电脑连接小米 6 后，Hermes 自动读取机型/BL 状态并检索 LineageOS 22（A15）与 Evolution X（A16）固件（物理音量键确认仍需人工介入）；在 Root 阶段 AI 因经验主义误用新版 KSU 修补 `boot` 导致 bootloop（4.4 老内核不支持新版 KSU + 伪造 AVB 属性 + fastboot 超时），最终从同源 KSU 版 ROM 提取适配老内核的 `boot.img` 成功 Root，并完成动画缩放（`0.3x`）、后台限制与加密 DNS 优化。

## 3. OpenClaw 死循环成本教训与“专事专办”选型反思

- **来源**：[酷安原帖](https://www.coolapk.com/feed/70929650)（`imgs=9`）
- **复盘要点**：作者在 Windows/NAS/云端部署 OpenClaw 后遇到下载视频死循环单次消耗 14 元及瞬间跑满 80W tokens 欠费的情况，认为复杂通用 Agent 运维成本与不可控性高，日常确定性任务更适合专项工具（如 SillyTavern、n8n、NAS Docker 固定定时脚本）。
