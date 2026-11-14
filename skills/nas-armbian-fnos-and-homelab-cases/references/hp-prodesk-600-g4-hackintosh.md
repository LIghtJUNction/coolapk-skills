# HP ProDesk 600 G4 Desktop Mini 安装 macOS Ventura

## 适用范围与证据边界
原帖针对 HP ProDesk 600 G4 Desktop Mini，报告使用 Intel UHD 630、`Macmini8,1` SMBIOS 的 EFI，在 macOS Ventura `13.7.8` 下完成安装与核显加速。属于单一配置的发帖者实践，未独立验证；不能外推到其他机型、主板版本或 macOS 版本。原帖称主机二手约 600 元，属于发帖时价格描述而非当前报价。图片未读取，评论未提供。

EFI 仓库地址未在可读正文中呈现；原文另给出一键脚本地址，不能据此推测或替换仓库名称。

## 硬件与显示连接
关键适配组合：HP ProDesk 600 G4 Desktop Mini、Intel UHD 630 核显、`Macmini8,1` SMBIOS、macOS Ventura `13.7.8`。原帖报告核显加速已适配。

显示输出方面，优先使用 DP 直连 DP 显示器。只有 HDMI 显示器时，建议主动式 DP 转 HDMI 转接器或主动式转接线；原帖报告普通被动式 DP 转 HDMI 线可能在安装阶段或 UHD 630 加速后出现跑码完成但黑屏/无信号。首次安装可先用 safe 安全亮屏版 EFI 确认进入安装界面，再考虑切换 igpu 核显加速版 EFI。原帖未提供 EFI 文件的公开下载 URL，不能补造。

## OpenCore Recovery U 盘
建议 U 盘至少 16 GB；原帖在线脚本演示为 32 GB U 盘（系统显示实际约 28.8 GB、盘符 F）。Recovery 方式不是把完整 macOS 镜像烧录到 U 盘，而是准备 EFI 引导文件和 `com.apple.recovery.boot` 恢复文件。

本地复制方式：把 U 盘格式化为 FAT32，将 EFI 和 `com.apple.recovery.boot` 放到 U 盘根目录。若 GitHub 源码包未含 Recovery 目录，需另行准备恢复文件。原帖未提供可读的 EFI 仓库 URL。

在线脚本方式的 PowerShell 命令如下，按原帖“删”字说明恢复 URL：

```powershell
irm https://raw.githubusercontent.com/JunWan666/hp-prodesk-600-g4-efi/main/script/install-efi-to-usb.ps1 | iex
```

插入 U 盘并确认盘符；脚本选择 EFI 来源 `1` 或回车用默认项，选择目标盘符，格式化选项输入 `2`，二次确认输入 `yes`。原帖称脚本会格式化目标 U 盘并下载 macOS Recovery 文件写入。该操作会清空目标盘数据。

## BIOS 设置
开机连续按 `F10`（需要 Fn 功能键时按 `Fn + F10`）进入 BIOS。原帖修改项：

- `Advanced > Display Language`：简体中文。
- 启动选项：取消“快速引导”和“网络（PXE）引导”。
- 安全引导配置：传统支持和安全引导均设为禁用。
- 内置设备选项：显存设为 `64MB` 或 `128MB`。
- 按 `Fn + F10` 保存退出；如出现授权确认界面，按提示输入屏幕数字。

## macOS Ventura Recovery 安装
优先使用有线网络；安装时优先 DP 直连或主动式 DP 转 HDMI。开机按 `F9`（或 `Fn + F9`）进入启动菜单，选择 U 盘；OpenCore 菜单选择 `OPENCORE (external) (dmg)`。

进入 Recovery 后打开“磁盘工具 > 显示 > 显示所有设备”，选择内置物理硬盘并抹掉，格式设 `APFS`、方案设 `GUID 分区图`。抹盘会清除数据。退出磁盘工具后选择重新安装 macOS Ventura，联网下载并安装。重启后通过 OpenCore 选择 `macOS Installer` 继续；可能经历多次重启，每次仍从 OpenCore 继续选择安装项。

## 将 EFI 安装到内置盘
完成安装进入 macOS 桌面后，保留 EFI U 盘连接，打开终端执行原帖脚本：

```sh
curl -fsSL https://raw.githubusercontent.com/JunWan666/hp-prodesk-600-g4-efi/main/script/install.sh | sh
```

选择默认当前 U 盘 EFI，按提示允许终端访问可移动卷并确认。原帖称脚本会将 EFI 复制到内置硬盘 EFI 分区；拔掉 U 盘重启后测试硬盘引导。原帖报告“关于本机”显示 Mac mini、显存 `1536MB`，作为其 UHD 630 加速已工作的现象记录，不构成其他系统的保证。

## 远程访问记录
原帖只概述在 macOS 系统设置的“通用 > 共享”开启“屏幕共享”，以 VNC 方式访问；cpolar 使用 TCP 隧道映射 macOS 屏幕共享的 `5900` 端口。原帖示例公网地址为 `1.tcp.cpolar.top:11444`，其中插入“删”的字符按原帖说明移除。该地址是原帖当时的示例，不能视为长期有效地址。原帖未给出完整 cpolar 安装、隧道创建教程链接。

## 来源
- 原帖：<https://www.coolapk.com/feed/72209617>
- U 盘制作脚本：<https://raw.githubusercontent.com/JunWan666/hp-prodesk-600-g4-efi/main/script/install-efi-to-usb.ps1>（按原帖明确规则删除插入的“删”字后恢复）
- EFI 迁移脚本：<https://raw.githubusercontent.com/JunWan666/hp-prodesk-600-g4-efi/main/script/install.sh>（按原帖明确规则删除插入的“删”字后恢复）
