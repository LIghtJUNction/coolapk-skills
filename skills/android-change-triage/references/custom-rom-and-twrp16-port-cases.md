# 第三方 ROM（EvoX / LineageOS 24）与 TWRP 16.0 适配记录

本参考整理酷安社区中水月雨 MIAD01（Evolution X / Android 16）、小米 17 Pro Max（LineageOS 24 / Android 17）及红米 Note 9 5G / Note 9T（TWRP 16.0）的适配特征、刷写与降级数据清除边界。图片、评论与外部安装包均未在本库下载或实机验证。

---

## 1. 水月雨 MIAD01 — Evolution X [11.12 20261112]（Android 16 Unofficial）

- **来源**: https://www.coolapk.com/feed/75047371
- **发布入口**: `https://github.com/Cirrest/MIAD01_EvolutionX_Update/releases`
- **关键特征**:
  - 基于 MediaTek ODM Tree 适配，预装 GMS，支持**回锁 Bootloader**（但**不可回锁 Root**；回锁 BL 后后续可通过本地升级包在系统更新中导入升级）。
  - 保留双 CS43131 HiFi DSP 直通链路、`Volume Master Control` 百级 DSP 调音与 `AudioConsole` 移植，内置 MiraVision、显示引擎与载波聚合设置。
  - **已知限制**: 相机 64MP 模式未适配；因未对预装应用预做 ART 编译，首次开机设置阶段存在临时卡顿与发热；因厂商未公开 Device Tree 与 Kernel 源码，暂不满足 Evolution X Official 要求。
- **刷机与救砖恢复**:
  - 需提前用多系统工具箱备份字库；线刷前先解压全部 `7z` 分段包（无需解压内部 `zip`）。恢复原厂可用备份字库中的 `flashall.bat` 或原厂救砖包（`MD-PH-001`）。

---

## 2. 小米 17 Pro Max — LineageOS 24 / Android 17（20261112 编译版）

- **来源**: https://www.coolapk.com/feed/75032043
- **基础构成**: 个人手动编译，底层沿用 `HyperOS 4.0.5.0` 驱动；提供 **ADB Sideload 卡刷包 + MiFlash Fastboot 线刷包**双版本及配套专属 Recovery。
- **内置与变更**: 内置完整 GMS、小米相机、谷歌浏览器、开发者选项 Root 开关；输入法替换为微信输入法；移除小米浏览器；背屏恢复官方背屏方案。
- **重点修复**: 修复 NFC 读写及公交/门禁卡异常、红外遥控发射、以及重启后输入正确锁屏密码仍报错的解锁 Bug。

---

## 3. 红米 Note 9 5G / Note 9T（`cannon` / `cannong`）— TWRP 16.0（YARP `twrp-16.0`）

- **来源**: https://www.coolapk.com/feed/75038263
- **发布入口**: `https://github.com/top-android/twrp-build/releases/tag/v26.11.13`（`recovery.img` MD5：`4780539593f6862f23cf3796176ac75d`）
- **适配范围与特性**:
  - 面向 `MT6853`（`cannon` 设备树沿用 `cannong` 软链接兼容），主目标为 Android 17 / LineageOS 24.0，向下兼容 Android 16 / 15。
  - 支持 Android 17 `metadata + FBE` 解密、MTK `haptic_pwm`（`/sys/devices/platform/haptic_pwm` + `AW8622`）、USB OTG（`vfat`/`exfat`/`ntfs`）、`ro.boot.slot_suffix` 为空时跳过 `SetActiveSlot`，并在编译期绕过 OTA `device/version assert`。
  - 已知外观瑕疵：启动 logo 后整屏白闪一次（GUI2 首次绘制时序）、深色主题状态栏顶部偶现白线（建议用浅色主题）。
- **安装与跨版本升降级限制**:
  - 该机为 **A-only 分区**，直接执行 `fastboot flash recovery recovery.img`。
  - **升级（A16 → A17）**: 直接刷入新版 TWRP 后再刷 ROM。
  - **降级（A17 → A16）**: A16 无法读取 A17 的 `spblob` / `CE key`，必须先清空加密数据：
    ```sh
    fastboot erase userdata
    fastboot erase metadata
    ```
    （或在官方 Recovery 执行 Factory reset），完成 A16 首次开机后再刷回对应版本 TWRP。
