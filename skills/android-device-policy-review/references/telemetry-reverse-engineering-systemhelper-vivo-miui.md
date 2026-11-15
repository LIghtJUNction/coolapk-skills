# 系统遥测与隐私组件逆向分析（SystemHelper / `com.vivo.abe` / 山海引擎 / 手机管家）

本参考整理酷安社区中对运营商 DM 客户端（`com.mobiletools.systemhelper` / `com.miui.dmregservice`）、`com.vivo.abe`、OPPO/真我 AI LinkBoost（山海通信网络引擎）及 HyperOS 手机管家/安全组件的逆向分析与实机取证记录。所有结论均以原帖作者提供的测试样本与观察边界为限。

---

## 1. `SystemHelper_3.0.1-20260313.apk` 静态逆向与多方核对

- **来源**:
  - https://www.coolapk.com/feed/74890364（小米 `23113RKC6C` / HyperOS `OS3.0.307.0` 实机取证）
  - https://www.coolapk.com/feed/74911090（多方开源取证核对与纠偏）
  - https://www.coolapk.com/feed/74897263（反编译隐私问题摘要）
  - https://www.coolapk.com/feed/74966338（CU DM 自注册背景与 ENUM E.164 DNS 说明）
  - https://www.coolapk.com/feed/74880649（`/product/priv-app` `uid 10100` 客户端概要）
- **样本标识（`74890364`）**:
  - 文件名 `SystemHelper_3.0.1-20260313.apk`（`3831041` 字节，SHA-256 `1de8d965e295db27e189aa179cdb45438d7f759cffbb43ce55f840b614ff6c65`），包名 `com.mobiletools.systemhelper`，安装路径 `/system/priv-app/SystemHelper/SystemHelper.apk`（`uid 10100`），无加固壳。
- **核心行为与纠偏（`74911090` / `74897263`）**:
  - `SystemHelper` 通过 `BOOT_COMPLETED`（优先级 `2147483647`）与 `AlarmManager.setExactAndAllowWhileIdle()` 常驻，按每小时/周期采集定位、双卡 IMEI/IMSI/ICCID、`QUERY_ALL_PACKAGES` + `PACKAGE_USAGE_STATS` 及 `NetworkStats`，在 HTTPS 失败时存在 HTTP 明文降级逻辑（`http://dm.wo.com.cn:18080/register.wo`）。
  - **主语纠偏（`74911090`）**: Wi-Fi 扫描列表、蓝牙扫描列表、BSSID/SSID、路由器 MAC 与公网 IP 字段实为**移动 DM 组件（`com.miui.dmregservice`）**采集，而非联通 `SystemHelper`。
  - **触发条件与实测验证（`74911090`）**: 触发条件为「连着 Wi-Fi 且为国内网络，或无 Wi-Fi 但是联通网」（插移动卡连 Wi-Fi 亦会触发）；第三方 App（实测含 16 个常见包）确可通过 normal 级 `XSOF` 权限绑定小米 `MiSafetyDetectService` 查询风险分；撤回手机管家隐私政策后截图计数仍从 94 增至 95（仅证实本地入库，未证实录音或云端上传）。

---

## 2. HyperOS 4.0.0 组件逆向与 72 小时行为记录

- **来源**: https://www.coolapk.com/feed/74833524
- **逆向样本清单**: 手机管家 `21.0`、运营商服务 `16.0.5`、安全服务 `13.6.5`、SystemHelper `4.0.0 20260910`、Analytics `6.33.0`、系统安全组件 `3.1.4 20260902.0`、`MiTrustService 1.2.260901`。
- **新发现行为**:
  - 含录制 3 秒 `720×1280` MP4 视频并加密上传至 `detection-aigc.engine.miui.com` 进行 AI 检测的逻辑（需设备支持、AI 检测开关开启、同意协议并处于微信/QQ 视频及摄像头相关场景才会触发）。
  - 本地统计最近 72 小时截图时间戳、屏幕共享状态与时长、通话、飞行模式、呼叫转移及安装应用类别以计算风险分（截图与共享记录仅存时间/状态，不含画面）。

---

## 3. OPPO / 真我「山海通信网络引擎」（AI LinkBoost）拆解

- **来源**: https://www.coolapk.com/feed/75018868
- **测试设备**: `realme RMX8899`（真我 Neo8），系统 `RMX8899_17.0.0.106(CN01C10P02)`
- **架构与发现**:
  - 代码实为同一套 `AI LinkBoost` 框架（`TeleService` / `AiLinkBoostActivity`），按品牌与硬件档位映射不同营销名称（山海、AI LinkBoost、AI Netpilot、Smart Link、苍穹通信）。
  - 由本地链路优化（运营商 QoS/QCI，需游戏方+厂商+运营商三方到位，极难触发）与云端隧道加速两条互斥路径组成；`TeleService` 本地 SQLite 表会记录通勤时间（`timer=0` 集中于 07/12 点）、加密住址及基站数据。

---

## 4. iQOO `com.vivo.abe` 后台上报与待机耗电观察

- **来源**: https://www.coolapk.com/feed/74939320
- **样本与现象**: 未 Root 的 `iQOO Z10 Turbo+` 夜间待机掉电约 `8%`，排查发现预置系统组件 `com.vivo.abe`（普通用户不可见且无法直接卸载）持续向 `*.vivo.com.cn` 域名上报数据。
