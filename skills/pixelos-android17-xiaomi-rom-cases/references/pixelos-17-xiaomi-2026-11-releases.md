# 2026 年 11 月小米 / 红米 PixelOS 17.0 发布与适配汇总

以下记录整理自 2026 年 11 月 2 日至 11 月 13 日发布的 17 篇社区更新日志（PixelOS 17.0 / Android 17），新增小米 17 Pro（`pandora`）、小米 15 Pro（`haotian`）、小米 14 / 14 Pro（`houji` / `shennong`）、红米 K70 Pro（`manet`）与红米 K80（`zorn`）等机型的构建记录。

## 机型与构建版本清单

| 机型 | 代号 | 更新日期 | 卡刷版本号 | 线刷版本号 | 来源 |
|---|---|---|---|---|---|
| Xiaomi 17 Pro | `pandora` | 2026-11-02 | `PixelOS_pandora-17.0-20261102-0346` | `PixelOS_pandora-17.0-20261102-0424` | [74789618](https://www.coolapk.com/feed/74789618) |
| Xiaomi 17 Pro | `pandora` | 2026-11-13 | `PixelOS_pandora-17.0-20261113-2116` | `PixelOS_pandora-17.0-20261113-2209` | [75049434](https://www.coolapk.com/feed/75049434) |
| Xiaomi 15 Pro | `haotian` | 2026-11-07 | `PixelOS_haotian-17.0-20261107-1913` | `Fastboot_PixelOS_haotian-17.0-20261107-1955` | [74915848](https://www.coolapk.com/feed/74915848) |
| Xiaomi 15 Pro | `haotian` | 2026-11-13 | `PixelOS_haotian-17.0-20261113-0140` | `PixelOS_haotian-17.0-20261113-0236` | [75032869](https://www.coolapk.com/feed/75032869) |
| Xiaomi 15 | `dada` | 2026-11-05 | `PixelOS_dada-17.0-20261105-0932` | `PixelOS_dada-17.0-20261105-1307` | [74875592](https://www.coolapk.com/feed/74875592) |
| Xiaomi 15 | `dada` | 2026-11-12 | `PixelOS_dada-17.0-20261112-2139` | `PixelOS_dada-17.0-20261112-2221` | [75032865](https://www.coolapk.com/feed/75032865) |
| Xiaomi 14 Ultra | `aurora` | 2026-11-11 | `PixelOS_aurora-17.0-20261111-1506` | `PixelOS_aurora-17.0-20261111-1600` | [75006029](https://www.coolapk.com/feed/75006029) |
| Xiaomi 14 Pro / Ti | `shennong` | 2026-11-11 | `PixelOS_shennong-17.0-20261111-1302` | `PixelOS_shennong-17.0-20261111-1327` | [75006010](https://www.coolapk.com/feed/75006010) |
| Xiaomi 14 | `houji` | 2026-11-11 | `PixelOS_houji-17.0-20261111-1349` | `PixelOS_houji-17.0-20261111-1440` | [75005961](https://www.coolapk.com/feed/75005961) |
| Redmi K90 Ultra | `warsaw` | 2026-11-05 | `PixelOS_warsaw-17.0-20261105-1411` | `PixelOS_warsaw-17.0-20261105-1618` | [74875514](https://www.coolapk.com/feed/74875514) |
| Redmi K90 Ultra | `warsaw` | 2026-11-12 | `PixelOS_warsaw-17.0-20261112-1601` | `PixelOS_warsaw-17.0-20261112-1703` | [75032872](https://www.coolapk.com/feed/75032872) |
| Redmi K90 Pro Max | `myron` | 2026-11-05 | `PixelOS_myron-17.0-20261105-1918` | `PixelOS_myron-17.0-20261105-2314` | [74876341](https://www.coolapk.com/feed/74876341) |
| Redmi K90 Pro Max | `myron` | 2026-11-13 | `PixelOS_myron-17.0-20261113-1730` | `PixelOS_myron-17.0-20261113-1829` | [75049446](https://www.coolapk.com/feed/75049446) |
| Redmi K80 | `zorn` | 2026-11-05 | `PixelOS_zorn-17.0-20261105-0809` | `PixelOS_zorn-17.0-20261105-0907` | [74875498](https://www.coolapk.com/feed/74875498) |
| Redmi K80 | `zorn` | 2026-11-12 | `PixelOS_zorn-17.0-20261112-0000` | `PixelOS_zorn-17.0-20261112-0208` | [75006083](https://www.coolapk.com/feed/75006083) |
| Redmi K70 Pro | `manet` | 2026-11-05 | `PixelOS_manet-17.0-20261105-0636` | `PixelOS_manet-17.0-20261105-0741` | [74875467](https://www.coolapk.com/feed/74875467) |
| Redmi K70 Pro | `manet` | 2026-11-11 | `PixelOS_manet-17.0-20261111-1624` | `PixelOS_manet-17.0-20261111-1730` | [75006054](https://www.coolapk.com/feed/75006054) |

## 共性修复与关键机型差异

- **11 月 5 日批次共性修复（`manet`、`zorn`、`warsaw`、`dada`、`myron`）**：
  - 修正联通默认上网 APN（保留彩信代理），补齐联通/电信 IMS 与广电 APN，修正同 Build ID 升级时 APN 不更新及运营商配置打包时 MNC 前导零丢失问题。
  - `myron`（11-05）另修复充电问题、整理 eSIM 配置与权限引用并补齐空间音频依赖。
- **11 月 11–13 日批次共性修复（`aurora`、`manet`、`shennong`、`houji`、`zorn`、`dada`、`haotian`、`warsaw`、`myron`、`pandora`）**：
  - 同步上游联通/广电 APN 与 4G/5G 信号显示阈值；多数机型撤除旧 5G RIL 兼容补丁并恢复原厂无线电库，修复 LHDC v5 蓝牙播放与采样率切换异常；统一使用本机原厂相机自适应桌面图标。
  - **小米 17 Pro（`pandora`）**：11-02 首发支持背屏与背屏相机及原厂徕卡相机（首次刷入需先线刷官方 OS4）；11-13 更新优化点按与键盘振动杂响、修正录像正负片滤镜读取权限、采用 4.0.5 原厂蓝镜头相机图标并补齐 5G SA 通话配置。
  - **小米 15 Pro（`haotian`）**：11-07 修复 MNC 前导零、补齐 VoNR SA 声明与电池服务专用 SELinux 配置（原帖注明在移动 `46002` 单机实测 NR SA 注册、IMS 语音与普通通话，QTI NSA+SA 跨重启保留）；11-13 修复 LiveDisplay 黑白阅读模式与颜色校准、修复显示服务写入背光节点权限异常。
  - **红米 K90 Ultra（`warsaw`）**：11-12 补齐原厂充电温控配置（原帖注明卡刷包此前遗漏该配置），并修复显示服务写入背光节点权限异常与自动夜景偶发黑图；首次刷入仍需先线刷 `warsaw-OS4.0.0.15.XHPCNXM.zip`。
  - **红米 K90 Pro Max（`myron`）**：11-13 独立维护本机原厂色彩库，公共提取基线及原厂底包同步至 `4.0.6`，接入 WWAN 兼容服务。
  - **小米 14 Pro / Ti（`shennong`）**：11-11 补齐 Xiaomi 14 Pro Ti 钛金属版设备信息并接入 WWAN 网络注册兼容服务。

## 证据边界

- 原帖均含 6–9 张图片（未读取），完整刷机包均引导至评论区或外部视频主页获取，未在本库验证下载链接、签名或真机刷写结果。
