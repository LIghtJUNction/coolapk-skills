# 小米平板 6S Pro/5 Linux 生态、LG V30 postmarketOS、ZeroPanel 与树莓派 4B

整理自酷安社区开发者移植与实验记录（未读配图、评论与外部仓库全量代码）。

## 1. 小米平板 6S Pro（`sheng`）多发行版 Linux 生态索引

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74271219)（`imgs=0`）
- **开源仓库汇总**：`ianchb/debian-sheng`、`code002-2/ubuntu-sheng`、`mumuxiao722/fedora-sheng`、`lzxcr/arch-xiaomi-sheng`、`DotRedstone/nixos-sheng`、`alghiffaryfa19/sheng-pmos-builds`、`code002-2/armada-sheng`、`code002-2/steamos-sheng`，以及底座与 Recovery（`map220v/android_device_xiaomi_sheng`、`NoobMaster233/twrp-sheng-build`、`UotanToolboxWebsite`），并附国内软件源/Flatpak/如意玲珑（Linyaps）配置入口。

## 2. 小米平板 5（`nabu`）Arch Linux ARM + FEX-Emu + Steam 笔记

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74078847)（`imgs=0`，`https://github.com/changkrstr/nabu_archlinux_notes`）
- **摘要**：整理清华 Arch Linux ARM/ArchLinuxCN 镜像、`paru`、Flathub 及 FEX-Emu（`InstallFEX.py`）与 Steam deb 部署配置链接。

## 3. LG V30（韩版 `v300l`）：postmarketOS（Alpine）移植与矿渣对比

- **来源**：[酷安原帖](https://www.coolapk.com/feed/73272562)（`imgs=5`，Geekbench 6 `https://browser.geekbench.com/v6/cpu/18999213`）
- **摘要**：作者将其与 S805/S905/S912（玩客云 OEC）和树莓派 4（BCM2711）对比，指出二手 LG V30 自带屏幕与电池 UPS 且性能更高；提供 `downstream-nogui`（保留触控与 framebuffer）与桌面版卡刷包。

## 4. `ZeroPanel`：Termux 原生与 Proot（Ubuntu/Debian）建站面板

- **来源**：[酷安原帖](https://www.coolapk.com/feed/73070570)（`imgs=6`，`https://github.com/2136206076/ZeroPanel`，面板默认 `http://localhost:5000`）
- **双版本差异**：集成 Nginx + MariaDB + PHP-FPM；Termux 原生轻量版支持网站/数据库/文件管理、监控与云更新；Proot 高级版额外支持在线文件编辑/解压、多 PHP 版本与扩展安装、伪静态、独立数据库与 `crontab` 定时任务。

## 5. 树莓派 4B（2GB）+ X825 SATA 转接板 + iStoreOS 闲置硬件方案

- **来源**：[酷安原帖](https://www.coolapk.com/feed/72559858)（`imgs=6`）
- **实测与建议**：BCM2711（2GB RAM）刷入基于 OpenWrt 的 iStoreOS 仅占用约 400MB 内存，适合旁路由与轻量下载；因 4B 无 PCIe 接口需通过 USB 3.1 转 SATA（X825 成本约 145 元偏高），作者建议重度备份仍选原生 SATA 主板与 3.5 寸盘。
