# 阿权 Lite 官改 ROM 规范与华为 P30 短接 COM 1.0 降级解锁

> 整理自酷安公开动态正文；图片、评论及外链未逐一核验，实际操作请以对应硬件版本与上游仓库为准。

- **阿权 Lite HyperOS 官改 ROM 特性规范与 Fastboot 刷入指南**
  - [酷安原帖](https://www.coolapk.com/feed/75038628)（`imgs=3`，配图未读）
  - **系统级底层修改**：精简 217 项内置应用（提供含小爱常规版与深度精简版）；全局移除 AVB 完整性校验、BL 锁状态伪装与 vbmeta 机型重签；集成核心破解（接管 V1/V2/V3 验签、签名比对与跨 sharedUserId 降级覆盖安装）；系统应用全局 4 字节 zipalign 对齐；解除温控限频降亮及 1Hz 全屏 AOD 注入。
  - **高级工具箱架构**：免 Magisk/Zygisk 框架注入通道，系统启动即加载；支持批量 dex2oat 编译已安装应用（支持断点续编与产物清理）；集成旁路供电、PPS 公版快充及定量断充。
  - **Fastboot 刷入与避坑流程**：
    - 电脑安装 drive 驱动，手机进 Fastboot 模式连接执行 bat 批处理；
    - 首次刷入必须按 N 格式化刷入，并在选项中选择 1无Root / 2Magisk / 3KSU；开机后手动安装配套 APK 补全环境；
    - 明确指出 KSU 若使用元模块（Meta Module），必须在 KSU 设置中开启「隐藏 SELinux 修改」，否则元模块修改上下文会导致开机卡二屏。

- **华为 P30 (ELE-AL00 Kirin 980) 拆机短接写底层、降级 EMUI 9 解锁 BL 与 Root**
  - [酷安原帖](https://www.coolapk.com/feed/75031276)（`imgs=9`，短接与拆机图未读，资源库：`https://www.yhcres.top/`）
  - **硬件前置**：必须保证屏幕为原装屏幕，第三方改换屏幕执行短接写底层会导致不可逆黑砖。
  - **短接 COM 1.0 与写底层**：拆机断开电池排线，镊子短接主板测试黄点与屏蔽罩金属外壳，连接电脑识别为 `HUAWEI USB COM 1.0`（驱动异常时手动指定华为 1.0 驱动 inf）；使用 Kirin-Tool v2.4.2（需 .NET）选择 Kirin 980 执行 Unlock 或使用夏天工具箱加载 980 底层；读取 Device Model 确认机型代码必须匹配 ELE-AL00。
  - **四包逐步降级与解锁**：使用猎人线刷工具（HW-auto-txt）按「鸿蒙 2 → EMUI 10 → EMUI 9」顺序四包（base+cust+preload+ptable）降级；降至 EMUI 9 后再次短接加载底层，终端执行 `fastboot oem unlock` 完成 BL 解锁。
  - **Recovery 面具修补**：
    - 方式 1：HuaweiUpdateExtractor 解包 EMUI 9 的 `recovery_ramdis`，传至手机用 Magisk 修补后通过 `fastboot flash recovery_ramdisk <修补文件>` 刷入，拔线长按音量上+电源键进入 recovery 执行一次恢复出厂设置固化分区；
    - 方式 2：Fastboot 刷入 710.img 到 recovery_ramdisk，进入 TWRP 经由 `adb sideload` 刷入面具 zip。注意严禁进入华为官方 Recovery，否则 Root 会被还原失效。
