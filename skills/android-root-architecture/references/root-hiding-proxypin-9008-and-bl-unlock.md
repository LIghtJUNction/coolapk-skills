# Root 隐藏方案、ColorOS 17 分身抓包、9008 救砖与 iQOO/红米镜像归档

整理自酷安社区实测与工具发布记录（未读配图与外部网盘文件，未在真机复现）。

## 1. Root 隐藏路线对比与模块组合

- **Magisk Alpha 转 KernelSU 动因**：[酷安原帖](https://www.coolapk.com/feed/74831327)（`imgs=5`）— 作者指出部分应用（如闪动校园）检测点下沉到内核层面，用户态隐藏难以持续对抗，故迁移至内核级 KernelSU。
- **4 模块过检组合（个人配置记录）**：[酷安原帖](https://www.coolapk.com/feed/74779916)（`imgs=5`）— 选用内置 Zygisk 的 KernelSU 版本（省去 Zygisk Next）+ `susfs-for-kernelsu` + `oh my key`（或 TS 模块隐藏 BL/Key）+ `Play integrity`。
- **Magisk Alpha 银行与政务 App 实测**：[酷安原帖](https://www.coolapk.com/feed/74730231)（`imgs=8`）— 按「01 Root / 02 隐藏环境 / 03 过 `Dirty Sepolicy`」分步配置后，实测众安银行、中银香港、农行、工行、交管 12123、铁路 12306、电子社保卡、数字人民币未闪退/未报 Root。
- **`妙涵一键隐藏` KSU 版（1018）**：[酷安原帖](https://www.coolapk.com/feed/74442796)（`imgs=2`）— 面向 KSU/杂鱼的整合隐藏模块发布说明。

## 2. ColorOS 17 分身抓包（ProxyPin）权限配置

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74760479)（`imgs=2`）
- **步骤**：KernelSU 关闭默认卸载模块后刷入 `MoveCertificate`（如 `v1.5.7`）与 `MagicMount`（KSU 及衍生物需要）并重启；安装 ProxyPin 用户证书后再次重启并开启 ProxyPin 分身；随后在终端以 Root 执行 `su -c appops set --user 999 com.network.proxy ACTIVATE_... allow`（`999` 替换为实际分身用户 ID，包名替换为对应软件）。

## 3. BL 解锁、Fastboot 降级与 9008 救砖记录

- **`小米 QC 解锁 BL 工具 v2.5Plus`**：[酷安原帖](https://www.coolapk.com/feed/74800795)（`imgs=9`）— 覆盖骁龙 `8Gen2` 至 `8E5`，新增 `64560`/`98260` 提权适配澎湃 OS 4 正式版（K90 至尊/K100/K90/小米 17 Max/100 Pro 等）；若工具未内置提权，用户自行获取临时 Root 后亦可调用工具后端解锁。
- **iQOO 11s（`PD2304`）Fastboot 全量包降级/救砖**：[酷安原帖](https://www.coolapk.com/feed/75003660)（`imgs=5`）— 解锁 BL 并安装 vivo 驱动后在 Fastboot 运行包内 `双击我线刷.bat`（附 OEM 修复与一键 Root 脚本，覆盖 `13.0.1` 至 `15.2.12.1` 多版本）。
- **Win11 下 9008 无响应处理与通用流程**：
  - 小米 6 Win11 点进 9008 无反应：先在工具箱功能扩展点击「USB3.0 修复」并**重启电脑**，再进 9008 并用 MiFlash 识别 `COM` 口（[酷安原帖](https://www.coolapk.com/feed/74156186)）。
  - 9008 通用步骤：三键同按进 9008 后松手，引导优先选 `v2`，XML 全选并勾选自动重启后写入分区（[酷安原帖](https://www.coolapk.com/feed/74129345)）。
- **Redmi K60 至尊版 `boot` & `init_boot` 镜像归档**：[酷安原帖](https://www.coolapk.com/feed/74811408)（`imgs=0`，中国移动云盘分享目录汇总）。
