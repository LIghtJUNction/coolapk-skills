# DroidSpaces 中 SteamOS ARM64 容器参考

> 社区实验记录，面向 OnePlus Ace 3（骁龙 8 Gen 2、Adreno 740）、Android + Magisk/SukiSU 与 DroidSpaces SteamOS ARM64 容器。仓库自述为 AI 生成；以下内容是社区资料，不代表官方文档或独立复测，也不能据此泛化到任意 ARM64 设备。命令按执行环境区分：`su -c`、`adb shell` 属于 Android 宿主；带 `droidspaces ... run` 前缀的命令也在宿主执行，由 DroidSpaces 将后半部分送入容器。纯 `systemctl`、Steam 路径及 `/dev/input` 操作则在容器执行，除非另有说明。文中 `/home/steamos`、用户 `steamos`、UID 1000 和设备节点号是资料中的环境值或示例，使用前应核对目标环境，不能视为通用实值。

## 1. 手柄识别：分清 Android、容器节点和 Steam 三层

手柄输入依次涉及三个互不等同的环节：

1. **Android InputManager 排除表（宿主）**：`/vendor/etc/excluded-input-devices.xml`；资料实际使用的模块文件为 `/data/adb/modules/gamepad-isolate/excluded-input-devices.xml`。这里按设备完整名称精确、区分大小写匹配，`*Xbox*` 这类通配符无效。它让 Android InputManager 忽略对应输入设备，目的是避免 Anland 同时转发按键，导致容器收到重复输入；不会注销内核设备或移除 evdev 节点；容器仍可按权限读取，Steam 的表现还受后续映射影响。资料称 InputManager 开机时读取一次，修改后需软重启或重启手机，示例软重启命令：`su -c 'stop; start'`。
2. **容器设备节点管理**：容器有独立的 `/dev`（资料称其 `devtmpfs` 的 `stat -c %d` 与宿主不同）；`systemd-udevd` 位于独立 netns，热插拔 uevent 不会送到容器 udev。仓库的 `input-nodes.sh` 每 0.5 秒轮询共享的 `/sys/class/input`，按设备名关键词或 udev 数据 `ID_INPUT_JOYSTICK=1` 创建 `/dev/input/eventN` 并设为 `0666`，移除 sysfs 中已消失设备对应的僵尸节点，并处理 hidraw、`/dev/uinput` 和状态变化。
3. **Steam/SDL/Steam Input**：Steam 需要访问 evdev 或 hidraw；Steam Input 还可能通过 `uinput` 创建虚拟 Xbox 手柄，供 Proton/Wine 游戏使用。

### Android 侧确认设备名

在手机宿主上读取内核实际报告的名字，再将完整字符串原样加入排除表：

```bash
su -c 'cat /proc/bus/input/devices' | grep 'N: Name='
```

资料举例包括 `Xbox Wireless Controller`、`GameSir-X5 Lite`、`Sony Computer Entertainment Wireless Controller`、`Steam Virtual Gamepad`、`Microsoft X-Box 360 pad` 及带编号的变体。DS4/PS 模式可能还会出现传感器设备。具体名称以设备输出为准；品牌词加到 XML 中不能代替精确名称。

### 容器节点、权限和热插拔

仓库的 `input-nodes.sh` 关键词匹配使用 shell `case` 通配，区分大小写且匹配子串。示例关键词覆盖 Xbox、X-Box、Microsoft、Controller、Gamepad、Joystick、`2.4G`、Wireless Controller、DualSense、DualShock、Sony、Nintendo、Switch Pro、Pro Controller，以及 GameSir、Flydigi、VADER、APEX、Direwolf、BETOP、BTP-、Beitong、KunPeng、Pangu、Asura、Sparta、ShanWan、8BitDo、BIGBIG、Machenike、IINE、Ajazz、Dareu、Rapoo、Thunderobot、RedMagic、Black Shark、Razer、Wolverine、Kishi、Logitech、SteelSeries、Stratus、Nimbus、PowerA、PDP、HORI、Nacon、SCUF、Turtle Beach、Thrustmaster。关键词来自内核设备名，不是中文商品名。

