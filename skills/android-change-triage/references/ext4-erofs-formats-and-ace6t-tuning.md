# EXT4 / EROFS 镜像格式解析与一加 Ace 6T 风驰内核调校

本参考整理酷安社区中对固件工程师核心镜像格式（EXT4 与 EROFS）的结构与重打包校验要点，以及一加 Ace 6T 在 ColorOS 16.1 下基于 KernelSU 的帧率调校实测记录。

---

## 1. EXT4 与 EROFS 系统镜像解析及重打包注意点

- **来源**:
  - https://www.coolapk.com/feed/74011353（EXT4 磁盘布局、超级块、块组、inode、extent 与稀疏镜像）
  - https://www.coolapk.com/feed/74011325（EROFS 只读压缩文件系统磁盘布局与实战）
- **EXT4 固件要点**: Android 稀疏镜像常用 `.img`/`.simg`，解包/重打包常用 `make_ext4fs`、`img2simg`、`e2fsck`；裁剪或重建原厂 `system.img` 时必须保证块大小、inode 数量与 UUID 与原镜像对齐，否则会触发 `dm-verity` 或 AVB hash tree 校验失败。
- **EROFS 固件要点**: 华为于 2018 年开源并在 Linux 5.3 合入主线，自 Android 11 起成为只读分区（`system`、`system_ext`、`product`、`vendor`）主流格式，相比 EXT4 省去日志与冗余 inode 开销并提供只读压缩；修改后需按原分区特征与 AVB/dm-verity 规则重建，否则易启动失败。

---

## 2. 一加 Ace 6T（`PLR110`）KernelSU + 风驰内核调校 Scene 帧率实测

- **来源**: https://www.coolapk.com/feed/74739218
- **测试环境**: 一加 Ace 6T（`PLR110`，骁龙 8 Gen5，16GB，Android 16，ColorOS 16.1 紫标），基于 KernelSU 组合 29 个模块（自研 9 项 + 第三方 20 项）与风驰游戏内核调校。
- **Scene 实测数据**:
  - 《巅峰极速》（锁 60 帧，采样 ~100s）：平均 `60.0 FPS`，最低 `59.0 FPS`，抖动 `0.2%`，帧时间基准 `16–17ms`（峰值 `36ms`，无 `50ms` 级长帧）。
  - 《迷你世界》（锁 60 帧，联机 ~20min）：平均 `59.0 FPS`，最低 `53.0 FPS`，5% 低帧 `57 FPS`，抖动 `1.3%`。
