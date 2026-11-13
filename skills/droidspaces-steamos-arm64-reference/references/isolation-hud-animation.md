# SteamOS ARM64 容器参考：05–08

仓库自述为 AI 生成的社区资料。资料面向一加 Ace 3（骁龙 8 Gen 2、Adreno 740）、Android + Magisk/SukiSU，以及 DroidSpaces 内的 SteamOS ARM64。内容是仓库作者自述的社区实验记录，不是官方文档或独立验证结果；其中对其他设备和软件版本是否适用，资料没有证明。

## 05｜Android 侧手柄输入隔离

`gamepad-isolate` 是 Magisk/SukiSU 模块，通过把模块内的 XML bind mount 到 `/vendor/etc/excluded-input-devices.xml`，让 Android `InputManagerService` 排除指定设备。文档称这样可避免 Android 经 Anland 转发按键、与容器 evdev 输入重复；模块不直接改写 `/vendor` 分区。

文件布局：

```text
magisk-module/gamepad-isolate/
├── module.prop
├── post-fs-data.sh
└── excluded-input-devices.xml
```

`module.prop` 的示例字段为 `id=gamepad-isolate`、`name=Gamepad Input Isolation`、`version=1.0`。启动脚本的关键操作是：

```sh
MODDIR=${0%/*}
chcon u:object_r:vendor_configs_file:s0 "$MODDIR/excluded-input-devices.xml" 2>/dev/null
mount --bind "$MODDIR/excluded-input-devices.xml" /vendor/etc/excluded-input-devices.xml
```

XML 用 `<device name="精确设备名"/>` 列出要排除的设备；名称必须精确匹配，不支持通配符。示例包含 `Xbox Wireless Controller`、`Microsoft X-Box 360 pad`、`Sony Computer Entertainment Wireless Controller`、`GameSir-X5 Lite` 和 `Steam Virtual Gamepad`。若手柄建立了 Touchpad、Motion Sensors 等多个输入设备，应逐个列出。资料特别指出 `qti-haptics` 和 `qcom-hv-haptics` 这两个震动设备也需纳入其配置，以免相关输入泄漏。

文档称 `InputManagerService` 开机时读取一次配置；改完 XML 后可软重启 Android 框架或重启手机：

```sh
su -c 'stop; start'
```

检查排除配置可用：

```sh
su -c 'dumpsys -t 30 input' | grep ExcludedDeviceNames
```

**验证边界：**原文还建议用 `cat /proc/bus/input/devices` 检查，但声称其中“应该看不到手柄”。这与该模块作用不符：Android InputManager 排除设备不会注销内核输入设备，也不会因此让它们从 `/proc/bus/input/devices` 消失。该文件可用于读取内核注册的设备及名称；不能用“设备消失”验证 InputManager 排除是否生效。容器能否看到 evdev 节点也取决于容器设备节点映射，需另行检查。

新增手柄时，原文用以下命令查看名称：

```sh
su -c 'cat /proc/bus/input/devices' | grep 'N: Name='
```

把完整设备名加入 XML 后，复制配置、设权限和 SELinux 标签，再运行模块脚本或重启：

```sh
su -c 'cp /data/local/tmp/excluded-input-devices.xml /data/adb/modules/gamepad-isolate/excluded-input-devices.xml'
su -c 'chmod 644 /data/adb/modules/gamepad-isolate/excluded-input-devices.xml'
su -c 'chcon u:object_r:vendor_configs_file:s0 /data/adb/modules/gamepad-isolate/excluded-input-devices.xml'
su -c 'sh /data/adb/modules/gamepad-isolate/post-fs-data.sh'
```

卸载步骤是删除 `/data/adb/modules/gamepad-isolate` 并重启：

```sh
su -c 'rm -rf /data/adb/modules/gamepad-isolate'
```

这会删除模块目录，操作前应确认目标路径确实是该模块目录。

## 06｜Deck 模式与性能 HUD

资料把 Steam 性能面板和 MangoHud 区分为两层：Steam 的 TDP、GPU 频率、FPS 限制等面板属于 Deck 模式 UI；屏幕上实际绘制 HUD 的是 gamescope 的 `--mangoapp`。