脚本把 `/dev/uinput` 设为 `0666`。资料报告的典型故障是 Steam 界面可用、游戏却看不到手柄：若 `/dev/uinput` 是 `0600 root:root`，普通用户运行的 Steam 无法创建虚拟手柄。该权限变更会放宽容器内 uinput 设备的访问权限，需结合目标环境评估，不要把资料中的脚本当成无条件安全配置。

`/run/input-nodes.state` 记录物理手柄 event 号集合；物理设备集合变化时，脚本对手柄节点执行 `udevadm trigger --action=add`，促使 Steam 重扫。资料称状态触发可避免 Steam 长期持有类似 `event10 (deleted)` 的旧 fd。其观察到蓝牙重连约 1–2 秒、节点轮询最多 0.5 秒、Steam 重扫约 1–2 秒，总计约 2–4 秒；这些是该环境的记录，不是保证值。更保守的恢复办法是先连手柄再启动 Steam，或重启会话。

### GameSir hidraw 与设备模式问题

资料报告 GameSir-X5 Lite 经 Steam HIDAPI 读取后可能重新枚举或在 GameSir `3537:1116` 与 Switch Pro `057e:2009` 模式间跳变，并出现 `Controller device closed after hid_read failure`。仓库方案是对匹配的 GameSir hidraw 节点限制 Steam 用户访问，让 SDL 回退到 evdev：

```bash
chown root:input /dev/hidrawN
chmod 660 /dev/hidrawN
```

`/dev/hidrawN` 是占位设备路径，必须先识别目标节点；不要原样执行。资料称其它品牌 hidraw 保持 `0666`，只有 GameSir 强制走 evdev。另有针对有线 GameSir 的社区解释：手机原厂内核缺少 `USB_QUIRK_WINDOWS_CONFIG_REQ_SIZE` 类 quirk，`xpad` 不支持 `3537:1116`，可能反复复位；PS/DS4 模式 `054c:09cc` 被报告可用，Xbox 模式可能不可用。资料还把某些有线 2.4G 接收器仅有方向键归因于 XInput 接口无驱动、只绑定键盘接口，并建议尝试 PS 模式或蓝牙。此类硬件结论需要按具体手柄、内核和连接方式核对。

### 手柄排障线索

| 现象 | 资料中的原因或检查方向 | 资料中的处理 |
|---|---|---|
| Steam 界面可操作，游戏无手柄 | 容器 `/dev/uinput` 权限不足，Steam Input 无法创建虚拟手柄 | 检查后设为 `0666`；仓库脚本会强制设置 |
| 启动时正常，热插拔后失效 | 容器 udev 收不到热插拔事件，Steam 仍持有旧 fd | 轮询和状态触发；可先连手柄再启动 Steam，或重启会话 |
| GameSir 断连或跳到 Switch Pro 模式 | HIDAPI 读取触发重新枚举 | 限制目标 GameSir hidraw 权限，使 Steam 走 evdev；核对节点后再改权限 |
| 容器中按键重复 | Android 经 Anland 转发，与 evdev 输入叠加 | 排除表添加精确设备名；按资料流程软重启宿主 |
| 宿主 `/proc/bus/input/devices` 有设备，容器无节点 | 容器 `/dev` 独立 | 核对并运行容器的 `input-nodes.sh` 服务 |
| Steam 日志有 `hid_read failure` | 设备可能在 Steam 读取时重新枚举；资料也提示检查线材和供电 | 按 GameSir hidraw 路径排查；资料提到 OTG 500mA 上限，但没有给出适用于所有设备的测量或验证方法 |

加新手柄时，先在宿主读取名称；若容器脚本关键词未命中，再修改仓库 `scripts/input-nodes.sh` 并在容器运行 `systemctl restart input-nodes`；如需避免 Anland 重复转发，将完整名称加入模块排除表并软重启宿主。容器侧可检查 `ls -l /dev/input`、`cat /run/input-nodes.state` 和 `tail -f /home/steamos/.local/share/Steam/logs/controller.txt`。路径和用户需按目标容器核对。

