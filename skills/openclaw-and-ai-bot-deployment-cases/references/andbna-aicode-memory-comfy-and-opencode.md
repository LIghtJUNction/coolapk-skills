# AndbnA-qbot、AiCode、AI 记忆中枢、Comfy 工作台与 OpenCode Termux

整理自酷安社区开发者发布记录（未读配图与外链仓库源码）。

## 1. `AndbnA-qbot`：基于 QQ 官方接口的 Android 独立 APK 多 Bot 管理器

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74787903)（`imgs=9`，开源 `https://github.com/wjljh/AndbnA-qbot`）
- **架构特点**：内置 `Python 3.13.16` + `Chaquopy 17.1.0`，直接接入 `q.qq.com` 官方 AppID/AppSecret 接口避开第三方协议掉线风控；加密存储凭证、隐藏最近任务卡片防误清、支持普通/ADB/Root 监控，实测运行 4 个 Bot 内存低于 300MB。

## 2. `AiCode`：集 AI Agent、PRoot Linux/SSH 与可视化 Git 于一体的 Android IDE

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74510509)（`imgs=4`，`https://github.com/jieapi/aicode`、`https://aicode.murk.top`）
- **能力矩阵**：内置免 Root/免 Termux PRoot Linux 与多标签终端及远程 SSH 模式；支持 OpenAI/Anthropic/Gemini 协议、MCP/技能/长期记忆、只读规划/正常/自动执行模式及修改前文件快照；提供彩色 Git 分支拓扑图与差异审查，可直接把构建出的 APK 发送到对话安装。

## 3. `AI 记忆中枢`：Tauri 2.0 + Rust + Vue 本地 AI 对话归档与局域网同步

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74661137)（`imgs=4`）
- **特性与踩坑**：支持 DeepSeek/ChatGPT/Kimi/豆包等 9 平台官方 JSON 导入去重、本地 TF-IDF 词云（过滤代码变量）及 PC 生成二维码手机 gzip 扫码同步；注明网页版 IndexedDB 懒加载导致 Chrome 扩展（连本地 `34567` 端口）仅能抓近期对话，全量迁移应导入官方 JSON。

## 4. `Comfy工作台 v0.3.0`：Android 原生 ComfyUI 客户端

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74454550)（`imgs=4`，GPL-3.0 `https://github.com/Mimikkoro1/comfy-workbench-android`）
- **前置条件**：纯本地工具无云端算力；电脑端 ComfyUI 需加 `--listen 0.0.0.0` 启动，且工作流必须导出为 **API 格式**（普通界面 JSON 会被拦截）；支持多服务器切换、前台服务后台跑图、Florence-2 文本工作流直出及可选 `gacha-server-lite` 随机抽卡服务。

## 5. Termux 原生直跑 `OpenCode CLI V2` 一键安装注意事项

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74613575)（`imgs=3`）
- **要点**：原生直跑无需 proot，内置 DNS 修复与更新接管；安装时需授予共享存储权限供 DNS 配置，安装后重开 Termux 会话使 `PATH` 生效，并关闭电池优化与放开后台子进程限制。