文档给出的 Deck 模式启动行是：

```sh
exec steam -gamepadui -steamdeck -steamos3 -steampal -cef-enable-debugging "$@"
```

作者记录首次启动可能触发 Steam 客户端切换并更新分支；若依赖蓝牙手柄热插拔，资料建议检查分支并使用 `steamdeck_publicbeta`。Deck 快捷访问菜单可用手柄 Guide 键加 A/×/B，或键盘 `Ctrl+2` 打开。该设备上 TDP 和 GPU 频率滑块没有实际硬件控制能力；作者称可用的是 FPS 限制和 HUD。

启用 MangoHud 的 gamescope 命令示例：

```sh
exec "$GAMESCOPE_BIN" --backend anland -f -e --mangoapp -- "$@"
```

`mangoapp` 来自 `mangohud` 包，文档所列路径为 `/usr/sbin/mangoapp`。可用 `pgrep -af mangoapp` 检查进程。

作者记录启用 `--mangoapp` 的一次会话出现 3607 条如下警告，并伴随 Steam UI 卡顿及更多 `SteamUI thread frame stalled`：

```text
[gamescope] [Error] wlserver: create_swapchain: Surface already had a gamescope_swapchain! Warning!
```

这是单次社区记录，不应视作所有版本必现。该文建议日常关闭 `--mangoapp`，需要测帧时再启用，用后执行仓库脚本 `sh scripts/remove-mangoapp.sh`。若只需单个游戏的帧率或温度叠加层，可在游戏启动选项使用：

```text
MANGOHUD=1 %command%
```

文档给出的检查命令：

```sh
grep 'exec steam' /home/steamos/gamescope_steam.sh
grep 'mangoapp' /home/steamos/gamescope_steam.sh
pgrep -af mangoapp
cat /home/steamos/.local/share/Steam/package/beta
grep -c GetState /home/steamos/.local/share/Steam/logs/console_log.txt
tail -20 /home/steamos/.local/share/Steam/logs/steamui.txt
```

恢复 Deck 启动参数和移除 MangoHud 的示例：

```sh
sh scripts/revert-deck-flags.sh
sh scripts/remove-mangoapp.sh
systemctl --user restart anland-game-mode
```

脚本相对路径取决于当前工作目录和仓库检出位置；原文没有给出仓库实际安装路径。

## 07｜Steam 启动动画

启动动画是 Steam 点数商店商品，不是 Steam 创意工坊内容。购买或兑换后，还需在 Steam 的“设置 → 自定义 → 启动动画”中选中；随后客户端才会下载影片。Deck UI 和大屏幕 UI 都提供相关设置入口。原资料将“特别快递”列为 3000 Steam 点数；这是资料中的价格记录，当前商品与价格需在商店核对。

原文示例中 `config/config.vdf` 可见如下字段；`MovieID` 为 `0` 表示未选择：

```text
"StartupMovie"
{
    "MovieID" "0"
}
```

文档指出，未选中动画前可能尚无 `config/movies` 目录；已选中时 Steam 会把影片放在安装目录下，具体位置随客户端版本变化，`config.vdf` 中的 `StartupMovie` / `MovieID` 是其给出的配置线索。

点数商店页面在 Steam CEF 中显示。若一直转圈，可先区分客户端后台更新、网络/CDN 较慢和下载队列被大型游戏占用。原文记录其设备访问的 CDN 主机名为 `dl.steam.clngaa.com`、`xz.pphimalayanrt.com`，并称当时下载返回 HTTP 200；这只是作者当时观察，不能保证当前可用性或证明页面资源已完整加载。可检查：

```sh
tail -20 /home/steamos/.local/share/Steam/logs/bootstrap_log.txt
tail -20 /home/steamos/.local/share/Steam/logs/content_log.txt
grep -i 'StartupMovie' /home/steamos/.local/share/Steam/config/config.vdf
```

