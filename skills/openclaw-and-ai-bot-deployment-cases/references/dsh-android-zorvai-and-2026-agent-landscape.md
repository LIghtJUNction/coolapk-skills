# DeepSeek Harness 安卓移植、ZorvAI ACI 架构与 2026 国内智能体生态

整理自酷安社区开发者发布与行业梳理记录（未读配图与 GitHub 源码）。

## 1. DeepSeek Harness（`dsh` `0.2.0-rc.2`）Android 原生完整移植

- **来源**：[版本介绍](https://www.coolapk.com/feed/74131737)（`imgs=3`）与 [架构/工具链说明](https://www.coolapk.com/feed/74126509)（`imgs=4`）
- **实现架构**：内置 bionic 版 Node.js 与完整上游 `dsh`（`0.2.0-rc.2`）引擎、WebUI 及插件体系，免 Termux/Root 运行（按需切换 Shizuku/Root），冷启动 <10 秒、前台服务常驻、多服务并行内存 <400MB。
- **扩展中心**：内置 19 项开发环境（Python、Go、Rust、Clang、OpenJDK、Git、Ruby、Perl、Lua、SQLite、FFmpeg、ImageMagick、OpenSSH、`adb`、Android 构建工具、vim 等），通过国内镜像并发安装并做 SHA-256 校验，支持无障碍读屏/点击与 TTS。

## 2. `ZorvAI`：Android 本地优先 Agent 与 Binder IPC ACI 框架

- **来源**：[酷安原帖](https://www.coolapk.com/feed/73093527)（`imgs=0`，`https://github.com/Quor-a/ZorvAI`，Kotlin + Jetpack Compose，Apache-2.0）
- **核心概念澄清**：“本地优先”指控制逻辑在手机本地进程、工具调度通过 Android Binder IPC 本地调用同设备 App（ACI：Agent Capability Interface）、执行结果不出设备；模型推理本身仍可自由接入云端 API 或局域网/本地模型。

## 3. 2026 年国内 AI 智能体生态分类梳理

- **来源**：[酷安原帖](https://www.coolapk.com/feed/73330640)（`imgs=0`）
- **分类记录**：大厂深度集成（腾讯 WorkBuddy、阿里 QwenPaw / QoderWork、字节 Trae、月之暗面 KimiClaw、MiniMax MaxClaw、百度 DuClaw 等）与开源/轻量变种选型对比（具体参数与访问量数据均按原帖记录，未独立核查）。
