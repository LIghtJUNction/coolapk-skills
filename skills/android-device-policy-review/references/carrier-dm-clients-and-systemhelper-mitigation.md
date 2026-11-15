# 运营商 DM 客户端分析与双层拦截实测（ColorOS / iQOO）

本参考整理酷安社区中关于三大运营商预装 DM（Device Management）客户端的逆向行为分析、Xposed + 内核双层拦截实测（ColorOS 16），以及在 iQOO 设备上针对 `systemhelper` 与 `devicereg` 仅做删除/冻结/断网时的实际局限。图片与评论未读，未在本库复现。

---

## 1. 三家运营商 DM 客户端机制与双层拦截实测（ColorOS 16, OPPO PLP110）

- **来源**: https://www.coolapk.com/feed/75049482
- **目标组件清单（均预装于 `priv-app`、无桌面界面，在开机/定时/SIM 变更时触发）**:
  1. **联通 `com.mobiletools.systemhelper`**（`uid 10100`，版本 `3.0.1`）：
     - 上报地址 `dm.wo.com.cn:18080`，单一调度入口 `e1.b.a()` 与 HTTP 出口 `g1.f.b()`（MD5 派生 AES key + RSA 签名）。
     - 采集五类信息：`DeviceInfo`（机型、系统版本、双卡槽、5G/VoLTE 开关）、`SigInfo`（RSRP/RSRQ/SINR/CQI、上下行速率、掉话/接入失败原因）、经纬度 + TAC/CID/PCI、`AppInfo`（应用清单、使用时长、逐应用流量），以及服务端下发的远程配置（存 `SharedPreferences`）。
     - 内置百度定位 SDK 的收发位于 JNI 层，Java 层无 hook 点，需内核层兜底。
  2. **移动 `com.coloros.regservice`**（`uid 10271`，版本 `16.0.20`）：
     - 神州泰岳（Richinfo）DM SDK，数据落盘 `cmcc_dm.db`；出口包含 `volley`、`HttpURLConnection` 及未加权限保护的 `ContentProvider`（`cn.richinfo.dm.provider.DMProvider`，其 `call` 方法可直接触发采集）。
     - 通过 IMSI 前缀（`46000`/`46002`/`46004`/`46007`/`46008`/`46020`）判定移动卡；除常规设备/网络信息外，还含网速、基站及终端锁定信息。
     - **隐藏特性**: `pm list packages` 默认不可见，需用 `dumpsys package` 查询；LSPosed 作用域界面无法直接勾选，原帖通过 `dbfix.dex` 直接写入 `modules_config.db`。
  3. **电信 `com.oppo.ctautoregist`**（`uid 10104`，版本 `16.1.5`）：
     - 除向 `zzhc.vnet.cn`、`typdm.tydevice.com` 发送自注册信息外，还包含 4 个短信服务（`ImsSmsSim1`/`2`、`SatelliteSmsSim1`/`2`），使用 `SmsManager.sendDataMessage` 发送 IMS/天通卫星注册短信（持有 `SEND_SMS`/`READ_SMS`/`RECEIVE_SMS` 权限）。**仅断网无法拦截其短信链路**；仅在插入电信卡时激活。
- **双层拦截方案与实测结果**:
  - **应用层（合并 Xposed 模块 `ShHook`/`RgsHook`/`CtaHook`）**: 分别伪装出口成功、短路总闸与门控、阻断定位与写库，并对电信 4 个发短信方法直接短路；作用域包含三个目标包及 `com.android.settings`（用于显示被隐藏包）。
  - **内核层（`carrier_dm_blocker`）**: 探测三个 `uid` 并 `REJECT` 全部 IPv4/IPv6 出站（含 60 秒自愈与开机补写 LSPosed 作用域）。
  - **主动触发验证**: `am startservice` 拉起电信 IMS SIM1 服务被 `L4 RegisterReceiver.onReceive blocked` 拦在入口；`content call` 调用移动 Provider 返回 `null`（`L5 provider call blocked`）。
- **已知限制**:
  - Hook 点依赖混淆类名（如 `e1`/`g1`/`f1`、`t.j`/`u.a`/`t.t`），与具体 APK 版本强绑定，OTA 后大概率失效；
  - 重装或升级导致 `uid` 变更后需重启重新探测内核规则；
  - 原帖自测脚本直接读取 `modules_config.db` 校验作用域（未依赖仅限电脑 adb root shell 的 `lspctl`）。

---

## 2. iQOO 设备上单纯删除 `systemhelper` 与禁用 `devicereg` 的对比观察

- **来源**: https://www.coolapk.com/feed/75032069
- **现象与观察边界**（原帖基于系统自带流量管理统计观察，未做抓包或内核防火墙拦截）:
  1. **`systemhelper`**: 作者在删除前观测其单月流量达 `108MB`（某日单日达 `41MB`）；删除/冻结/禁用联网后，后台仍出现流量消耗，只是统计归属转移到「系统联网服务」（占 98% 以上）与「已删除的程序」中。
  2. **`devicereg`（设备管理）**: 在无法使用 Dhizuku 删除的情况下，作者通过 Shizuku 配合蓝河工具箱禁用其联网权限后，连续 8 天未再观察到流量记录（对比此前平均每 2 天上传一次）。
- **结论差异**: 仅靠用户态“禁用联网/冻结/删包”在不同厂商和系统服务链路上的拦截效果不同；对于跨进程或系统服务代发的上报，单纯禁用单包联网往往不足。
