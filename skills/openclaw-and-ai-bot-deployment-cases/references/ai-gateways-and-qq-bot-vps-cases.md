# AI Agent 本地网关与 QQ Bot Linux VPS 二开部署案例

整理自酷安社区开发者发布记录（未读配图、评论与上游源码）。

## 1. `dafeiyu-bot` 二开增强版：Linux VPS 无人值守部署与持久化改造

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74868520)（`imgs=7`，上游 `https://github.com/SAKURAfan1023/dafeiyu-bot`）
- **Linux 云端改造重点**：
  - 原版面向 macOS 桌面设计，依赖 macOS Keychain 导致在 Linux VPS 闪退；二开版改用 `0600` 权限本地文件隔离存储凭证。
  - 原版认证 Token 每次重启随机写入 URL Hash（`/#token`），导致反向代理、隧道与 systemd 失效；二开版改为固定访问地址 + 独立 Web 登录页（HttpOnly 会话 Cookie + 防暴力破解）。
  - 配套 Swift 6.1.3（Ubuntu 22.04）源码编译、NapCat（`127.0.0.1:6099/webui/`）扫码接入及 systemd 开机自启配置。

## 2. `OFM Gateway` Android 版与 Magisk 模块：本地 OpenAI 兼容网关

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74856227)（`imgs=0`，APK 发布页 `https://github.com/luoyi0914/ofm-gateway-android/releases/`，同源 Magisk/KernelSU 模块 `luoyi0914/ofm-gateway-magisk`）
- **机制与运行边界**：
  - 将上游 Our Free Model 的本地 HTTP 服务、Node 运行时与 Magpie 引擎打包为独立 APK（约 67 MB，仅 `arm64-v8a`，Android 8.0+），免 Root/Shizuku/Termux 运行，监听 `127.0.0.1:15000/v1`（局域网暴露需用 Root 模块版）。
  - Material 3 + Compose 四页界面（首页启停与端口检测、`/v1/models` 搜索复制、按日期滚动日志与预填 issue、开机自启设置），通过前台服务常驻保活（建议加入 ROM 后台白名单）；v0.8.3 起更换签名需先卸载旧测试包。不提供模型额度。

## 3. `DSH Go-su`：为 Android DeepSeekHarness 增加 Root Shell 工具

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74789332)（`imgs=2`，上游 `https://github.com/ccch1mneyyy/dsh-Go`）
- **改动说明**：原版 `dsh-Go` 1.0.8 虽然加入了 Root 免确认截图/点击，但无法执行 Shell；作者新增 Root 级 Shell 工具与对应启动开关，使 Agent 可执行任意目录读写、内核参数/系统属性修改与 `dd` 分区备份等操作（作者提示高风险，须查看原帖置顶注意事项后使用）。
