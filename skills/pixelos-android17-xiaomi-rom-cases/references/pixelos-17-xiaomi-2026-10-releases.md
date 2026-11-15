# 2026 年 10 月小米 / 红米 PixelOS 17.0 发布与适配汇总

以下记录整理自 2026 年 10 月中下旬发布的 8 篇社区更新日志，均面向 PixelOS 17.0（Android 17）第三方适配包。各机型均提供卡刷包与线刷包两种形态；已安装上一版本时，多数日志说明可在“设置 > 系统 > 系统更新”中选择本地安装包直接升级（其中 `mondrian` 建议升级前备份数据并视情况清数据）。

## 机型与构建版本清单

| 机型 | 代号 | 更新日期 | 卡刷版本号 | 线刷版本号 | 首次刷入底包/前置要求 | 来源 |
|---|---|---|---|---|---|---|
| Redmi K90 Ultra | `warsaw` | 2026-10-28 | `PixelOS_warsaw-17.0-20261028-0951` | `PixelOS_warsaw-17.0-20261028-1139` | 先线刷官方 OS4（原帖点名 `warsaw-OS4.0.0.15.XHPCNXM.zip`）再线刷 PixelOS | [74685814](https://www.coolapk.com/feed/74685814) |
| Redmi K90 Pro Max | `myron` | 2026-10-28 | `PixelOS_myron-17.0-20261028-1238` | `PixelOS_myron-17.0-20261028-1519` | 首次刷入需先线刷官方 OS4，再线刷 PixelOS | [74685829](https://www.coolapk.com/feed/74685829) |
| Xiaomi 14 Ultra | `aurora` | 2026-10-30 | `PixelOS_aurora-17.0-20261030-0651` | `PixelOS_aurora-17.0-20261030-0804` | 首次安装需先线刷更新到 3.0 最新版再线刷 PixelOS | [74716098](https://www.coolapk.com/feed/74716098) |
| Xiaomi 15 | `dada` | 2026-10-28 | `PixelOS_dada-17.0-20261028-0701` | `PixelOS_dada-17.0-20261028-0900` | 首次刷入需先线刷官方 OS4（原帖点名 `dada-OS4线刷包.tar`）再线刷 PixelOS | [74685787](https://www.coolapk.com/feed/74685787) |
| Xiaomi 15 | `dada` | 2026-10-14 | `PixelOS_dada-17.0-20261014-0237` | `PixelOS_dada-17.0-20261014-0421` | 同上（`dada-OS4线刷包.tar`） | [74353628](https://www.coolapk.com/feed/74353628) |
| Redmi K60 | `mondrian` | 2026-10-17 | `PixelOS_mondrian-17.0-20261017-1814` | `PixelOS_mondrian-17.0-20261017-1957` | 已装上版可直接升级无需双清（原帖建议备份数据） | [74429735](https://www.coolapk.com/feed/74429735) |
| Redmi K50 Ultra | `diting` | 2026-10-17 | `PixelOS_diting-17.0-20261017-1643` | `PixelOS_diting-17.0-20261017-1734` | 已装上版可直接升级无需双清 | [74429716](https://www.coolapk.com/feed/74429716) |
| Xiaomi 12 Pro | `zeus` | 2026-10-17 | `PixelOS_zeus-17.0-20261017-2040` | `PixelOS_zeus-17.0-20261017-2212` | 已装上版可直接升级无需双清 | [74429697](https://www.coolapk.com/feed/74429697) |

## 分机型技术适配与修复要点

### Redmi K90 Ultra（`warsaw`）[74685814](https://www.coolapk.com/feed/74685814)
- **基础与内核**：建立纯 64 位专属设备树（适配屏幕、刷新率、指纹位置、电池与机型身份）；按本机内核配置处理 4K 页内核兼容；更新 OS4.0.4.0 底包、相机与主指纹；修正充电节点 SELinux 类型与标签、补齐原厂 `init` 服务用户声明。
- **触摸、无线与 NFC**：改用本机配套触摸组件以消除跳点/漂移；补齐 Kiwi 驱动配置与查找路径修复 Wi-Fi 开启与设置卡死；修正 NFC 服务标签恢复 HAL 启动，并补齐原厂 RF 配置恢复双向读卡。
- **原厂相机与散热风扇**：补齐原厂依赖、FCM 兼容矩阵与符号版本冲突，调整连拍缓存/写入方式、PostProc 权限链路、HEIF 权限与 HDR 算法依赖、地区水印插件与字体；移植原厂相机、扫描器（接入 BCR）以及散热风扇三种模式、温控/场景联动、通话录音停转、自清洁与快捷磁贴。

### Xiaomi 15（`dada`）[74353628](https://www.coolapk.com/feed/74353628)、[74685787](https://www.coolapk.com/feed/74685787)
- **系统与通信**：10 月 14 日基于 OS4 编译适配的版本开启了系统更新本地选择安装包（补齐 OTA 分区标签以修复本地更新错误 7），补齐国内/国际版本属性、区域与 eSIM 配置，修复 IMS 依赖与音频参数桥接以恢复拨号及双向通话声音，并修复 5G 图标短暂消失。10 月 28 日版本进一步修复特定情况下无法挂断电话。
- **指纹与交互**：修复超声波指纹服务启动、录入位置与属性权限，补齐高通/汇顶指纹加载入口与节点权限，将双击亮屏与息屏指纹统一由 Power/UDFPS 处理，修复高触控采样率并补齐人脸模型。原帖注明“汇顶指纹仍待对应硬件反馈”。
- **相机**：完成 OS4 徕卡相机 Android 17 接口与图形缓冲兼容，修复快速变焦空裁切元数据崩溃、1:1 画幅预览尺寸、连拍退出、缩略图快览跳转、SNPE DSP 人像预览卡顿、电影模式无画面、慢动作预览帧率与录屏绿边/偏绿及 QQ 视频通话偏色；10 月 28 日版本进一步修复 HEIF 拍照问题。

### Xiaomi 14 Ultra（`aurora`）[74716098](https://www.coolapk.com/feed/74716098)
- 10 月 30 日更新集中修复徕卡相机与显示适配：修复连续拍摄闪退、4K60 录像退出、HEIF 原图空白、系统色彩模式切换失效及萌拍入口缺失；修正相机调度与控制节点权限并补齐 ToF 测距传感器适配。

### Redmi K90 Pro Max（`myron`）[74685829](https://www.coolapk.com/feed/74685829)
- 10 月 28 日版本修复拍照 HEIF 问题；标配说明集成原厂相机并标注支持 Bose。

### Redmi K60（`mondrian`）、Redmi K50 Ultra（`diting`）、Xiaomi 12 Pro（`zeus`）[74429735](https://www.coolapk.com/feed/74429735)、[74429716](https://www.coolapk.com/feed/74429716)、[74429697](https://www.coolapk.com/feed/74429697)
- 10 月 17 日更新均记录：设备树、公共配置及内核同步 LineageOS 24.0 上游更新，更新无线投屏组件及 Android 17 接口适配，开启本地更新，并标明集成 MIUI 相机且内置 ReSukiSU 与 SUSFS 2.3。

## 证据边界

- 8 篇原帖均含 5–9 张图片（未读取），且完整刷机包与历史视频入口仅指向评论区或外部平台主页，本库未验证任何下载包、签名或设备实测结果。
- 8 篇来源：[74685814](https://www.coolapk.com/feed/74685814)、[74716098](https://www.coolapk.com/feed/74716098)、[74685829](https://www.coolapk.com/feed/74685829)、[74685787](https://www.coolapk.com/feed/74685787)、[74429735](https://www.coolapk.com/feed/74429735)、[74429716](https://www.coolapk.com/feed/74429716)、[74429697](https://www.coolapk.com/feed/74429697)、[74353628](https://www.coolapk.com/feed/74353628)。
