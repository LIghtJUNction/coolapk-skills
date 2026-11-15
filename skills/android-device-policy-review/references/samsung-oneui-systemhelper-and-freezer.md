# 三星 OneUI 8.5 `system helper` 实证、遥测域名与 Muz·冰封 Freezer

本参考整理酷安社区中对国行三星 OneUI 8.5 内置 `system helper` 的签名与行为取证、联通/三星遥测屏蔽域名清单，以及基于 Shizuku 的开源系统组件冻结工具 Muz·冰封 Freezer 的边界说明。

---

## 1. OneUI 8.5 国行 `system helper` 实证与触发机制

- **来源**: https://www.coolapk.com/feed/74875406
- **签名与来源澄清**:
  - APK v2 签名主体为 `CN=NewDM, OU=RI, O=CU, L=BJ, ST=BJ, C=86`（中国联通研究院 New Device Management），属于厂商为满足中国联通《5G终端白皮书 - DM设备管理要求》入库入网认证而预置的定制客户端。
- **行为与日志取证**:
  - 代码集成应用列表枚举、`UsageStatsManager`（24 小时前台时长）、`NetworkStatsManager`（按 UID 统计蜂窝/Wi-Fi 流量），并与 IMEI/IMSI/ICCID、基站 `TAC`/`Cell ID`/`PCI`、信号强度及百度定位 SDK 绑定。
  - `dumpsys alarm` 显示 `GetSignalInfoReceiver` 以 `30 分钟`周期唤醒（已记录 115 次）；`AppInfoRegisterReceiver` 触发后 `38 秒`在 `AppOps` 留下 `GET_USAGE_STATS` 拒绝记录（说明曾尝试读取全机使用时长）；授予该权限并触发广播后，抓包显示其立即发起两次连续 HTTP POST 上报。
  - **网络条件绕过风险**: 代码判定为 `if (isUnicomNetwork || isWifiConnected)`（联通 MCC/MNC 为 `46001`/`46006`/`46009`/`46010`），即连接 Wi-Fi 时即便未插联通卡也满足执行前置条件。

---

## 2. `systemhelper` 缓解方案与遥测域名列表

- **来源**: https://www.coolapk.com/feed/74900348
- **两种免 Root 思路**:
  1. 本地 `Shizuku + Canta` 删除该组件（需注意不同机型/ROM 上是否存在系统级代发回退，参见 `carrier-dm-clients-and-systemhelper-mitigation.md`）。
  2. 本地 DNS 拦截名单屏蔽遥测域名。
- **原帖逆向/整理的域名清单**（含联通 DM 及国行三星 S23U 测试中列入的遥测域名，未在本库逐一核实服务端用途，屏蔽前需自行评估对正常业务/推送的影响）:
  - 联通相关：`dm.wo.com.cn`、`wo.com.cn`、`wo.cn`、`10010.com`、`10010.cn`、`chinaunicom.com`、`chinaunicom.cn`
  - 三星相关（原帖 S23U 测试清单）：`di.atlas.samsung.com`、`dqa.samsung.com`、`samsungdm.com`、`samsungdmroute.com`、`samsungidp.com`、`dir-apis.samsung.com.cn`、`diagmon.samsung.com`、`usability.samsung.com`、`samsungads.com`、`samsungpush.com`

---

## 3. Muz·冰封 Freezer：无 Root（Shizuku）系统组件冻结工具

- **来源**: https://www.coolapk.com/feed/74905510
- **基本属性**: 包名 `com.tuygu.freezer`，开源协议 `GPL-3.0-or-later`，不申请 `INTERNET` 网络权限、无广告与统计 SDK，所有操作基于 `pm` 命令且可随时解冻恢复。
- **高危操作提醒**: 冻结系统组件可能导致功能异常、推送失效、定位异常或 OTA 升级失败；建议每次仅冻结一个组件并观察一段时间，出问题及时解冻。