## 2. Steam 分支、Deck 模式和恢复

资料记录的设备环境为 SteamOS ARM64 容器中的 ARM64 Steam（`steamrtarm64/steam`）和 Proton 11.0 ARM64。它记录了以下分支观察：

| 分支 | 资料中的 manifest 名 | 记录的构建号 | 蓝牙热插拔记录 |
|---|---|---:|---|
| `steamdeck_publicbeta` | `steam_client_steamdeck_publicbeta_linuxarm64` | `1792192922`，后期 `1792711043` | 正常 |
| `steamdeck_stable` | `steam_client_steamdeck_stable_linuxarm64` | `1792618285` | 回归，重连可能不触发重扫 |
| 普通 public | `steam_client_linuxarm64` | 未提供 | 资料称该设备未使用 |

分支文本文件路径为 `~/.local/share/Steam/package/beta`。资料称删掉它会回退到该镜像默认分支（当时为 `steamdeck_stable`）；写入 `steamdeck_publicbeta` 可选择该分支。分支切换曾触发约 500 MB 下载、自更新和重启。分支行为与构建号都可能随客户端更新改变，不能把该记录当作当前版本保证。检查示例：

```bash
cat /home/steamos/.local/share/Steam/package/beta
grep -n steam_client_ /home/steamos/.local/share/Steam/logs/bootstrap_log.txt
grep 'Client version' /home/steamos/.local/share/Steam/logs/steamui.txt
```

### Steam 与 Gamescope 参数

资料中的大屏幕配置：

```bash
exec steam -gamepadui -cef-enable-debugging "$@"
```

Deck 模式实验配置：

```bash
exec steam -gamepadui -steamdeck -steamos3 -steampal -cef-enable-debugging "$@"
```

参数含义按资料记录：`-gamepadui` 启用大屏幕/手柄 UI；`-cef-enable-debugging` 开启 CEF 远程调试端口 `8080`，Decky Loader 依赖它；`-steamdeck`、`-steamos3`、`-steampal` 用于 Deck UI/SteamOS 集成。开启 CEF 调试会暴露调试端口；资料没有说明该端口的监听地址、访问控制或网络隔离情况。使用前应核对实际监听范围与容器网络，不应默认端口只对本机可见。

Gamescope 的资料示例：

```bash
exec "$GAMESCOPE_BIN" --backend anland -f -e --mangoapp -- "$@"
```

日常无 HUD 示例为 `exec "$GAMESCOPE_BIN" --backend anland -f -e -- "$@"`。`--mangoapp` 用于 MangoHud/Steam 性能 HUD；资料报告启用后出现大量 `create_swapchain: Surface already had a gamescope_swapchain!` 警告，UI 响应变差，移除该参数可隐藏 HUD 并改善稳定性。`--default-touch-mode` 的记录取值是 `0` hover、`1` 左键、`2` 右键、`3` 中键、`4` passthrough（直触）；原示例未展示该参数如何拼入完整启动行。

### Deck UI 相关故障和回退

资料记录 Deck/大屏 UI 在该 ARM64 环境首帧可能需 1–2 分钟，并出现 SteamUI frame stalled（示例 `6220 ms`、`5205 ms`、`58097 ms`）。它将大量 `IClientSystemDisplayManager::GetState took too long: 50 msec` 超时归因于 Deck UI 查询 display manager，而容器以 Anland 会话 broker 和定制 Gamescope 取代原 SteamOS 服务。这是仓库作者的解释，并非独立确认。

CEF 日志曾出现 `steamloopback.host` 的 `Transport Error: no transport is available for request`。资料称该私有环回域名解析至 `127.0.0.1`，把传输错误解释为 UI 线程阻塞症状；只有确认真实解析故障时才建议在容器 `/etc/hosts` 添加 `127.0.0.1 steamloopback.host`。不要仅凭这条错误就改 hosts。

如果 Steam 或 Deck UI 卡住，资料给出的命令通过宿主 DroidSpaces 执行，作用于容器：