若 `bootstrap_log.txt` 出现 `Downloading manifest` 或“正在展开安装包”，先等客户端更新完成；若下载队列被大游戏占用，可暂停该游戏下载后重试。已购买但列表中没有时，检查是否已在设置里选中。影片无法播放时，原文建议重启 Steam；仍失败可尝试较短的 WebM，但没有提供编码参数或兼容性保证。ARM64 上的 CEF/编解码问题仅作为作者列出的可能原因。

## 08｜排障记录与检查

下列是原文整理的现象、作者记录的原因和处理方向。文档 01–04 的细节见[配套参考](input-steam-decky-systemd.md)；本表中的“未提供”仅描述 05–08 四篇的内容边界，不能替代完整文件核对。

| 现象 | 原文记录的原因或处理方向 |
|---|---|
| 容器看不到手柄 | 容器 `/dev` 使用独立 devtmpfs，节点不会自动出现；由 `input-nodes.sh` 轮询 `/sys/class/input` 并用 `mknod` 创建（依赖文档 01，脚本细节未知）。 |
| 重复按键、D-pad 变方向键 | Android 转发与 evdev 输入重复；使用 `gamepad-isolate` 和 `excluded-input-devices.xml`（见本参考 05）。 |
| 重启后输入节点丢失 | 检查 `input-nodes.service` 与僵尸节点清理（01、04，细节未知）。 |
| 触摸表现为模拟鼠标而非直触 | 原文称 gamescope `touch_click_mode` 默认 `1`；建议 `STEAM_TOUCH_CLICK_MODE=4`（passthrough）并由 `anland-touch-direct.service` 保持（06，配置细节未知）。 |
| 有线 GameSir 只有方向键 | 原文记录其 XInput 接口 `3537:1116` 无驱动、仅键盘接口绑定；建议 PS 模式 `054c:09cc` 或蓝牙。适用型号和驱动条件未详。 |
| PS4 / PS 模式手柄未识别 | 原文归因于 event 节点缺失或 Steam 扫描竞态，列出 `input-nodes.sh` 与 `udevadm trigger`（01）。 |
| Decky Loader 安装失败 | 原文称稳定版仅提供 x86_64 `PluginLoader`，建议使用 `v3.3.0-pre1` 的 `PluginLoader-arm64`；对应 ARM64 官方下载地址和安装记录见[配套参考](input-steam-decky-systemd.md)。 |
| 容器重启后 Decky 消失 | 原文称自定义 unit 若位于 `/etc/systemd/system`，首次扫描时 overlay 可能尚未挂载；建议将 unit 与 `wants` 软链接置于 `/usr/lib/systemd/system`（04，完整步骤未知）。 |
| Deck 模式后 Steam 卡在转圈 | 原文记录 `IClientSystemDisplayManager::GetState` 超时、CEF transport 错误及 MangoApp swapchain 冲突；处理方向为退回大屏幕、关闭 `--mangoapp` 并检查 Steam 分支（02、06）。 |
| 蓝牙手柄不能热插拔 | 原文归因于客户端从 `steamdeck_publicbeta` 切到 `steamdeck_stable`，建议恢复分支。未提供修改分支的具体命令。 |
| GameSir 重枚举或跳至 Switch Pro 模式 | 原文归因于 Steam HIDAPI 读取导致复位（`hid_read failure`），记录的规避办法是限制 hidraw 权限：`chmod 660`；未给出具体设备路径和所有者设置。 |
| Steam UI 可操作但游戏不识别手柄 | 原文称 `/dev/uinput` 为 `0600 root:root`，Steam 无法创建虚拟 Xbox 手柄；记录的处理为 `chmod 666 /dev/uinput`。这是容器侧权限变更，重启后是否持久及安全影响未说明，应先核实目标节点和运行环境。 |
| 热插拔延迟或不稳定 | 原文称将轮询周期由 2 秒改为 0.5 秒，并在状态变化时运行 `udevadm trigger`（01，脚本实现未知）。 |
| Anland 无画面 | 原文归因于手机锁屏或通知栏遮挡 `SecondaryActivity`，列出 `cmd statusbar collapse`、启动 `SecondaryActivity` 和延长屏幕超时；完整 `am start` 示例见[配套参考](input-steam-decky-systemd.md)。 |
| `systemctl is-system-running` 一直为 `starting` | 原文称 `qteesupplicant`、`sheng-devauth`、`xiaomi-sheng-thp`、`bluetooth` 等手机专属服务阻塞启动事务，并记录在 `/usr/lib/systemd/system` 屏蔽服务；屏蔽示例及其适用条件见[配套参考](input-steam-decky-systemd.md)。 |
| `systemctl --user` 连接失败 | 原文归因于最后一个会话结束时用户管理器退出，列出 `loginctl enable-linger steamos`。`steamos` 是资料中的容器用户名示例，使用时需核实实际账户。 |
| `steamloopback.host` transport 错误 | 原文归因于 display manager 超时拖住 UI 线程，并称退出 Deck 模式后解决、该域名解析到 `127.0.0.1`；属于作者记录，未提供独立复现。 |
| 重启后 PID 变化 | 进程重新启动后 PID 改变本身不表示故障；不能用 PID 变化反推 unit 文件被重建。原文的这层因果说明缺乏证据。 |

