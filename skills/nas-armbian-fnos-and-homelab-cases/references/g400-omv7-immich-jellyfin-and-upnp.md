# 老旧笔记本（联想 G400）OMV7 部署 Immich/Jellyfin、UPnP 投屏与 NovaPlayer

本参考整理酷安社区中老旧笔记本改造 NAS 运行 OMV7 的资源取舍、BubbleUPnP 软路由无损串流，以及面向单手刷剧的 NAS/网盘直连播放器 NovaPlayer 的记录。

---

## 1. 联想 G400（奔腾 G2020M / 16GB）OMV7 + Tailscale IPv6 部署取舍

- **来源**: https://www.coolapk.com/feed/73741632
- **硬件与网络**: 奔腾 `G2020M`（双核双线程）、`16GB DDR3L`、`240GB SSD`（系统）+ `512GB HDD` + `1TB PSSD`；通过 **Tailscale + IPv6 公网直连**上行实测约 `60Mbps`。
- **核心原则（用内存换 CPU、用 SSD 换机械盘）**:
  - **Immich**: 机器学习/人脸识别容器对 G2020M 压力极大，**必须关闭或限制并发**，否则首次传图即可导致系统死机。
  - **Jellyfin**: 核显比笔记本独显（AMD HD 8750M，GCN 1.0 无硬件编码优势且增加 7×24 功耗）更适合作为 NAS 基础显示/轻量支持。

---

## 2. 顺丰手表 + BubbleUPnP 无损投放与软路由解码方案

- **来源**: https://www.coolapk.com/feed/74287697
- **架构**: 手表端运行 `BubbleUPnP` 挂载 NAS（SMB / WebDAV），利用 UPnP 协议仅向帝瓦雷音箱推送音频 URL，由音箱端（或部署在软路由上的 BubbleUPnP Server）完成 `24bit/192kHz` 解码播放，手表几乎不承担解码负载从而保持超长续航。

---

## 3. NovaPlayer：面向单手操作的 SMB/WebDAV/网盘直连播放器

- **来源**: https://www.coolapk.com/feed/74293412
- **开源入口**: `https://github.com/guixun/NovaPlayer/releases`
- **特性**: 支持原生 SMB / WebDAV / AList，以及阿里云盘、夸克网盘、115 网盘免 AList 客户端直连与分块并发加速，提供竖屏上下滑切集与一键递归文件夹建收藏夹。
