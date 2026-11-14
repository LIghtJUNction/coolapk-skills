# Scene 调度、触控与性能测量历史案例

以下均为社区历史说明或单次实验，未独立复现；软件版本、内核、机型和温控设置会改变结果。

## 调度与版本

- [46753166](https://www.coolapk.com/feed/46753166) 解释 Scene 6 辅助调速器：在原有 walt/schedutil 基础上随负载调整频率边界；以天玑 8100 为例，1800MHz 上限、90% 负载和 300MHz 余量得到 1920MHz，再映射到频率表中的 2100MHz。余量限制到 30MHz 时算得 1650MHz，文中称映射 1700MHz。FAS 或 FEAS 生效时该辅助调速器不工作；具体行为取决于版本。作者称热门游戏可有独立线程放置预设，核心分配是否开启不改变这些预设；检测到 AsoulOpt 工作会避让。配置与格式见 [预设仓库](https://github.com/helloklf/scheduler/tree/scene6-presets)、[格式索引](https://github.com/helloklf/scheduler/blob/scene6-presets/docs/0index.md)。
- [47014406](https://www.coolapk.com/feed/47014406) 记载 Scene 历史配置将骁龙 8 Gen 2 游戏线程主/次核心由 CPU7/CPU6 改为 CPU7/CPU4，区分 6.2HP 和 6.2LP 配置。作者报告 A715 极限性能约高 5%、核心功耗约高 10%，测试条件不足以推广为所有负载。
- [48267216](https://www.coolapk.com/feed/48267216) 为 Scene 7 Core 发布前计划，提到 FAS 适配骁龙 855、天玑 1100/1200，144Hz 及以上帧率统计与新调节面板；计划不等同功能已交付。
- [49627920](https://www.coolapk.com/feed/49627920) 描述 Scene 7 Core 预览：简化界面、自定义配置、游戏列表和通知栏刷新率切换；作者称 FAS 不通过 Hook SurfaceFlinger 或 libgui 实现，并讨论刷新率与游戏帧率匹配。反作弊兼容与账号安全声明未独立验证。[应用页](http://www.coolapk.com/apk/com.omarea.vtools)。

## 触控与编译

[47132708](https://www.coolapk.com/feed/47132708) 以 Redmi K60 Pro 为例，称触控控制涉及 `/dev/xiaomi-touch` 的 ioctl，另提及 `/sys/class/touch/touch_dev/touch_thp_cmd`；作者实验中 Goodix 的 `switch_report_rate` 日志并不证明实际采样率被修改。文中使用 `su -c "getevent -r"` 观察事件频率，但事件间隔受输入行为、驱动及调度影响，不能等同面板硬件采样率。删除设备节点是侵入性实验，不作为通用操作，重启及系统行为未验证。

[47344048](https://www.coolapk.com/feed/47344048) 在 Android 12 上比较 QQ 安装后、speed 编译后及删除 odex/vdex 的占用，观察到系统可能在重启或后台重新优化；作者提及 `install-fast=skip`，并指出旧 Android 版本模式不同。单次观测不能保证节省空间、性能或跨版本兼容；直接删除运行时产物可能导致重编译或故障。

## 散热和 SoC 对比

[46867410](https://www.coolapk.com/feed/46867410) 使用拆除摄像头并加硅脂的 K50 Pro、标称 27W 背夹和高负载游戏，约三分钟接近过热关机，电池接近 40°C；作者不确定背夹是否处于最大功率。改装设备、散热接触和功率未知，因此不构成背夹通用评测。

[47374138](https://www.coolapk.com/feed/47374138) 在高负载游戏场景记录多种 SoC 的帧率与整机功耗，例如天玑 9000 约 42fps/8.3W、8 Gen 2 电池约 42°C 时约 52fps/10W；部分设备已 Root、超频或改压，温度和 CPU 负载也不同。作者明确称数据不严谨，不能当成同条件 GPU 能效排名。

以上关联图片尚未读取，内容和效果未实机复测。

## 本地通信、FEAS 与降压风险

- [43055930](https://www.coolapk.com/feed/43055930) 称部分去广告模块删除 hosts 中的 localhost 定义，可能导致 Scene 的 UI 与高权限 daemon 回环通信失败，表现为模式切换异常或闪退。帖子建议核对 `127.0.0.1 localhost` 和 `::1 ip6-localhost`；本地流量本身不能证明访问互联网，具体仍应结合连接目标检查。该故障与修复未独立复现。
- [42895469](https://www.coolapk.com/feed/42895469) 列出当时 FEAS 支持机型、游戏与 Scene 模式，并称未适配的第三方调度或手动调频可能冲突，某些悬浮窗高频刷新可能干扰。机型支持和行为均为历史版本描述，不能套用当前版本；切换策略前先记录原配置并单变量验证。
- [40252352](https://www.coolapk.com/feed/40252352) 讨论天玑 8100/9000 降压探索，警告欠压可能引发崩溃、重启、文件损坏或无法开机；作者还警告某些早期超频方案可能造成实际硬件频率锁定与过热。具体实现和风险程度未独立验证，动手前需备份并准备对应恢复方案。

## FAS 原理和历史实现边界

[43487643](https://www.coolapk.com/feed/43487643) 描述帧时间驱动调频：单看 CPU 负载无法区分有效工作与游戏空转，按帧耗时反馈可在帧率稳定时降低频率、掉帧时释放性能。但 GPU 瓶颈、过热或游戏自身问题也会掉帧，盲目升 CPU 频率可能增加功耗；用户态帧时间采集还可能有延迟和偏差。作者称早期 Scene FAS 支持 60/90/120 帧，帧率与刷新率不匹配或过场动画可能产生误判；这些限制只针对当时版本。

[43886147](https://www.coolapk.com/feed/43886147) 对 Scene 6.1 内测的说明称，带 FAS 配置和动态响应时按游戏策略生效；开发者选项中的日志开关不是 FAS 开关。以 120Hz 显示 90fps 为例，显示间隔可能分布在约 8.33ms 与 16.67ms，控制器需避免把正常节拍误判成卡顿。帖子建议反馈附机型、长时间帧率记录及场景；其兼容性声明和设置属于历史版本，未经独立验证。

[41080525](https://www.coolapk.com/feed/41080525) 区分简单限制最高频率与动态调节：前者主要影响触及上限的场景，后者可改变响应及不同负载下的频率；第三方调度可能与系统策略或其他模块相互覆盖。作者当时计划 Scene 6 沿用 schedutil/walt 并减少为了区分模式而刻意制造的差异，这是设计意图，并非所有设备的实测结论。

## vivo X90 历史测试

[41307974](https://www.coolapk.com/feed/41307974) 对 vivo X90 和天玑 9200 的性能与功耗给出早期观点，强调持续性能、散热和内存配置共同影响结果；其中竞争和产品配置推断未提供可复现测试，不应视作硬件定论。

[41517377](https://www.coolapk.com/feed/41517377) 使用 X90 2800×1260、室内 Wi-Fi、双卡 5G、亮度条约 35% 测试；原神极高画质 60fps 设定、约 18°C 环境下六轮平均约 51.85fps、6.18W，测试后电池最高约 34.2°C。作者记录 Wild Life Extreme 20 次循环 X90 整机平均 11.58W、电池最高 50.8°C；比较 X80 时指出负载与降频影响，不能据此直接计算同性能能效。4K 120 HDR 本地播放测试中，X90 起初输出 120fps 后降至 60fps；这是当时单机和软件版本的观察。九张图片未读取，所有数值未经独立复测。

## Scene 5 外部配置及监测

[35309329](https://www.coolapk.com/feed/35309329) 描述 Scene 5 外部配置接口：切换模式时执行 `sh /data/powercfg.sh [mode]`，模式包括 `init`、`powersave`、`balance`、`performance`、`fast`、`pedestal`。`/data/powercfg.json` 的 `features.strict` 与 `features.pedestal` 声明适配；`state` 可指向状态文件或属性，`module` 列相关模块 ID。严格模式和动态响应启用后，脚本中的 `top_app`/`scene` 可表示前台应用，息屏十秒后可收到 `standby`；文中称当时 `entry` 和 `projectUrl` 字段尚不起作用。路径、字段与权限均需按实际版本验证；配置脚本可执行高权限操作。参考 [uperf 项目](https://github.com/yc9559/uperf) 和 [Scene 应用页](http://www.coolapk.com/apk/com.omarea.vtools)。

[39767537](https://www.coolapk.com/feed/39767537) 区分瞬时频率与上一采样周期负载：文中以固定 2GHz、利用率 93% 举例得到约 1860M cycles。作者用该指标观察温度上升与目标频率、实际频率差异；周期数不是跨架构的完成工作量或性能评分，采样方式、运行时间和硬件计数定义需分别核实。图片未读，测试未经复现。

[34082469](https://www.coolapk.com/feed/34082469) 将 Scene 无法取得 Root 的排查分为网络/本地通信限制、Root 授权被拒或隐藏、安装异常及历史版本缺陷；帖子称旧版试用到期和大跨度升级问题可通过更新后重启处理。具体版本与管理器行为可能不同，应先核对授权状态与本地连接。

## 频率和负载案例

[36015817](https://www.coolapk.com/feed/36015817) 强调 DDR `MinFreq` 是下限，而非固定频率或上限。作者在 K50 游戏场景报告下限 800 时约 30–58fps、4266 时约 47–60fps、5500/6400 时约 53–60fps，对应电流约 1.9/2.0/2.1A；负载、温度与测量条件不完整，不能直接推广或把电流当成功率。提高下限可能改善受内存限制场景，也可能增加闲置功耗。

[34809523](https://www.coolapk.com/feed/34809523) 记录当时 K50 Pro / 天玑 9000 系统的 CPU `policy0`、`policy4`、`policy7` 和 GPU `/sys/class/devfreq/13000000.mali/` 接口、cpuset/cpuctl 与 core_ctl；文中还列出旧版设备的频点及温控目录。节点、单位与权限因内核和设备而异，不能直接照搬写入；帖子也提醒线程固定可能与系统调度冲突。关联[频率测试](https://www.coolapk.com/feed/34503960)尚未读取，图片亦未读。

## 游戏与温度观察

[36457823](https://www.coolapk.com/feed/36457823) 讨论天玑 8100 游戏 120fps/HDR 的调度对比，但关键数据在未读图片中，不据正文断言哪套方案更好。[35349602](https://www.coolapk.com/feed/35349602) 在改装设备和外置背夹下记录游戏瞬时 CPU 温度与电流；不同模式、散热条件无法构成公平性能对比。[34502499](https://www.coolapk.com/feed/34502499) 是 K50 Pro 上市初期体验：作者称默认 FHD、压力测试频率下降及游戏频率限制，关键曲线与充电数据在未读图片中；观点不代表后续系统表现。

## Scene 激活与版本沿革

[46265741](https://www.coolapk.com/feed/46265741) 描述通过 Shizuku 配对无线调试，再启动 Shizuku、选择 Scene 的 ADB 模式并授权；帖子称部分 MIUI 版本需 USB 调试安全设置和原生通知样式，OriginOS 有时需手动打开 Shizuku。不同系统菜单及权限取决于版本；图片未读，未实机验证。

[30902177](https://www.coolapk.com/feed/30902177) 是 Scene 4.7 发布前的历史说明，提供 [应用官网](http://vtools.omarea.com/) 和 [历史版本发布页](https://github.com/helloklf/vtools/releases)；链接和文件未验证。[32520247](https://www.coolapk.com/feed/32520247) 介绍 Scene 5 公测计划，包括基础、ADB 和 Root 模式及性能监视等功能；当时的付费及激活政策不代表现行政策。[32716184](https://www.coolapk.com/feed/32716184) 是 Scene 5 早期激活套餐和账号故障公告，历史积分、注册与套餐条件可能已经改变，不应当作当前购买依据。

[46247682](https://www.coolapk.com/feed/46247682) 称自 2023 年 7 月起部分中低端或旧 SoC 从逐型号转为按核心规模使用通用配置，例如 `snapdragon-62`、`snapdragon-611`、`dimensity-62` 和 PPM 变体；支持矩阵仅反映当时的配置。[42086340](https://www.coolapk.com/feed/42086340) 建议先体验原厂调度；作者称卸载 Joyose 可能影响部分机型的 FEAS，依赖具体系统版本，并非所有机型通用。

## 手动 OTA 与历史设备测量

[31441339](https://www.coolapk.com/feed/31441339) 记载已 Root 的虚拟 A/B 设备通过 `update_engine_client` 读取全量 OTA 中的 `payload.bin` 与 `payload_properties.txt`，观察 `onPayloadApplicationComplete` 返回码；`kSuccess` 是操作返回成功，仍需检查启动和系统状态。帖子强调备份，并称更新后需重新获取 Root。标题中的“任意版本”不等于跨版本、跨机型或降级一定受支持；反回滚、签名、分区与兼容条件未独立验证。[历史 ROM 入口](http://www.miui.com/getrom.php) 未验证。

[32992447](https://www.coolapk.com/feed/32992447) 记录 realme GT Neo 天玑 1200 的历史游戏测试：预热约 45°C、GPU 手动设 750MHz，约 3 分 46 秒测得 58.3fps、99.1% 时间不低于 45fps，结束温度约 48.3°C；另一次约十分钟游戏升至 52°C 并出现降频。测量为特定设备、场景与设置，九张图片未读，不能代表当前系统或普遍表现。

## 补充历史观察

- [34505953](https://www.coolapk.com/feed/34505953)：作者报告天玑 9000 某游戏场景约 115±5fps、平均电流低于 1200mA；具体设备、亮度、电压及两张图未核实，不能换算为可比功率或推广至其他机型。
- [28968088](https://www.coolapk.com/feed/28968088)：针对 Scene 4 通过模块转为系统应用后的更新问题，帖子建议先移除对应模块并重启，再更新或切回普通应用；模块和系统机制可能随版本变化，三张图片及具体操作未验证。
- [35333908](https://www.coolapk.com/feed/35333908)：K50 Pro 历史单场景实验，4K 在线视频、2K/60Hz、弹幕、Wi-Fi、亮度条 50%，同时后台运行两线程压力测试；记录 Uperf 713mA、Scene 756mA、原厂 1032mA。电池电压、温度和重复测试不足，不能作为普遍能效排名。
- [27952440](https://www.coolapk.com/feed/27952440)：Scene 4 Core 历史说明称保留性能监视和调度，支持 `/data/powercfg.sh` 外部配置；[应用页](http://www.coolapk.com/apk/com.omarea.scene)、[相关 Uperf 帖](https://www.coolapk.com/feed/27893833)。链接目标和历史兼容性未验证，图片及关联帖未读。

## 历史监测和 MTK 接口

[24444971](https://www.coolapk.com/feed/24444971) 称旧版 Scene 迷你监视器在后半数核心中存在高于 70% 且高于多核平均的负载时显示该单核值，否则显示多核负载；这是展示策略，不是系统总 CPU 利用率的统一定义。[18953694](https://www.coolapk.com/feed/18953694) 描述 Scene 3 内置、在线、本地导入、外部 `/data/powercfg.sh` 和自定义配置，称当时不支持部分模式自定义混搭；历史规则不一定适用当前版本。

[21121775](https://www.coolapk.com/feed/21121775) 讨论旧 MTK PPM 接口：频率表索引与频率值不同，`-1` 在其示例中代表未设置；关闭 PPM 可能连带影响 boost。具体频率表在未读图片中，[原文引用](https://cpu52.com/archives/229.html)未验证。[21088366](https://www.coolapk.com/feed/21088366) 列出历史 GED 参数，但作者未解释大部分参数含义，仅提示强制 MDP 可能影响视频硬解；不应把节点名称或示例值当作通用调优方案。接口与效果随设备和内核变化。

## 骁龙 855/865 单机 GPU 观察

[24381602](https://www.coolapk.com/feed/24381602) 报告骁龙 865 在 NOM、730MHz 和液冷条件下 GPU 压测整机功耗约 5.2W；作者称 NOM_L1 比 NOM 增约 1.6W。具体电压和压力测试条件在未读图片中。[24279207](https://www.coolapk.com/feed/24279207) 报告另一颗骁龙 855 在 SVS_L1 610MHz 的稳定性观察，并称该场景约 5.2W；超频和电压变化与设备体质相关，两帖数据不能直接作为跨设备能效定论。图片、完整评论未读，均未独立复测。
