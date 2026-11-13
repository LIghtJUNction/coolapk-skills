---
name: windows-network-printing
description: 将 Windows 上通过 USB 连接的打印机桥接到局域网，供 iPad、iPhone 和安卓设备发现并打印；包含安装、网络、防火墙及故障排查步骤。
---

# Windows AirPrint Bridge：共享 Windows USB 打印机

本技能整理了原帖的操作经验，并补充了同名开源项目的公开文档链接。项目链接是按名称和程序名补查所得，无法确认它就是原帖使用的安装包；安装器来源及内容未独立校验。原帖实测版本为 v1.4.0；README 提供 v1.4.2 安装器方案。请按实际下载版本核对发布说明。

- [项目仓库](https://github.com/salmanasmat/Windows-AirPrint-Bridge)
- [发布与安装器下载](https://github.com/salmanasmat/Windows-AirPrint-Bridge/releases)
- [README](https://github.com/salmanasmat/Windows-AirPrint-Bridge/blob/main/README.md)
- [Python 脚本与任务计划方案](https://github.com/salmanasmat/Windows-AirPrint-Bridge/blob/main/SETUP_GUIDE.md)

原帖没有提供公开分享链接或提取码。下载前应核对仓库、版本和发布说明；不要把来源不明的二次打包文件当作官方发行版。

## 工作原理与适用条件

桥接程序在连接 USB 打印机的 Windows 电脑上运行服务，经局域网提供打印能力。设备发现通常使用 mDNS（UDP 5353），打印任务使用 IPP（TCP 631）。它不会给打印机增加无线模块。

链路为：手机或平板 → 家庭 Wi-Fi／局域网 → Windows 电脑 → USB 打印机。Windows 电脑须开机、联网且桥接服务运行。打印机驱动、Windows 打印系统和移动端打印服务也必须正常。

原帖报告的实测组合是 Lenovo M7400 Pro 黑白激光打印机、Windows 11、iPad 和安卓设备；作者称两类移动设备都发现打印机并完成打印。这是单一设备组合的经验，不代表所有机型、应用和文件类型都兼容。原帖准备条件列出 Windows 10／11、USB 打印机、iPad／iPhone 或安卓设备，以及允许设备互通的局域网。

## 获取与安装

原帖称实测版本为 v1.4.0，并描述按安装向导安装；安装器可能需要管理员权限。作者机器上的目录为 `C:\Program Files\AirPrintBridge\`，包含 `AirPrintBridge.exe` 和 `airprint_bridge.log`，实际位置可能不同。

补查到的项目 README 提供 v1.4.2 安装器方案；安装器内容未在本资料中校验。README 的安装器路径与 SETUP_GUIDE.md 的 Python 脚本／任务计划路径是两种方案。SETUP_GUIDE.md 要求 Python 3.10+；不能把该脚本方案当作安装器的必需步骤，也不能据此声称两种方案都无需 Python。

安装后以本机实际安装目录为准，查看程序和日志。新版先阅读对应发布说明与安装文档，不要假定设置和旧版一致。

## 确认 Windows 本地打印与服务

1. 打开“设置 → 蓝牙和设备 → 打印机和扫描仪”，确认目标打印机能从 Windows 正常打印。若不行，先处理驱动、USB 连接或 Windows 打印队列。
2. 尽量将目标实体打印机设为 Windows 默认打印机。桥接程序可能使用默认打印机；PDF 虚拟打印机或其他软件可能改动默认项。
3. 按 Win + R，运行 `services.msc`。在服务列表中查找 `AirPrintBridge` 并确认正在运行。未运行时可尝试手动启动；启动失败则记录错误并查看 `airprint_bridge.log`。
4. 查看日志是否有类似 `mDNS service registered` 和 `IPP server listening` 的记录。这只说明程序报告了服务注册或监听步骤，不证明移动设备一定能发现打印机。

原帖日志示例出现 `IPP server listening on 0.0.0.0:678`，但文中同时称 IPP 默认使用 TCP 631。两处端口不一致；不要仅凭示例认定实际监听端口，按本机日志和版本文档确认。

若手机已发送任务而实体打印机无反应，先检查默认打印机是否仍指向目标设备，再检查服务、队列和日志。

## 检查 Windows 网络配置文件

Windows 网络配置文件会影响入站通信。原帖遇到家庭 WLAN 为 Public、而防火墙规则只允许 Private 的情况，导致服务运行但移动设备无法发现打印机。

1. 以管理员身份打开 PowerShell，运行：

```powershell
Get-NetConnectionProfile
```

查看 `NetworkCategory` 和 `InterfaceAlias`。常见接口名有 `WLAN`、`Wi-Fi`，以本机输出为准。

2. 只有确认当前网络是可信的家庭网络，且接口别名确为 `WLAN` 时，才使用以下示例：

```powershell
Set-NetConnectionProfile -InterfaceAlias "WLAN" -NetworkCategory Private
```

如果实际别名是 `Wi-Fi`，按实际名称替换 `WLAN`。

3. 再次运行 `Get-NetConnectionProfile`，确认目标网络类别。

不要将公共 Wi-Fi、酒店网络或其他不可信网络改为 Private。原帖没有提供适用于 Public 网络的具体规则命令；不可信网络应保持 Public，并谨慎限制通信。

## 检查 Windows 防火墙

先查看已有规则，不要重复创建同名规则：

```powershell
Get-NetFirewallRule -DisplayName "*AirPrint*" | Format-Table DisplayName, Enabled, Direction, Action, Profile
```

核对规则是否启用、方向是否为入站、动作是否允许，以及 `Profile` 是否匹配当前网络。只有确实缺少规则、且仅在可信家庭 Private 网络使用时，才考虑添加以下两条规则。两条命令已按排版整理为独立单行 PowerShell 命令；原帖把它们挤在一行，此处仅作排版整理。执行前仍应核对现有规则和本机网络配置。

```powershell
New-NetFirewallRule -DisplayName "AirPrint Bridge TCP 631" -Direction Inbound -Protocol TCP -LocalPort 631 -Action Allow -Profile Private
```

```powershell
New-NetFirewallRule -DisplayName "AirPrint Bridge UDP 5353" -Direction Inbound -Protocol UDP -LocalPort 5353 -Action Allow -Profile Private
```

TCP 631 用于 IPP 打印通信，UDP 5353 用于 mDNS 发现。不要关闭整个 Windows 防火墙；仅开放需要的通信并限制到合适的网络配置文件。

## 移动设备打印

### iPad 与 iPhone

1. 连接家庭 Wi-Fi，打开 PDF、网页或其他支持打印的文件。
2. 从分享菜单选择“打印”，等待搜索局域网打印机。
3. 选择发现的设备，先打印一页普通文档。名称可能带 Windows 主机名，例如 `Lenovo M7400 Pro (LAPTOP-XXXX)`。

### 安卓

1. 在设置中搜索“打印”或“打印服务”，确认默认打印服务已启用。
2. 如果没有可用的默认打印服务，可考虑安装官方 Mopria Print Service。
3. 从图片、PDF 等文件的分享或菜单中选择“打印”，再选择 Windows 共享出来的打印机。

安卓菜单会因品牌和系统版本不同而变化。原帖没有提供 Mopria 的下载地址或版本。

## 故障排查顺序

### 1. Windows 本地打印

先确认 Windows 能直接打印到目标实体设备。不能打印时，先处理驱动、USB 连接或 Windows 队列，暂不排查移动端桥接。

### 2. 服务与 IPP 监听

确认 `AirPrintBridge` 服务正在运行，查看 `airprint_bridge.log` 中的服务启动、mDNS 注册和 IPP 监听信息。服务启动不等于网络发现成功。若提示端口被占用，可查询 TCP 631：

```powershell
Get-NetTCPConnection -LocalPort 631 -State Listen | Select-Object LocalAddress, LocalPort, OwningProcess
```

用实际 PID 查看进程：

```powershell
Get-Process -Id <PID>
```

将 `<PID>` 替换为查询结果中的数字。先识别占用程序，不要直接结束不认识的系统进程。原帖未提供释放端口或更改桥接端口的步骤。

### 3. mDNS 与防火墙

设备搜不到时，检查 Windows 网络配置文件、允许入站的 TCP 631 和 UDP 5353 规则，以及规则的 `Profile` 是否与当前网络匹配。已存在规则时先核对配置，不要盲目重复添加。

### 4. 局域网与路由器隔离

确认手机和电脑所在网络允许互相通信。电脑走网线、手机走 Wi-Fi 也可以，只要两者网络可互通。同一台路由器并不保证互通：访客网络、AP／客户端隔离或 VLAN 隔离都可能阻止发现。

### 5. 移动端打印服务

确认 iPad／iPhone 使用系统打印入口；安卓确认默认打印服务已启用。不同系统的菜单名称可能不同。

### 6. 队列、日志与目标打印机

设备可见但没有输出时，确认 Windows 默认打印机仍是目标实体机、`AirPrintBridge` 服务仍在运行、Windows 打印队列没有卡住的任务，并查看 `airprint_bridge.log` 的错误记录。

### 7. 照片尺寸与版面

打印成功不代表尺寸、缩放或裁切正确。原帖作者报告 iPad 原生照片打印存在缩放问题；照片长宽比、页面尺寸、驱动缩放和桥接处理都可能相关，但资料没有确定根因。

先打印普通文档，再用废纸测试图。逐项检查纸张大小（例如 A4、A5 或照片纸）、图片拉伸、裁切、白边和实际打印尺寸。每次只改一个设置。需要精确尺寸时，可先用排版应用设置尺寸、边距和裁切，导出 PDF 后打印。

## 配置参考

以下字段来自补查的项目 README，不是原帖提供的配置。配置差异应以所用版本的 README 和本机 `config.json` 为准。

```json
{
  "printer_name": "",
  "max_render_dpi": 300,
  "reverse_page_order": false,
  "auto_rotate_to_fit": false
}
```

- `printer_name`：默认空值，选择用户的 Windows 默认实体打印机。
- `max_render_dpi`：默认 300，范围 72–1200。
- `reverse_page_order`：默认 `false`。
- `auto_rotate_to_fit`：默认 `false`。

## 运行条件与限制

电脑关机、断网或桥接服务停止时，移动打印可能中断。桥接程序不会增强 Wi-Fi 信号。资源占用取决于文件、打印内容、驱动和电脑配置；原帖未提供 CPU 或内存实测数据。重要文件或特殊纸张先做小规模测试。

## 资料范围

原帖图片内容、评论、原帖所用安装包的直链及校验值、照片缩放根因及端口冲突处理方法均未提供。补查到的同名 GitHub 项目链接可供进一步核对，但不能证明它就是原帖使用的安装包；安装器来源内容未校验。

README 配置参考来源：https://github.com/salmanasmat/Windows-AirPrint-Bridge/blob/main/README.md
Python 脚本与任务计划方案来源：https://github.com/salmanasmat/Windows-AirPrint-Bridge/blob/main/SETUP_GUIDE.md

来源：[Coolapk 原帖](https://www.coolapk.com/feed/74583366)。
