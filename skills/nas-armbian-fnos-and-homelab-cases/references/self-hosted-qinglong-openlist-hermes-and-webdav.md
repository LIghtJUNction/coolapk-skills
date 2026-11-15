# 自托管与手机端服务部署（Android 离线青龙、OpenList、Hermes Agent、WebDAV）

本参考整理酷安社区中关于手机本地 chroot 运行青龙面板、绿联 NAS 部署 Hermes Agent、OpenWrt 手机面板 3.0、纯血鸿蒙 OpenList 客户端、WorkBuddy/NAS 部署 `legado-reader` 及 Keepass2Android WebDAV 直连的架构与边界说明。

---

## 1. Android 离线 chroot 青龙面板（`v2.24.0` Magisk/KSU 模块）

- **来源**: https://www.coolapk.com/feed/74821302
- **架构与特点**:
  - 仅支持 `arm64` + Root（Magisk/KernelSU；KSU 使用标准模块脚本但未经 KSU 真机实测）。
  - 使用 `crane` 从 `whyour/qinglong:2.24.0` arm64 Alpine 镜像导出 rootfs，离线内置 Node.js `24.22`、Python `3.11`、青龙 `2.24.0` 与 SQLite 原生模块，无需联网下载依赖。
  - 数据持久化于 `/data/adb/qinglong/data`（升级不丢数据，**卸载模块会删除全部数据**），内置开机自启 + watchdog，提供 `start`/`stop`/`restart`/`status`/`open`/`log`/`shell`/`ql` 命令，面板默认监听 `0.0.0.0:5700`（本地 `127.0.0.1:5700`，电脑可用 `adb forward tcp:5700 tcp:5700`；以 root 身份运行，务必设置强密码）。

---

## 2. 绿联 NAS 部署 Hermes Agent 与 OpenWrt 手机面板 3.0

- **来源**:
  - https://www.coolapk.com/feed/74922752（绿联 NAS 部署 Hermes Agent 接入飞书、影音下载与音乐刮削）
  - https://www.coolapk.com/feed/74922028（OpenWrt 路由器手机面板 3.0）
- **要点**: `74922752` 记录在绿联 NAS 上配置 Hermes Agent 对接飞书机器人的全流程；`74922028` 提供装在 OpenWrt 路由器上的移动端管理面板 3.0。

---

## 3. Keepass2Android WebDAV 直连与纯血鸿蒙 OpenList / Legado 部署

- **来源**:
  - https://www.coolapk.com/feed/74914286（Keepass2Android WebDAV 直连）
  - https://www.coolapk.com/feed/74936669（纯血鸿蒙 ArkUI V2 原生 OpenList 客户端）
  - https://www.coolapk.com/feed/75003775（WorkBuddy / NAS / 树莓派部署 `legado-reader` Web 阅读器）
- **要点**:
  - Keepass2Android 直接通过 WebDAV（坚果云、Nextcloud、群晖/自建 NAS）读写 `.kdbx` 数据库（AES/ChaCha20/TwoFish + Argon2），关闭后不在本地留长期缓存，`v1.15-r6` 修复了 WebDAV 连接与非 ASCII URL 问题。
  - `75003775` 利用后端设备（WorkBuddy/云服务器/树莓派/NAS）执行 `github.com/zuomeizuoyang/legado-reader` 书源解析脚本，iOS 端可通过 Safari 添加到主屏幕全屏使用（部署后需自行导入书源）。
