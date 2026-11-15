# Termux 原生手搓 APK、多源无广天气 App 与仿澎湃 4 相册

整理自酷安社区开发者发布记录（未读图片与评论，未做实机或源码审查）。

## 1. `ZXToolbox`：Termux 纯命令行无 Gradle/Android Studio 构建的原生 App

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74936432)（`imgs=0`）
- **构建链路**：跳过 Android Studio 与 Gradle，手写 `AndroidManifest.xml`，直接在手机 Termux 终端中使用 `aapt + javac + d8 + apksigner` 完成编译、打包与签名。
- **功能构成**：内置 JNI 桥接的独立 Python 3.15 引擎（无需 Termux 即可执行 Python）、内置官方 `apksig` 本地离线签名（带同名覆盖防毁包保护）、Bootloader 锁与 Root 状态探针、强密码生成/简繁转换等工具。
- **公开入口**：官网 `https://zx.xn--6or.gay/`，开源仓库 `https://gitee.com/xzh_Tim/zx`。

## 2. `棉花糖天气 v1.3.0`：多数据源直连与自托管 VPS 代理池设计

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74935345)（`imgs=8`，支持 Android 8.0+，约 9.2 MB）
- **数据源策略**：
  - 默认内置免 Key 的小米天气公开接口（提示其长期稳定性无保证）；另支持和风天气（需填专属 `API Host` 与 `API Key`）、心知（私钥）、高德（Web 服务 Key）、华风爱科、OpenWeather（AppID）、WeatherAPI、Visual Crossing、AccuWeather 共 9 家，缺逐小时或空气质量字段时不混拼数据、直接隐藏卡片并在数据说明注明。
- **两种接入架构与省额度机制**：
  - **直连模式**：无需服务器，Key 存于手机端直连请求。
  - **自托管后台模式**：Docker Compose 运行后端服务（监听 `8787`，建议反向代理并开启 HTTPS，Web 管理页 `/admin` 使用 `ADMIN_TOKEN`，客户端使用独立访问令牌；状态持久化于 `data/state.json`），支持同源多 Key 轮换（额度耗尽 Key 默认 24h 重试）、跨数据源兜底顺序、5 分钟同城缓存、全源失败返回 6 小时内缓存；App 端 30 分钟内打开不重复联网，桌面小组件（2x2/4x2/4x4）默认 60 分钟刷新并在低电量推迟。

## 3. 仿澎湃 4 悬浮底栏相册（AI 辅助开发）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74924709)（`imgs=8`）
- **兼容性与特性**：针对老系统缺少 HyperOS 4 Rust 库无法安装新相册的问题，支持 Android 7+（Android 13 以下仅半透明无模糊，HyperOS 1–3 支持 MIUEX 悬浮底栏与渐进模糊），提供纯色无模糊风格切换、文件夹名模糊搜索、系统分享/移动与跳转小米系统图片编辑；因签名变更需先卸载旧版再安装。
