# 欧加真系 SukiSU Ultra / AK3 Root 与环境隐藏流程线索

本记录整理自社区针对 OPPO / 真我 / 一加机型的 Root 与环境配置步骤，原帖未在本库实机复现，跨机型/跨系统版本兼容性未验证。

1. **前置与备份**：先解锁 Bootloader；刷入 AnyKernel3（AK3）会替换原版内核，操作前需备份原版 `boot.img`，并建议后续在取得 Root 后通过爱玩机工具箱等工具对基带分区做完整备份并导出至外部存储。
2. **刷入 AK3 内核**：
   - 已 Root 设备：可通过 Horizon Kernel Flasher 或 SukiSU Ultra 界面选择 AK3 文件刷入（注意确认对应 A/B 槽位）。
   - 未 Root 设备：可通过对应机型的 TWRP/OrangeFox 刷入（若无法解密 `/data`，需放置至对应可读目录或先修补镜像）。
3. **隐藏模块组合与路径**：
   - 模块清单参考：`tricky_store`、`tricky_store` 自动更新包名列表、`zygisk_next`（原帖提及 `1.2.9`）、`zygisk_lsposed`（未整合 SUSFS 时原帖建议去特征版）、`shamiko`（若 AK3 已整合 SUSFS 则无需重复安装）、救砖模块、以及 `susfs_for_kernelsu`（若带 SUSFS）。
   - `keybox.xml` 放置路径：`/data/adb/tricky_store/`；若特定检测报错篡改密钥，原帖提及在 `/data/adb/tricky_store/target.txt` 中移除对应包名。
- 来源：[Coolapk 帖子 67046466](https://www.coolapk.com/feed/67046466)。
