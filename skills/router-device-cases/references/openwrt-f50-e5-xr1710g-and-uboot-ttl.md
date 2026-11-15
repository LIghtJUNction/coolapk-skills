# OpenWrt 固件（F50 / 容悦 E5 / LibWrt）、UFI-Gate 与 TTL 刷 U-Boot 指南

本参考整理酷安社区中关于中兴 F50 TF 卡 OpenWrt 25.12.5 + 8153B 转网口配置、容悦 E5 OpenWrt 修改版与信息屏应用、UFI-Gate v1.1.7–v1.1.9、LibWrt 25.12.2 大分区固件、XR1710G/XG2010G TTL 刷 U-Boot 及 TP-LINK WA933RE 穿墙实测的记录。

---

## 1. 中兴 F50 OpenWrt（TF 卡 / 双系统）与 USB 转网口设置

- **来源**:
  - https://www.coolapk.com/feed/74574428（OpenWrt `25.12.5` / 内核 `7.2.8` + 8153B 转网口）
  - https://www.coolapk.com/feed/74246133（安卓/OpenWrt 双系统切换与 Wi-Fi 同步特性）
- **实操要点**:
  - 装入 8GB TF 卡并使用面具刷入 OpenWrt 后，若需外接 `RTL8153B` USB 转网口：在 OpenWrt 蜂窝设备管理中将 USB 角色设为**主机模式（开机默认主机模式）**，并在 USB 网卡设置中勾选**自动将空闲网卡加入 LAN**，即可热插拔转网口并关闭机身 Wi-Fi 降功耗。
  - 双系统下 OpenWrt 不能单独修改 Wi-Fi 名称与密码（需与原系统同步）；切回安卓后可通过高级后台安装系统切换器无电脑切回 OpenWrt（OpenWrt 启动明显慢于原系统）。

---

## 2. 容悦 E5 OpenWrt 修改版（`e5-sleep8`）、信息屏应用与 UFI-Gate

- **来源**:
  - https://www.coolapk.com/feed/74429458（容悦 E5 OpenWrt 修改版，SHA-256 `029bd2bc86a70eee276e57231c0f6fd18bf73acf4b980191738682da830c1d2e`）
  - https://www.coolapk.com/feed/74379644（容悦 E5 OpenWrt 信息屏应用 5 个，音量键改调屏幕亮度）
  - https://www.coolapk.com/feed/74433503（`UFI-Gate` v1.1.7–v1.1.9：`github.com/nanbeiluduo/UFI-Gate`）
- **要点**:
  - 容悦 E5 修改版仅适用于容悦 E5，新装运行 `flash.cmd`，已装 OpenWrt 可在 LuCI `系统 -> E5 系统升级` 上传 zip 或用 `flash.cmd --update` 保留配置升级。
  - `UFI-Gate` 为 `UFI-Tools` 插件，v1.1.7–v1.1.9 新增管理员门户登录恢复、离线 TXT 恢复密钥及自定义 HTTPS 穿透远程审批入口（需自备穿透隧道）。

---

## 3. LibWrt `25.12.2`（内核 `6.12.103`）大分区 NSS 固件特性

- **来源**: https://www.coolapk.com/feed/73825573
- **内置组件**: 支持 NSS 硬件加速，内置 `luci-app-5gmodem`、iStore、DDNSTO、USB 打印服务器、SAMBA4、Argon 主题、SmartDNS、OpenList 与文件管理器。

---

## 4. Gemtek XR1710G 路由器与 XG2010G 光猫 TTL 刷入 U-Boot

- **来源**:
  - https://www.coolapk.com/feed/73747900（XR1710G TTL 刷 U-Boot，需 T10 螺丝刀拆机）
  - https://www.coolapk.com/feed/73587679（XG2010G 光猫 TTL 刷 `xg2010g-chainloader-web-final.itb`）
- **通用准备**: 准备 USB 转 TTL（如 `CH343`）、杜邦线、串口终端（MobaXterm/SecureCRT/minicom）与 TFTP 服务（TFTPD64，`192.168.255.1` 网段），先核对主板 TTL 引脚定义后再通电传输 U-Boot 镜像。

---

## 5. TP-LINK WA933RE 2.4G Wi-Fi 扩展器穿墙实测

- **来源**: https://www.coolapk.com/feed/74752058
- **测试条件与结果**: 主路由 `TP-LINK BE3630` 易展版，WA933RE 置于主路由旁时 2.4G 峰值 `100Mbps`、均值约 `70Mbps`；隔 4–5 堵墙约 `20m` 位置下，红米 K70 至尊版与一加 Ace 6 Ultra 单刷抖音/短剧基本不卡，边下载边快滑才会卡顿。
