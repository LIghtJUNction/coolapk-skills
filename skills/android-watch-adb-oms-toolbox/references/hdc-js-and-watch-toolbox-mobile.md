# 鸿蒙 HDC 协议逆向（`hdc-js`）与通用手表工具箱手机版

整理自酷安社区开发者发布与实践记录（未读图片、评论及 GitHub 仓库源码）。

## 1. 无 Root/无 PC 在 Android 上逆向并重实现鸿蒙 HDC 客户端（`hdc-js`）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74986444)（`imgs=2`）
- **目标与环境壁垒**：在 `aarch64` Android 平板上无 Root 运行 DevEco CLI / 鸿蒙 HDC 遇到三层限制：官方仅提供 macOS/Windows/x86_64 Linux 二进制；App 内置 Node 缺少 `npm`；Android 29+ W^X 与 SELinux `untrusted_app` 域禁止直接 `exec` App 私有目录或 `/data/local/tmp` 写入的可执行文件（`EACCES`）。
- **六阶段排障与交付路径**：
  1. **Node 自举 npm**：用内置 `https` 拉取 `registry.npmjs.org/npm` tarball，手写 60 行 tar 解析器（处理 `ustar`/GNU longname/PAX）解包运行 `npm-cli.js install`（`12.2.2`）。
  2. **提取二进制受阻**：从 2.38 GB SDK 提取的二进制在 Android 上死于 UART 线程及无 Root USB/exec 权限限制。
  3. **协议逆向与纯 JS 重写**：读取官方头文件，还原 108 字节大端头 + protobuf 协议结构，写出 257 行纯 JS 客户端/服务端与自环测试，并整理 139 行协议白皮书与 168 行排障手册。
  4. **跨 UID 桥与开源**：通过跨 UID IPC 桥接接回 CLI 调用；最终代码与文档开源至 `github.com/w1663/hdc-js`（MIT）。

## 2. 通用手表工具箱（手机版 v3.1.193）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74880005)（`imgs=5`）
- **连接与双协议模式**：
  - 支持同 Wi-Fi 自动扫描 IP、相机扫码连接（支持 `nt://` 协议）、USB OTG 有线免 Root、以及顶部切换「其他安卓（ADB）」与「华为鸿蒙（HDC）」双面板（自动屏蔽非对应品牌专属功能）。
- **保活与核心功能**：
  - 温和/标准/加强三档保活守护，断线或进程被杀后自动重连，支持开机自启。
  - 批量安装任意 APK 与无线安装提速（作者实测 188MB 微信约 54 秒完成，实时显示网速）。
  - 应用冻结/卸载/DPI 调整、屏幕亮度/分辨率/常亮、双端保存截屏与录屏、嵌入式 Wi-Fi 文件浏览器、微信快捷页、ADB/HDC Shell 终端与中文设备验机诊断。