```bash
droidspaces --name=steamos run pkill -9 -f steamrtarm64/steam
droidspaces --name=steamos run pkill -9 -f steamwebhelper
droidspaces -n steamos -u steamos run env XDG_RUNTIME_DIR=/run/user/1000 \
  systemctl --user restart anland-game-mode.service
```

这些命令会强制结束 Steam 及 webhelper 并重启用户会话；执行前确认容器名、运行中的任务和 UID/runtime 路径。资料称客户端更新时 bootstrap 会校验和修复文件，但没有提供更新中强制终止的风险边界。检查分支、构建和日志可用：

```bash
droidspaces --name=steamos run cat /home/steamos/.local/share/Steam/package/beta
droidspaces --name=steamos run grep 'Client version' /home/steamos/.local/share/Steam/logs/steamui.txt | tail -1
droidspaces --name=steamos run tail -20 /home/steamos/.local/share/Steam/logs/steamui.txt
droidspaces --name=steamos run grep -c GetState /home/steamos/.local/share/Steam/logs/console_log.txt
```

资料建议日常先用 `-gamepadui`，仅在确实需要性能面板时试 Deck 参数，并接受启动等待和 UI 卡顿；Steam 更新后再检查手柄热插拔。它建议当时使用 `steamdeck_publicbeta`，但这是特定构建上的社区观察，不应未经核实地照搬到其他版本。

## 3. ARM64 Decky Loader

资料称官方安装脚本获取的稳定版 `PluginLoader` 只有 x86_64 资产，无法在 aarch64 容器运行；另记录预发布 `v3.3.0-pre1` 提供 `PluginLoader-arm64`，并称其为 aarch64 ELF（`e_machine = 0x00B7`）。这是特定版本和社区资料的记录；本次另查官方发布记录，已核对下面的实际 ARM64 资产；没有下载或执行二进制。

先从下方核对过的链接取得 `PluginLoader-arm64` 和 `plugin_loader.service`，置于容器当前工作目录。原资料的安装步骤如下，示例用户和目录须与目标容器一致：

```bash
mkdir -p /home/steamos/homebrew/services
cp PluginLoader-arm64 /home/steamos/homebrew/services/PluginLoader
chmod 755 /home/steamos/homebrew/services/PluginLoader
cp plugin_loader.service /usr/lib/systemd/system/plugin_loader.service
ln -sf /usr/lib/systemd/system/plugin_loader.service \
       /usr/lib/systemd/system/multi-user.target.wants/plugin_loader.service
systemctl daemon-reload
systemctl enable --now plugin_loader.service
```

前提是 Steam 启动参数含 `-cef-enable-debugging`，Steam 正在运行；资料称 CEF 界面起来后日志才会出现 `Loading Decky frontend!`。检查示例：

```bash
systemctl is-active plugin_loader.service
journalctl -u plugin_loader.service -n 20
```

预期日志片段包括 `Starting Decky version v3.3.0-pre1`、`plugin_path: /home/steamos/homebrew/plugins` 和 `Loading Decky frontend!`。

卸载示例：

```bash
systemctl disable --now plugin_loader.service
rm -f /usr/lib/systemd/system/plugin_loader.service \
  /usr/lib/systemd/system/multi-user.target.wants/plugin_loader.service
rm -rf /home/steamos/homebrew
```

最后一条会递归删除整个 homebrew 目录，包括其中插件和其他文件；执行前应核对目录内容并备份需要保留的数据。不要把示例路径当成宿主 Android 路径；这些命令针对容器。

资料还称商店中的 Decky LSFG-VK 依赖 x86_64 `lsfg-vk` Vulkan 层，在 ARM64 不适用，并列出社区替代项目 `Janleyx/decky-lsfg-vk-for-Ayn-Odin-2`、`razoomnik/lsfg-vk2-arm64-odin3`、`BakaPute/ArmadaLSFG`、`drewano/armada-lsfg-adaptive`。原文只给出这些精确仓库名称；可按下方项目主页继续核对版本和资产，当前未读这些仓库正文。Odin 2 Pro 的记录也不能替代 Ace 3 上的验证。