原文给出的宿主 Android 检查命令：

```sh
su -c 'cat /proc/bus/input/devices'
su -c 'lsusb'
su -c 'dmesg' | grep -i usb
dumpsys usb | sed -n '/host_manager/,/}/p'
dumpsys window | grep mCurrentFocus
```

下面带 DroidSpaces 前缀的命令从 Android 宿主执行，由它将检查命令送入容器，检查设备节点、节点状态、服务日志和 Steam 控制器日志：

```sh
droidspaces --name=steamos run ls -l /dev/input /dev/hidraw* /dev/uinput
droidspaces --name=steamos run cat /run/input-nodes.state
droidspaces --name=steamos run journalctl -t input-nodes -n 20
droidspaces --name=steamos run tail -30 /home/steamos/.local/share/Steam/logs/controller.txt
droidspaces --name=steamos run grep -c GetState /home/steamos/.local/share/Steam/logs/console_log.txt
droidspaces --name=steamos run systemctl --failed
```

示例中的 `/dev/hidraw*` 可能先被宿主 shell 展开，不能据此认定已枚举容器全部 hidraw 节点；应按容器实际节点核对。

`steamos` 和 `/home/steamos` 是原文路径及账户示例；执行前须确认当前容器配置。原文所谓最终状态包括 `systemctl is-system-running` 为 `running`、`systemctl --failed` 无失败项、`plugin_loader.service` active 且日志出现 `Loading Decky frontend!`、`input-nodes.service` active，以及 Steam 通过 evdev 打开手柄并创建 `Microsoft X-Box 360 pad N`。其中“event 节点与 `/dev/uinput` 都是 `0666`”是原文记录的目标状态，不构成所有设备都应采用该权限的通用建议。文档还将 Steam 分支 `steamdeck_publicbeta` 列为目标状态。

## 来源

- [05-magisk-module.zh-CN.md](https://raw.githubusercontent.com/chickenisbeautiful/droidspaces-arm64-solutions/main/docs/05-magisk-module.zh-CN.md)
- [06-performance-hud-and-deck-mode.zh-CN.md](https://raw.githubusercontent.com/chickenisbeautiful/droidspaces-arm64-solutions/main/docs/06-performance-hud-and-deck-mode.zh-CN.md)
- [07-steam-startup-animation.zh-CN.md](https://raw.githubusercontent.com/chickenisbeautiful/droidspaces-arm64-solutions/main/docs/07-steam-startup-animation.zh-CN.md)
- [08-troubleshooting-log.zh-CN.md](https://raw.githubusercontent.com/chickenisbeautiful/droidspaces-arm64-solutions/main/docs/08-troubleshooting-log.zh-CN.md)


## InputManager 排除与内核设备的区别

[AOSP EventHub.cpp](https://android.googlesource.com/platform/frameworks/native/+/refs/heads/main/services/inputflinger/reader/EventHub.cpp) 在打开设备后按完整名称比较排除列表，命中时关闭该读取句柄并返回；该流程没有注销内核输入设备。因而不能把 `/proc/bus/input/devices` 中设备消失当成排除成功的必要条件。这里是对源码流程的推断，不是该模块的设备验收。
