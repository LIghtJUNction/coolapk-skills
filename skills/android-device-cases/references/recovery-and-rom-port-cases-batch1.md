# Recovery 与第三方 ROM 移植案例（2026-11-15 批次 1）

整理自酷安社区开发者发布记录（未读图片、评论及外链目标，未做实机刷写验证）。

## 1. 酷比魔方 iPlay 50 Pro：TWRP 解密 Data 适配首版

- **来源**：[酷安原帖](https://www.coolapk.com/feed/75047414)（`imgs=9`）
- **测试状态（256GB 机型实测，128GB 自测）**：
  - 正常：ADB、MTP、Data 解密（数字/字母混合密码，图案未测）、触摸、`bootctrl`、备份（恢复未测）；Fastbootd 未测试刷入。
- **前提与刷入分区**：
  - 必须使用**国内版 `20240328` 底包**，且未刷过米系包、未修改 `vendor` 与安全补丁级别（国际版可能无法开机或无法解密）。
  - 需先解锁 Bootloader、关闭 AVB，将 TWRP 刷入 `vendor_boot_a` 分区。
- **使用与防砖注意事项**：
  - 首次进入会在 TWRP Logo 停留约 30 秒至 1–5 分钟（视设备文件数而定，常见约 40 秒）后出现密码解密界面；中文可先点设置语言切换。
  - 解密完成后需等待 10–20 秒让 `data` 重挂载完成（可进高级 → 文件管理 → `tmp` 查看是否存在 `remount_Success` 文件），否则内置存储可能乱码。
  - **严禁在重启菜单切换 AB 槽位**（按作者说明会瞬间变砖）。
  - `system_wipe` 标蓝分区无法挂载属正常（用于刷 GSI 前在高级清除勾选格式化 system，格式化后无法直接开机）。
  - USB 大容量存储模式仅支持 Linux 挂载（Windows 无法识别），在 TWRP 点卸载前必须先在 Linux 电脑端解除挂载，否则有数据丢失或 `data` 文件系统损坏风险。
  - 设置振动提示“振动器已停用”属正常，该设备无振动马达。

## 2. 联想小新 Pad Pro 12.7 2025（TB375FC）：DerpFest Android 17 与全套开源树

- **来源**：[酷安原帖](https://www.coolapk.com/feed/75033416)（`imgs=5`）
- **公开仓库与下载入口（原帖附带外链，未逐一打开验证）**：
  - 博客原文：`https://blog.mykeyvans.com/article/lenovo-tb375fc-derpfest-android17/`
  - 设备树 / 厂商树：`etnperlong/android_device_lenovo_TB375FC`（`derpfest-17`）、`etnperlong/android_vendor_lenovo_TB375FC`（`derpfest-17`）
  - OrangeFox 设备树：`etnperlong/orangefox_device_lenovo_TB375FC`
  - 内核与构建工具：`etnperlong/android_gki_kernel_6.1_optimistic`、`etnperlong/GKI_Optimistic_Kernel_Builder`、`fatalcoder524/KernelFlasher`、`trinadhthatakula/Bolt`
  - ROM 镜像：`DerpFest-v17-20261108-TB375FC-SPFT.7z`（SourceForge / Mykeyvans Drive）

## 3. 小米平板 6 Max 14（yudi）：OrangeFox R12.0 设备树与适配说明

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74917822)（`imgs=0`）
- **基础信息**：
  - 机型：小米平板 6 Max 14（`yudi`）；版本：OrangeFox R12.0，基于 `fox_16.0`（Android 16）。
  - 验证环境：HyperOS 3 `OS3.0.306`（Android 16，做梦书移植版安全补丁级别）；设备树开源于 `https://github.com/OneFeiFan/device_xiaomi_yudi`。
- **适配矩阵**：
  - 已验证：启动、触摸、震动、电量/充电/CPU 温度、ADB、Windows 双向 MTP、中文界面与等宽字体、设置重启保留、Data 解密（无锁屏/图案/PIN/文本/多用户/应用双开 profile）、双清与格式化 data、fastbootd、OTG。
  - 部分验证：zip 刷入（测试包/通用卡刷包正常，官方完整包与 OTA 未测）、备份/恢复（`boot`/`data` 正常，`super` 仅测备份）、槽位切换（双槽 rec 均可进，切槽启系统未测）。
  - 未验证：分区修复与调整、禁用 `vbmeta` AVB2.0；已关闭：USB Storage（仅单 USB-C 口）。
- **关键约束与刷入命令**：
  - 解密密钥策略与 `OS3.0.306` 安全补丁级别绑定，其他版本 ROM 可能无法解密需改配置重编。
  - 双清/格式化 data 会导致系统分身与双开用户数据无法通过 data 备份恢复；data 备份不含内部存储。
  - Recovery **不带内核**，依赖 `boot` 分区内核启动（已在 5.10.272 自编 GKI 测试），因此**不能使用 `fastboot boot` 临时启动**，需直接刷入 `recovery_a` / `recovery_b`；刷官方完整包后会覆盖对侧槽位 rec，需重刷。

## 4. 联想小新 Pad 2020（TB-J606F / bengal）：LineageOS 23.2（Android 16）EXT4 GSI

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74866109)（`imgs=0`）
- **实测组合**：
  - 镜像版本：`23.2-20260524-VANILLA-EXT4-GSI`（MisterZtr GSI：`https://sourceforge.net/projects/misterztr-gsi/files/LineageOS/Android%2016/`）
  - 内核：原厂 `4.19.95`；Vendor：原厂国行 Android 10（VNDK 29）；SELinux：`Enforcing`。