## 4. 容器 systemd、手机服务和会话

### Unit 文件可见性

资料观察到容器 `/etc` 为 overlay 挂载，systemd 首次扫描 unit 时 overlay 尚未可见；放在 `/etc/systemd/system/` 的自定义 unit 可能显示 `enabled`，但 `inactive (dead)`，`systemctl show ... -p WantedBy` 为空，直到 `daemon-reload` 或下次启动。仓库建议将自定义 unit 和 `*.wants` 链接放入容器 `/usr/lib/systemd/system`。这是该容器镜像的行为记录，其他镜像需自行检查。

### 被屏蔽的手机专属服务

资料列出容器中可能无法工作的服务：`qteesupplicant.service`（等待不存在的 `/dev/tee0`）、`sheng-devauth.service`（小米 DevAuth）、`xiaomi-sheng-thp.service`（资料观察到重启风暴）和 `bluetooth.service`（容器内 bluetoothd 无法取得 mgmt 接口）。作者称这些服务会令 `systemctl is-system-running` 停在 `starting`，并有启动任务挂起。

仓库示例在容器 `/usr/lib/systemd/system` 备份原 unit 后以 `/dev/null` 屏蔽并 reload：

```bash
cd /usr/lib/systemd/system
for u in qteesupplicant sheng-devauth xiaomi-sheng-thp bluetooth; do
  [ -f "$u.service" ] && cp -a "$u.service" "$u.service.droidspaces-backup"
  ln -sf /dev/null "$u.service"
done
systemctl daemon-reload
systemctl reset-failed
```

这会改变容器启动服务；尤其 `bluetooth.service` 是否应屏蔽取决于目标容器是否需要蓝牙。执行前检查当前 unit、依赖和运行任务；不要在 Android 宿主误运行。资料称处理后 `systemctl is-system-running` 为 `running`、`systemctl --failed` 为 0，但未给出普遍适用性保证。仓库回退文件名为 `*.service.droidspaces-backup`；原文没有说明如何自动恢复。按 systemd mask 的语义，回退需移除对应 `/usr/lib/systemd/system/<unit>.service` 的 `/dev/null` 链接，用保存的备份恢复原 unit，再 `systemctl daemon-reload`；恢复前确认备份存在且目标链接确为该 mask，避免覆盖变更。

### 用户管理器和 linger

资料提供的容器命令：

```bash
loginctl enable-linger steamos
systemctl start user@1000.service
```

目的是在没有登录会话时保持用户管理器，使 `systemctl --user`、`anland-game-mode`、`anland-touch-direct`、`steamos-manager` 可用。用户 `steamos` 和 UID `1000` 是资料环境值，执行前应核对实际用户与 UID。用户总线错误示例为 `Failed to connect to user scope bus`。

资料列出的 unit 包括：`plugin_loader.service`、`input-nodes.service`（系统 unit，`/usr/lib/systemd/system`）；`anland-touch-direct.service`、`anland-game-mode.service`、`steamos-manager.service`（用户 unit，资料称后两者管理 Gamescope/Steam 会话及 Anland broker）。另读到的 `plugin_loader.service` 设置 `User=root`，`ExecStart=/home/steamos/homebrew/services/PluginLoader`，工作目录为 `/home/steamos/homebrew/services`；`UNPRIVILEGED_PATH`、`PRIVILEGED_PATH` 与 `HOME` 均指向该环境的 steamos 目录。`input-nodes.service` 执行 `/usr/local/bin/input-nodes.sh`，设置 `Restart=always`、`RestartSec=2`。这些是实际文件配置，尚未运行验收。

### Android 宿主前台活动

资料称 Anland 显示活动 `com.anland.consumer/.SecondaryActivity` 必须在前台；锁屏或通知栏遮挡可能销毁 surface，令 Gamescope 无法启动 Steam。宿主检查与恢复示例：

```bash
dumpsys window | grep mCurrentFocus
adb shell cmd statusbar collapse
adb shell am start -n com.anland.consumer/.SecondaryActivity --activity-reorder-to-front
```

