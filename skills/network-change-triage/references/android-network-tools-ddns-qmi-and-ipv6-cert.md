# 安卓网络与信号工具（QMI 锁频 / RouteLink / `ddns-go-magisk` / FTP）及 IPv6 证书排查

本参考整理酷安社区中四款 Android 网络工具的功能与限制，以及一例“误以为 WebView 模块修好了证书、实为 IPv6 环境导致证书拉取失败”的排查纠偏记录。

---

## 1. 欧加真 QMI 频段锁定与网络信号分析工具

- **来源**: https://www.coolapk.com/feed/74689639
- **适用范围**: 欧加真高通/联发科设备（部分高级功能需 Root、LSPosed 或厂商系统接口）。
- **更新与功能**: 支持双卡网络面板、RSRP/RSRQ/SINR/RSSI 曲线、高通 QOS、路测打点，并新增 Material Compose 界面的 QMI 频段与小区控制（读取支持频段、切换 SA/NSA、锁定频段与小区，同步上游 `qcom-band-menu` 至 `d199690`，仅在 5G 模式变化时写入）。

---

## 2. RouteLink（OpenWrt 移动端）与 `ddns-go-magisk`

- **来源**:
  - https://www.coolapk.com/feed/74296788（RouteLink 开源仓库 `https://github.com/tsix2019/RouteLink`）
  - https://www.coolapk.com/feed/74240389（`ddns-go-magisk` 开源仓库 `https://github.com/lanyixuanli/ddns-go-magisk`）
- **要点**:
  - **RouteLink**: 支持 Android / iOS 与演示模式，提供局域网自动发现、实时网速/CPU/内存监控、Wi-Fi/DHCP/端口转发、SSH 终端与桌面小组件。
  - **`ddns-go-magisk`**: 兼容 Magisk / KernelSU / APatch / FolkPatch，内置看门狗保活与 WebUI（默认监听 `9876` 端口，KSU/APatch/FolkPatch 可直接点模块图标打开面板，Magisk 需浏览器访问 `手机IP:9876`）；需设备具备公网 IP 或 IPv6。

---

## 3. Android FTP 共享工具改进（Wi-Fi 5 峰值 ~30MB/s）

- **来源**: https://www.coolapk.com/feed/74032248
- **要点**: 基于 `ftp share/apache` 二开（AI 辅助），API 支持 Android 6–14（作者实测 Android 9/10/12/14），修复重命名与覆盖丢文件 Bug，增加 IP 白名单、IPv6 访问、Shizuku/无障碍保活与 Android 10–14 CPU 负载显示；Wi-Fi 5 下峰值传输约 `30MB/s`（原帖标注 30m），不建议用于替代专业生产 FTP 服务。

---

## 4. 排查纠偏：老版 App WebView 证书失效实为 IPv6 环境问题

- **来源**: https://www.coolapk.com/feed/74206203
- **误区复盘**: 原以为导入用户证书或替换 `mywebview` 模块拉取 AIA 证书链修复了某老版 App 商家页证书过期问题，后复盘发现在安装模块重启后外出切到了移动数据——真正原因在于当前 **IPv6 网络环境下拉取 AIA 证书链异常**。
- **有效解法**: 使用移动数据时在 APN 接入点中关闭 IPv6，使用 Wi-Fi 时在路由器后台关闭 IPv6 即可恢复（与是否更换 WebView 无关）。
