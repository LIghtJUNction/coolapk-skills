# 特定机型刷机、解工厂锁、GSI、9008 救砖与无障碍读屏实测

本参考整理酷安社区中关于华为/荣耀麒麟 970 解工厂锁、努比亚 NX563J GSI、联想 TB321FU 免解 BL 9008 刷 PixelOS 15、日版乐天 BIG 刷 Axon20 9008 救砖、Anker 投影仪参数边界，以及全盲用户视角的各 ROM 读屏无障碍实测记录。图片与外部包均未验证。

---

## 1. 荣耀 V10 / Mate10 / MateRS（EMUI 9.1）解工厂 BL 锁边界

- **来源**: https://www.coolapk.com/feed/74983750
- **验证机型与范围**: 荣耀 V10（`BKL-AL20`，EMUI 9.1）、MateRS（`NEO`）、Mate10（`ALP`）实测成功；荣耀 Play、Note10、Nova3 未确认。
- **核心前提与回锁规则**:
  - 需租用付费 NCK 加密狗配合 USB 重定向，配合 `Kirin-Tool`、Android Utility（AU）等工具。
  - **进 1.0 端口**: EMUI 8/9/10 多可用与系统版本相近的**免拆跳转包**（三键强刷进 `5%` 后在 `Kirin-Tool` `VCOM Operations -> Software Testpoint` 加载，成功后黑屏出现 `COM1.0`）；HarmonyOS 大概率无免拆包，需拆机短接。
  - **OTA 回锁警告**: 解工厂 BL 锁后，EMUI 之间 OTA（含海外 EMUI 12）不会回锁；但 **OTA 或代理升级至 HarmonyOS，以及 HarmonyOS 之间 OTA/华为助手升级均会回锁**，需在 Fastboot 用 AU 手动刷入官方包。
  - NCK 中 `Fastboot/Xloader` 页签对应选项：荣耀 V10 选 Kirin970 `V5`，Mate10 选 `V2`。

---

## 2. 联想 TB321FU（`tangorpro`）免解 BL 9008 刷入 PixelOS 15（Rootless）

- **来源**: https://www.coolapk.com/feed/74835499
- **项目入口**: `https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_ap3a.241005.015`
- **关键技术改造**:
  - 以 **9008 线刷包**形式在**不解锁 Bootloader** 的情况下刷入，全程 **Rootless**，清理 `phh`/`Mystic` GSI 痕迹与 `system` 可写挂载、关闭默认开启的 `adb root`、反编译移除开机内部一致性弹窗，并针对本机 **AVB 重签 `boot`**，使环境检测与原厂保持一致。
  - 移植一加 12 Android 15 ColorOS 的 **Tango** 实现 32 位 APK 兼容，并修复 SELinux 规则、蓝牙音频与 `DeviceConnectionService` 域问题。

---

## 3. 努比亚 NX563J Project Treble / GSI 刷机线索

- **来源**: https://www.coolapk.com/feed/74978642
- **主入口**: XDA 论坛 `nx563j-project-treble-android-10-11-12-13-14-15-16-17-gsi-ab.4194977`（原帖同时关联多篇酷安参考帖）；适用于替代原厂降亮度、半刷新率驱动体验不佳的 AOSP 小改系统，具体刷写细节需结合原帖引用贴核实。

---

## 4. 日版乐天 BIG（Axon20 5G 套娃机）9008 救砖与解 BL 代价

- **来源**: https://www.coolapk.com/feed/74566919
- **实测过程与不可逆副作用**:
  - 该机无实体 SIM 卡槽（仅 TF 扩展），直接刷工程 `abl` 无法解 BL；进 9008 用刷机匣备份全分区后刷入中兴 Axon20 MyOS12 救砖包可开机，随后再刷工程 `abl` 成功解锁 BL。
  - **已确认副作用**: 解 BL 后**指纹录入直接报错无法使用**（未解 BL 时可用），同时出现**基带未知**、刷新率锁定 `60Hz`、DTS 许可证报错（不影响放音）。

---

## 5. Anker 可乐投影仪（骁龙 210 / Android 7.1.2）硬件规格

- **来源**: https://www.coolapk.com/feed/74545838
- **实测软件层参数**: 骁龙 210（高通 210）、`Android 7.1.2`（API 25）、`armeabi-v7a`（32 位系统）、480p 投影分辨率、自带电池与遥控器（二手购入 170 元）。

---

## 6. 全盲用户视角下的国产 ROM 与类原生无障碍读屏演进实测

- **来源**: https://www.coolapk.com/feed/74897254
- **实测对比记录**:
  - **小米 9 / 小米 10（MIUI Android 11 vs Android 10 / 类原生）**: MIUI Android 11 触摸浏览体验明显退步，用户被迫降级 Android 10 或改刷 ArrowOS / Pixel Experience Plus / Evolution X / TequilaOS 等类原生以解决来消息提示音拉大读屏音量的问题。
  - **一加 12（2024 年底 vs 2025 年 5 月后更新）**: 早期桌面支持自定义操作（图标拖动、最近任务关闭应用）表现良好；2025 年 5 月系统为桌面与通知栏加入动画后无障碍大倒退，出现滚动列表翻页后图标无法触摸/浏览、自定义移动图标引发桌面崩溃或未实际移入文件夹、触摸通知栏亮度条导致 SystemUI 崩溃并锁屏要求输密码等故障。