宿主屏幕设置示例：

```bash
su -c 'settings put system screen_off_timeout 1800000'
su -c 'settings put global stay_on_while_plugged_in 7'
```

这些会改变 Android 宿主的状态栏、前台活动和屏幕超时；按需执行，并确认设备上包名/活动名有效。资料中的 `1800000` 是 30 分钟。

## 5. 公开源文件与资料边界

以下是已读取的四篇中文文档 URL，作为对应主题的来源链接：

1. 手柄识别与映射：<https://raw.githubusercontent.com/chickenisbeautiful/droidspaces-arm64-solutions/main/docs/01-gamepad-mapping.zh-CN.md>
2. Steam 版本与缺陷：<https://raw.githubusercontent.com/chickenisbeautiful/droidspaces-arm64-solutions/main/docs/02-steam-versions-and-defects.zh-CN.md>
3. ARM64 Decky Loader：<https://raw.githubusercontent.com/chickenisbeautiful/droidspaces-arm64-solutions/main/docs/03-decky-loader-arm64.zh-CN.md>
4. 容器 systemd 与会话：<https://raw.githubusercontent.com/chickenisbeautiful/droidspaces-arm64-solutions/main/docs/04-container-systemd.zh-CN.md>

仓库入口：<https://github.com/chickenisbeautiful/droidspaces-arm64-solutions>。原文相对文件链接在下方展开。除另行注明的两个 service 文件外，脚本正文尚未读取；资料未证明调试端口的实际监听范围，也没有独立设备复现结果。

## 已核对的公开资源

- [Decky v3.3.0-pre1 发布页](https://github.com/SteamDeckHomebrew/decky-loader/releases/tag/v3.3.0-pre1)
- [PluginLoader-arm64 下载](https://github.com/SteamDeckHomebrew/decky-loader/releases/download/v3.3.0-pre1/PluginLoader-arm64)，GitHub 元数据记录 34,063,176 字节。
- SHA-256：`a721b3d2c49e8a1693b53949a574af64f4d456e5306ec68638fe6a5e1238edf1`。来自官方资产元数据，未下载后独立重算。
- [Decky service](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/services/plugin_loader.service) 与 [input-nodes service](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/services/input-nodes.service)：正文已读。
- [input-nodes.sh](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/scripts/input-nodes.sh)：原文相对链接展开，正文未审。
- [mask-phone-units.sh](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/scripts/mask-phone-units.sh)：原文相对链接展开，正文未审。
- [fix-steam-cef.sh](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/scripts/fix-steam-cef.sh)：原文相对链接展开，正文未审。
- [patch-performance.sh](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/scripts/patch-performance.sh)：原文相对链接展开，正文未审。
- [enable-deck-flags.sh](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/scripts/enable-deck-flags.sh)：原文相对链接展开，正文未审。
- [revert-deck-flags.sh](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/scripts/revert-deck-flags.sh)：原文相对链接展开，正文未审。
- [remove-mangoapp.sh](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/scripts/remove-mangoapp.sh)：原文相对链接展开，正文未审。
- [test-keywords.sh](https://github.com/chickenisbeautiful/droidspaces-arm64-solutions/blob/main/scripts/test-keywords.sh)：原文相对链接展开，正文未审。

## 原文列出的其他 ARM64 项目

以下主页由原文精确的 owner/repo 名称按 GitHub 标准路径展开，尚未读取或验证其资产，也不代表与 Ace 3 兼容：

- [Janleyx/decky-lsfg-vk-for-Ayn-Odin-2](https://github.com/Janleyx/decky-lsfg-vk-for-Ayn-Odin-2)
- [razoomnik/lsfg-vk2-arm64-odin3](https://github.com/razoomnik/lsfg-vk2-arm64-odin3)
- [BakaPute/ArmadaLSFG](https://github.com/BakaPute/ArmadaLSFG)
- [drewano/armada-lsfg-adaptive](https://github.com/drewano/armada-lsfg-adaptive)