- **关键坑点**：MisterZtr 同时提供 `EROFS` 与 `EXT4` 两种变体，其中 `EROFS` 变体砍掉了 VNDK 28/29 支持，老机型（VNDK 29）必须选用 `EXT4` 变体才能正常引导。

## 5. Redmi Note 9 5G / 9T（cannon）：LineageOS 23.2 & 22.2 与 MTK 纯 LTE VoLTE 修复

- **来源**：
  - LineageOS 23.2（Android 16）`v26.11.2.1`：[酷安原帖](https://www.coolapk.com/feed/74803350)（`imgs=0`，`https://github.com/top-android/releases/releases/tag/v26.11.2.1`）
  - LineageOS 22.2（Android 15）`v26.11.2`：[酷安原帖](https://www.coolapk.com/feed/74799234)（`imgs=0`）
- **核心修复（两者共用）**：
  - 问题：MTK 平台纯 LTE 冷启动时 IMS 不注册（VoLTE 通话/短信不可用），A15 早期需等待约 6 分钟或手动开关飞行模式。
  - 根因与修复：MTK `+EIMSCFG` 关→开指令需 ≥5s 才能落定，原开机时序仅间隔 3s 被 modem 合并丢弃；在 `ImsPhoneCallTracker` 增加“开机自动重放（关→开 VoLTE，间隔 6s）”后，开机约 15s 自动注册（`mVoiceRegState=IN_SERVICE`）。
- **版本差异与资产**：
  - `23.2-20261102-UNOFFICIAL-cannon`：升级至 Android 16，内核 `4.14.336`，提供 `user` / `userdebug` 非 AB sideload 全量包、配套 `recovery-userdebug-enableadb.img` 及 `boot-a16-next-susfs-new-20261102.img`。
  - `22.2-20261102-UNOFFICIAL-cannon`：Android 15 `user` 变体 `releasekey` 签名（`1,272,643,633` 字节，SHA256 `0cf47b6bcbc1938f45a82795545b26878aa6dbaabd4a975089599df92c9f2a0e`），继承 WiFi HAL 防崩溃、热点自定义网段等修复；`ro.boot.hwc=CN` 下无 NFC 开关属正常。
