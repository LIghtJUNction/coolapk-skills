# GhostLock 592、Timer Root、MTK `lk.img` 解锁与 Fastbootd 工具案例

本参考整理酷安社区中关于特定内核内联 GhostLock 配置（Moto Razr 50 Ultra）、`timer root` UI5.0 805 推导配置、MTK 机型 `lk.img` 解锁/永久伪回锁工具箱，以及小米/红米自动化解包与 fastbootd 刷写工具的注意事项。图片与外部工具包均未在本库下载或验证。

---

## 1. GhostLock 1.2 (592)：Moto Razr 50 Ultra 内联内核配置

- **来源**: https://www.coolapk.com/feed/75014999
- **目标内核**: 摩托罗拉 Razr 50 Ultra `6.1.145-android14-11-geaa643a2c0ee-ab14763719`（`arm64-v8a`，Android 12+，包体约 `2.8 MB`）
- **配置变更**:
  - 早期配置仅含 `include` 引用，关键偏移字段为空，运行时需拼接外部片段导致命中不稳；`592` 版移除 `include` 改为完整内联写死：`kernel_phys_load = 2818572288`、`kernel_phys_offset = 2147483648`、`empty_zero_page = 35586048`，并补齐 `cred` 段与 `kernelsnitch` 的 `collisions`。
- **使用与兼容边界**:
  - 严格绑定精确内核构建版本，差一位均无法生效；首次建议完整备份，装过 `591` 版因签名变更需先卸载再安装。

---

## 2. 真我 GT5 Pro（UI5.0 805）`timer root` 临时 Root 流程

- **来源**: https://www.coolapk.com/feed/75009642
- **步骤**: 准备并启动 Shizuku → 下载与当前系统版本一致的 `boot.img` 备用 → 打开 `timer root` 并授予 Shizuku 权限 → 点击“推导配置”选中该 `boot.img` → 执行一键 Root + KernelSU（约 10 秒）→ 安装 KernelSU APK。

---

## 3. MTK 机型提权与 `lk.img` 解锁 / 永久伪回锁工具箱

- **来源**: https://www.coolapk.com/feed/74853407
- **原帖列出支持机型**: `k90max`、`k70u`、`turbo4`、`turbo5`、`k70e`、`turbo5max`、`k80u`、`k60u`、`kpad2`、`小米17tpro`、`kpad`（支持永久解锁与永久伪回锁可选）。
- **两条前置路径**:
  1. **未解 BL**: 使用提权软件给面具开启 shell 权限后，通过专属工具箱走 ADB 模式自动解锁。
  2. **已用过 `lk.img` 解锁**: 直接进入 Fastboot，使用工具箱 FB 模式自动处理。

---

## 4. 小米/红米自动化解包与 `fastbootd` 刷写工具注意事项

- **来源**: https://www.coolapk.com/feed/74171902
- **功能边界**: 自动化处理卡刷包解包、物理分区刷写及切换 `fastbootd` 刷逻辑分区；仅在小米/红米测试（支持 Android 10–17，作者声称理论上适用于 vivo/OPPO 但未实测）。
- **操作注意**: 卡刷包需放入工具内指定目录（「核心文件不能碰！」文件夹），需对杀毒软件误报添加信任并先看内附教程与救砖指南。
