# 跨平台开发者自建工具：Maltoria、MoRead 与 Lazer

整理自酷安社区开发者发布记录（未读图片、外链下载页及源码）。

## 1. `Maltoria`：面向 Galgame 兼容性的跨平台 Wine 管理器

- **来源**：[酷安原帖](https://www.coolapk.com/feed/75031222)（`imgs=1`，官网 `https://daydalek.github.io/Maltoria/`）
- **定位与机制**：Fork 自 Whisky，支持 macOS 与 Linux；集成移植自 `ge-proton` 的多媒体改动与针对 Galgame 的定向补丁，用于解决 OP 无法播放、开幕黑屏、全屏异常等常见 Wine 兼容问题，并提供元数据刮削、逐游戏运行环境选择与超分功能。

## 2. `MoRead`：多平台电子书阅读器迭代要点

- **来源**：[酷安原帖](https://www.coolapk.com/feed/75006383)（`imgs=0`，下载页 `https://routinemiku.github.io/MoRead-Windows/download.html`）
- **跨平台覆盖**：Windows、Linux、Android、macOS、iOS、开源鸿蒙、网页端等 9 个平台。
- **关键工程优化**：
  - 书源切换耗时由 `1693ms` 降至 `82ms`（消除同步等待网络阻塞）。
  - 修复倒序章节目录错位（如 12 条倒序章节与 716 条真实目录混排导致的章节跳转偏差）及仅统计正文目录。
  - 阅读排版一屏由 17 行增至 23 行，侧栏由 7 项精简为 3 项，支持以单个 JSON 注册扩展面板并通过 `postMessage` 通信。

## 3. `Lazer`：Flutter 跨平台插件化音乐播放器

- **来源**：[酷安原帖](https://www.coolapk.com/feed/75003100)（`imgs=2`，GitHub `chuxuehaocai/Lazer`）
- **技术栈与覆盖**：基于 Flutter 构建，覆盖 Windows、Android、macOS、Linux、iOS 与 Web 端，支持通过插件扩展多个音乐源。
