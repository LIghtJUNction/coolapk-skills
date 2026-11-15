# Cannon / 小米 17 Pro Max / K30 5G / 三星 ArtisanROM 构建案例

整理自酷安社区开发者发布记录（未读配图与评论，未做实机刷入验证）。

## 1. Redmi Note 9 5G / 9T（`cannon`）LineageOS 系列修复记录

- **LineageOS 22.2 `v26.10.31`（Android 15，内核 `4.14.336`）**：[酷安原帖](https://www.coolapk.com/feed/74751255)（`imgs=0`）
  - 修复反复开关 Wi-Fi 或关闭热点时 Wi-Fi HAL 崩溃/tombstone。
  - 复刻 A13 行为：`WifiPickerTracker.handleWifiStateChangedAction()` 开 Wi-Fi 时通过 `conditionallyUpdateScanResults(true)` 秒刷可用网络列表；验证放宽缓存窗口到 300s 无法消除打开页面约 1s 的固有加载成本且会拖慢热点冷扫至 12s，故回退保持 ~5s。
  - 支持自定义热点私网网段（`10.0.0.0/8`、`172.16.0.0/12`、`192.168.0.0/16`，非私网仅 Toast 提示不强制拦截）。
- **LineageOS 22.2 `v26.10.30`（Android 15，内核 `4.14.336 #1`）**：[酷安原帖](https://www.coolapk.com/feed/74739608)（`imgs=0`）
  - 修复短信发送异常：原厂 A12 预编译 `com.mediatek.ims` 静态调用被移除的两个 `@hide` 方法（`SubscriptionManager.getSimStateForSlotIndex(int)` 与 `StorageManager.inCryptKeeperBounce()`）抛 `NoSuchMethodError` 导致进程反复崩溃；在 framework 补回兼容桩后恢复。
- **LineageOS 21 `v26.10.29`（Android 14，内核 `4.14.336 #1`）**：[酷安原帖](https://www.coolapk.com/feed/74690345)（`imgs=0`）
  - 规避闭源 `libwifi-hal.so` 在 `ap0` 存在时返回空句柄解引用崩溃（`SIGSEGV`）：芯片特性集改为开机进程级 static 缓存，`dumpsys wifi` 跳过向 HAL 查询 host wake reason。
  - 内核向 `4.14` 回填 `bpf_attr` 字段及 `BPF_MAP_TYPE_DEVMAP_HASH`（Linux 5.4 语义）以支持 A14 用户态 BPF 加载器与 KernelSU-Next。
- **LineageOS 21 构建基础与 4 分支说明**：[酷安原帖](https://www.coolapk.com/feed/74671045)（`imgs=0`，`https://github.com/top-android/releases/releases/latest`、`https://github.com/top-android/cannon-build`）
  - 基于 `xiaomi-mt6853-devs`（MIUI `13.0.3-20220802` 底包），提供无 KSU / KernelSU / KernelSU-Next / +SUSFS 四套内核分支；修复 Jelly 百度 `baiduboxapp://` 劫持、NTP 与 Captive-Portal 切国内节点；说明国行 `M2007J22C` 无 NFC 硬件、EN-DC 限制下需工程模式锁 NR-only 开 5G。

## 2. 小米 17 Pro Max：LineageOS 24（Android 17）自编译与配套 Recovery

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74688477)（`imgs=9`）
- **摘要**：基于 GitHub 开源源码与 HyperOS `4.0.5.0` 底层驱动编译，内置小米相机（作者称相机专项适配耗时 20 余天）、小米浏览器、开发者选项内置 Root 开关及配套自定义 Recovery。

## 3. 红米 K30 5G：SukiSUUltra 自编译内核（MIUI 14 / A12）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74548155)（`imgs=2`，仓库 `https://github.com/acgmnc/SukiSU-Redmi-K30-5G-Driftwood`）
- **摘要**：针对老机型缺少维护自编译 SukiSUUltra，在 A12 MIUI 14 成功开机并修复管理器“更新 app profile 失败”提示；仍存在偶发性软重启问题。

## 4. 三星 Galaxy：`ArtisanROM Aether`（One UI 二开项目）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74526296)（`imgs=3`，`https://github.com/ArtisanROM/ArtisanROM`、基于 `https://github.com/UN1CA`）
- **摘要**：面向三星 Galaxy 的免费社区 One UI 第三方固件（开发中），作者注明不负责系统完整性/硬件认证/Root 检测隐藏。
