# 分类说明、配置与资源

按原帖的功能分类重新组织。下载站、项目页、介绍帖和文件直链分别按其实际用途标注；介绍帖不等于已获得安装包。

## 救砖模块

文章列出两种模块。神仙自动救砖在连续重启 3 次或停留开机界面 2 分钟时禁用所有模块；OTA 升级时，等待判定时间会延长到 30 分钟。仍无法开机时会进入 APP 解冻救砖模式。模块目录中的“白名单.conf”可填写希望跳过救砖处理的模块。音量键极速救砖在开机 1 秒后监听按键：音量加三击进入 Magisk 核心模式并禁用所有模块，音量减三击禁用 Xposed 模块。禁用模块后再次重启时，模块还会尝试恢复应用卸载、伪装、误禁用组件和误冻结等状态，并可禁用无障碍服务。文章称可在爱玩机的 Magisk 模块管理“智能救砖”查看触发次数；多次重启次数显示为 0 时，模块可能未生效。

### 操作与行为

- 神仙自动救砖：满足连续重启 3 次或开机界面停留 2 分钟后，系统自动禁用所有模块；OTA 后等待时间延长至 30 分钟。
- 需要救砖时仍无法开机，模块会尝试执行 APP 解冻救砖模式再启动。
- 可在模块目录的“白名单.conf”添加要跳过救砖处理的模块。
- 音量键极速救砖：开机 1 秒后，音量加三击触发 Magisk 核心模式；音量减三击触发 Xposed 模块禁用。
- 在爱玩机 Magisk 模块管理的“智能救砖”查看触发次数；若多次重启后计数为 0，按文章描述模块无效。

### 参数与路径

- 自动救砖触发：连续重启 3 次，或卡在开机界面 2 分钟。
- OTA 后开机等待判定时间：30 分钟。
- 音量键救砖触发时点：开机 1 秒后；对应按键均需三击。

### 适用条件

- 文章未给出这两种救砖模块的 Android、Magisk、机型或模块版本要求。
- 音量键极速救砖涉及 Magisk 与 Xposed 模块禁用；具体 Xposed 实现和兼容条件未说明。

### 故障与恢复

- 模块未触发时，文章给出的检查线索是多次重启后的救砖次数仍为 0。
- 自动救砖后仍无法启动时，模块会尝试解冻应用再开机；文章没有说明失败后的人工恢复步骤。

## 调度模块

文章整理了多类 CPU 或帧感知调度方案。yc / Uperf 系列有原版、多个作者的修改版及偏重游戏性能的 Uperf 衍生版。ct 调度可通过酷安站内 CuToolbox 启动，作者提示不同版本的调度实现不同；ct 与 AsoulOpt 不可同时开启。Scene EP/LP/HP 需要 Scene 5，原帖历史标价为 12 元，现价未核对；在 Scene 中选择调度、开启并重启。fas-rs 在用户态运行 FAS（Frame Aware Scheduling），支持插件。cs 调度标注支持 Android 10–14，但其内置挂载的 schedhorizon 仅适用于 Linux 内核 5.10–5.15；设备支持该调速器时也可使用。另列出 YukiCpuScheduler，但正文没有说明操作和兼容条件。

### 操作与行为

- ct：在酷安站内下载 CuToolbox 并启动调度；不同版本调度实现不同。
- Scene EP/LP/HP：安装 Scene 5，在应用中选择并开启调度，然后重启设备。
- fas-rs：调度运行于用户态，可搭配插件；文章未给出安装或配置步骤。
- cs 调度：先确认 Android 版本和内核/设备的 schedhorizon 支持情况；文章未给出安装步骤。

### 参数与路径

- Scene 5：文章标注入正价格为 12 元。
- cs 调度：Android 10–14；内置 schedhorizon 挂载适用于 Linux 内核 5.10–5.15。

### 适用条件

- ct 与 AsoulOpt 不可同时开启。
- cs 调度面向 Android 10–14；内置 schedhorizon 挂载限定于内核 5.10–5.15。设备若自行支持 schedhorizon 调速器，文章称也可使用。
- Scene EP/LP/HP 依赖 Scene 5。
- fas-rs 被描述为用户态 FAS，支持插件；具体系统和机型要求未知。
- yc、Uperf 衍生版、Crazy Uperf、YukiCpuScheduler 的具体设备和系统兼容条件未说明。

### 故障与恢复

- ct 与 AsoulOpt 同时开启存在明确的互斥限制；文章未提供冲突后的恢复步骤。
- cs 调度的内置 schedhorizon 挂载有内核版本范围限制；不符合时文章未提供替代方案。

### 公开资源

- [yc / Uperf 项目链接；正文未明确细分为原版或特定修改版](https://github.com/yc9559/uperf)
- [Uperf 游戏性能衍生版发布页](https://github.com/yinwanxi/Uperf-Game-Turbo/releases)
- [Crazy Uperf 酷安介绍](https://www.coolapk.com/feed/61915486)
- [Scene 应用页面（不是 CuToolbox）](https://www.coolapk.com/apk/com.omarea.vtools)
- [SCENE8 腾讯频道](https://pd.qq.com/s/5vcohvfds)
- [fas-rs 发布页](https://github.com/shadow3aaa/fas-rs/releases)
- [cs 调度酷安介绍](https://www.coolapk.com/feed/63235988)
- [YukiCpuScheduler 酷安介绍](https://www.coolapk.com/feed/65930205)
- [YukiCpuScheduler 项目](https://github.com/imacte/YukiCpuScheduler_update)

## 墓碑后台模块

文章将 NoActive、Freezer 和“冻它（lambda）”列为墓碑后台类模块，并概括它们的大功能相近，差异在细节功能。正文没有给出具体配置步骤、机制差异、版本或设备条件。

### 适用条件

- 三款模块的 Android 版本、机型和 Root 框架要求未说明。

### 公开资源

- [Android 墓碑腾讯频道](https://pd.qq.com/s/2mxuv1a23)
- [NoActive 官网](https://app.myflv.cn/)
- [NoActive 下载站](https://down.app.myflv.cn/)
- [Freezer 酷安介绍](https://www.coolapk.com/feed/64079642)
- [冻它（lambda）酷安介绍](https://www.coolapk.com/feed/64448959)

## 内存优化与后台保活

Scene 附加模块（二）修改版被描述为保后台、省电，极限保后台相关配置位于 /data/adb/modules/swap_controller/swap/。运存防线的目标用户包括不常清理后台、内存较紧张（举例 6GB、8GB）、后台自启严重，以及同时使用墓碑和保后台后出现卡死或自动重启的场景。DZram 被描述为会动态扩大或缩小 Zram。文章没有提供具体参数和启用步骤。

### 适用条件

- 运存防线举例面向 6GB、8GB RAM 等内存吃紧设备，但没有给出硬性限制。
- Scene 附加模块（二）修改版的 Android、机型和 Scene 版本要求未说明。
- DZram 的系统与设备要求未说明。

### 故障与恢复

- 文章指出墓碑模式与保后台同时使用时，后台过多可能导致手机卡死并自动重启；没有说明具体恢复步骤。

### 公开资源

- [Scene 附加模块（二）修改版介绍](https://www.coolapk.com/feed/47697375)
- [运存防线介绍](https://www.coolapk.com/feed/44289299)
- [DZram 模块介绍/资源页](https://200ok.work/#go/mod/mg/dzram)

## 清理模块

文章列出苏柚-SUU、CleanZero、Aurora 清理和 ClearBox。共同的使用提醒是先查看白名单，避免误清理。文章未展开清理范围、规则或配置操作。

### 操作与行为

- 使用清理模块前先查看其白名单，避免误清理。

### 适用条件

- 各清理模块的 Android 版本、机型和 Root 框架要求未说明。

### 故障与恢复

- 可能发生的风险是误清理；文章仅建议使用前查看白名单，未给出误删后的恢复方法。

### 公开资源

- [苏柚-SUU 网站](https://suu.oom-wg.dev/)
- [苏柚-SUU 酷安介绍](https://www.coolapk.com/feed/43678549)
- [CleanZero 酷安介绍](https://www.coolapk.com/feed/44281693)
- [Aurora 清理酷安介绍](https://www.coolapk.com/feed/57934034)
- [ClearBox 酷安介绍](https://www.coolapk.com/feed/65652590)

## 美化与 Monet 取色

Hyper OS Monet Project 3.0.0 高级版用于为 Hyper OS 系统应用启用 Monet 取色。Monet-All 是一个莫奈取色整合模块。正文没有写安装方式、配置步骤或兼容范围。

### 参数与路径

- Hyper OS Monet Project：正文列出的版本为 v3.0.0（高级版）。

### 适用条件

- Hyper OS Monet Project 面向 Hyper OS 系统应用；其他系统兼容性未说明。
- Monet-All 的系统和机型兼容范围未说明。

### 公开资源

- [Hyper OS Monet Project 酷安介绍](https://www.coolapk.com/feed/63580604)；原帖版本或发布标签：v3.0.0（高级版）
- [Monet-All 个人博客介绍](https://blog.jerryz.com.cn/article/Monet-All/)
- [Monet-All 更新日志](https://monet.jerryz.com.cn/Monet-All.md)
- [Monet-All GitHub 项目](https://github.com/YangguangZhou/Monet-All)
- [Monet-All 酷安介绍](https://www.coolapk.com/feed/57595011)

## 音效增强模块

NLSOUND 仅适用于基于高通平台的设备。Hydrostellaire 按原帖描述支持其撰文时近两年的大部分一加、真我新机及部分小米机型；它从 Hydrostellaire 沨莹主模块分离，二者不能共存。xaga＆rubens 音效仅适用于 Redmi Note 11T Pro（+）和 K50。DolbyATMOS 模块要求设备系统自带杜比音效，且 DAX 路径为 /system/vendor/etc/dolby/。调音基于 Redmi K50U 的 1012/1216 扬声器；其他机型硬件不同会导致效果差异，非 K50U 可能音量偏小。文章建议音量保持 60%–80%，不要大幅调整系统均衡器；配置文件位于 /data/skai/audio.conf，修改后保存并重启生效。Tinymix 调整音量增益等功能仅适用于 K50U。

### 操作与行为

- 安装 DolbyATMOS 前，确认设备自带杜比音效，并检查 DAX 文件路径是否为 /system/vendor/etc/dolby/。
- DolbyATMOS 配置文件为 /data/skai/audio.conf；修改并保存后重启生效。
- 使用 DolbyATMOS 时，文章建议音量在 60%–80%，系统均衡器仅作小幅微调。
- Hydrostellaire 与其分离来源的 Hydrostellaire 沨莹主模块不能同时安装或启用。

### 参数与路径

- DolbyATMOS 建议音量：60%–80%。
- DolbyATMOS 配置文件：/data/skai/audio.conf。
- DolbyATMOS 的 Tinymix 增益调整功能：仅 K50U。
- 文章称 1012/1216 扬声器效果最佳；此模块调音基于 Redmi K50U 的该扬声器配置。

### 适用条件

- NLSOUND：仅基于高通平台的设备。
- Hydrostellaire：近两年的大部分一加/真我新机及部分小米机型；具体型号清单未知。
- Hydrostellaire 与 Hydrostellaire 沨莹主模块不能共存。
- xaga＆rubens 音效：仅 Redmi Note 11T Pro、Redmi Note 11T Pro+ 和 K50。
- DolbyATMOS：要求系统自带杜比音效，并且 DAX 位于 /system/vendor/etc/dolby/；K50U 的 Tinymix 功能为专属。
- DolbyATMOS 在非 K50U 设备可能音量偏小；不同设备硬件会造成音效差异。

### 故障与恢复

- 非 K50U 安装 DolbyATMOS 可能音量偏小；文章建议介意者不要刷入。
- DolbyATMOS 的扬声器效果因设备硬件不同而异；大幅调整系统均衡器不被建议。
- 文章没有给出安装失败或音频异常后的恢复方法。

### 公开资源

- [NLSOUND 酷安介绍](https://www.coolapk.com/feed/57877351)
- [Hydrostellaire 酷安介绍](https://www.coolapk.com/feed/61413503)
- [xaga＆rubens 音效酷安介绍](https://www.coolapk.com/feed/57898596)
- [DolbyATMOS 音效模块酷安介绍](https://www.coolapk.com/feed/65490784)

## 震动增强模块

原帖另列单机型项目，提及 MI 10 Ultra、K50 等，但条目还含有“禁用震动等”的描述，具体子功能未展开。HapticRE 面向运行 MIUI/澎湃系统的米系 X 轴马达设备，主要适配 0809 马达，其他 X 轴马达仅称可能兼容且不保证效果；非 MIUI/澎湃 ROM 的兼容性未知。出现破音时应降低强度，其他以安装提示为准。欧加真马达振动增强复生版提供两种触感取向：纯粹清脆和增强强度的弹簧体质清脆。

### 操作与行为

- HapticRE 出现破音时降低振动强度；其他调整遵循模块安装提示。
- 欧加真马达振动增强复生版可选择纯粹清脆或弹簧体质清脆模式。

### 参数与路径

- HapticRE 主要适配 0809 X 轴马达。
- 欧加真马达振动增强复生版：纯粹清脆偏触感，弹簧体质清脆用于增强强度。

### 适用条件

- Friend1y 条目提及 MI 10 Ultra、K50；具体兼容版本和功能对应关系未知。
- HapticRE：主要适配米系 X 轴马达及 MIUI/澎湃 ROM；其他 X 轴马达效果不保证，非 MIUI/澎湃 ROM 兼容未知。
- 欧加真马达振动增强复生版的具体型号清单未说明。

### 故障与恢复

- HapticRE 可能出现破音；文章给出的处理是降低强度。

### 公开资源

- [HapticRE 酷安介绍](https://www.coolapk.com/feed/65831915)
- [欧加真马达振动增强复生版酷安介绍](https://www.coolapk.com/feed/63863509)

## 快充模块与电量统计

文章提醒快充模块风险较高，且不同手机效果不同。“充电加速 and 精准电量”模块的参数文件在 /data/adb/turbo-charge/option.txt，日志在同目录 log.txt。精准电量功能可通过 /data/adb/accurate_battery/no_trickle 控制是否把涓流充电过程计入电量统计：创建该文件表示不计入，删除表示计入，操作即时生效且无需重启。FFC 强制快充称适配多设备，主要为小米系和欧加真系，并明确不包含移除温控功能。

### 操作与行为

- 在 /data/adb/turbo-charge/option.txt 调整“充电加速 and 精准电量”模块参数；日志文件位于同目录的 log.txt。
- 创建 /data/adb/accurate_battery/no_trickle：不把涓流充电过程计入电量统计。
- 删除 /data/adb/accurate_battery/no_trickle：把涓流充电过程计入电量统计。
- no_trickle 文件的创建或删除即时生效，无需重启。

### 参数与路径

- 配置文件：/data/adb/turbo-charge/option.txt。
- 日志文件：/data/adb/turbo-charge/log.txt。
- 涓流计量开关：/data/adb/accurate_battery/no_trickle；存在表示不计入，删除表示计入。

### 适用条件

- 文章称快充效果因手机而异，未给出“充电加速 and 精准电量”的具体适配清单。
- FFC 适配多设备，主要为小米系和欧加真系；文章明确其不包含温控移除功能。

### 故障与恢复

- 文章将快充模块整体标为风险较高，并提示不同手机效果不同；没有列出具体故障或恢复操作。

### 公开资源

- [充电加速 and 精准电量酷安介绍](https://www.coolapk.com/feed/42722676)
- [FFC 强制快充模块资源页](https://200ok.work/#go/mod/mg/ffc)

## 关闭 logd 模块

文章列出“关闭 logd”和 logDisabler。前者描述为关闭 logd 服务、关闭 EdXposed、Magisk、MIUI 内测桌面及 MIUI 内核日志，并在开机 5 秒后杀死 logd；还提到清理并禁用无用 Wi-Fi 日志，以及未具体说明实现方式的待机优化。logDisabler 只作简要介绍，没有操作或兼容说明。

### 操作与行为

- 关闭 logd 模块据称会在开机 5 秒后杀死 logd 服务；文章未给出安装和配置步骤。

### 参数与路径

- 关闭 logd 模块的 logd 处理时点：开机 5 秒后。

### 适用条件

- 提及 EdXposed、Magisk、MIUI 内测桌面和 MIUI 内核日志；适用系统版本、机型及 Magisk 版本未知。

### 公开资源

- [logDisabler 酷安介绍](https://www.coolapk.com/feed/42174985)

## 温控移除模块

移除温控正式版用于移除各厂商温控模块。Karakuchi 被描述为尽可能将所有传感器温度固定为 25°C，并处理部分温度墙。Moka 是通用温控移除模块，可与 Karakuchi 同时使用。Moka Lite 相比 Moka 侵入性更低，移除大部分温控，并集成简单的环境隐藏功能；也可与 Karakuchi 同时使用。文章没有提供安装步骤，也没有列出具体厂商或机型。

“固定为 25°C”是原帖对传感器读数处理的说法，不能据此判断设备真实温度已降至 25°C。

### 参数与路径

- Karakuchi 的传感器温度目标：25°C。

### 适用条件

- 移除温控正式版：泛称适用于各大厂商，具体机型和系统版本未说明。
- Moka、Moka Lite 与 Karakuchi 可同时使用。
- Karakuchi 对传感器温度和温度墙的具体机型覆盖范围未知。

### 公开资源

- [移除温控正式版酷安介绍](https://www.coolapk.com/feed/57872829)
- [Karakuchi 模块资源页](https://200ok.work/#go/rkk-mod/mg/karakuchi)
- [Moka 模块资源页](https://200ok.work/#go/mod/mg/moka)
- [Moka Lite 模块资源页](https://200ok.work/#go/mod/mg/moka/lite)

## 性能与线程优化

AsoulOpt 被描述为线程调整程序，支持主流游戏及部分小众游戏。线程优化模块被描述为可自定义线程，且有许多二次创作版本。文章未提供配置方式、参数或兼容版本。

### 适用条件

- AsoulOpt 支持主流游戏及部分小众游戏；具体游戏名单、Android 版本和设备要求未说明。
- ct 调度不可与 AsoulOpt 同时开启，详见调度模块部分。
- 线程优化模块的系统与机型要求未知。

### 公开资源

- [AsoulOpt GitHub 项目](https://github.com/nakixii/Magisk_AsoulOpt)
- [线程优化模块酷安介绍](https://www.coolapk.com/feed/65931844)

## 其他模块：去广告、Zygisk Next 与 Shamiko

去广告模块组合使用 hosts、chattr、pm 和 iptables；规则来自网络及作者手动抓取。文章指出规则超过 5 万条会轻微影响网络，超过 50 万条可能带来一定网速延迟。Zygisk Next 的用途被描述为在 KernelSU（KSU）中启用 Zygisk。Shamiko 仅列出名称与 GitHub 链接，未说明功能细节或操作。

### 参数与路径

- 去广告 hosts 规则超过 5 万条：文章称可能轻微影响网络。
- 规则超过 50 万条：文章称网速可能出现一定延迟。

### 适用条件

- Zygisk Next：用途针对在 KSU 中启用 Zygisk；版本兼容范围未说明，链接指向 v1.1.0。
- Shamiko：正文未提供兼容条件，链接指向 shamiko-344 发布标签。

### 故障与恢复

- 去广告规则数量过大可能影响网络速度；文章只给出规则数量与影响的描述，没有提供恢复或清理规则的方法。

### 公开资源

- [Zygisk Next v1.1.0 发布页](https://github.com/Dr-TSNG/ZygiskNext/releases/tag/v1.1.0)；原帖版本或发布标签：v1.1.0
- [Shamiko 344 发布页](https://github.com/LSPosed/LSPosed.github.io/releases/tag/shamiko-344)；原帖版本或发布标签：shamiko-344



## 补全的原帖介绍入口

- [Apodidae介绍](https://www.coolapk.com/feed/54471247)，属于ct调度段落的变体入口。
- [fas-rs介绍](https://www.coolapk.com/feed/64618435)，与其GitHub发布页分别保留。

依据：[原始模块整合帖](https://www.coolapk.com/feed/47750576)。只移除了酷安分享追踪参数，未改变帖子ID；未读取这两篇目标正文。

fas-rs v4.9.0介绍帖另给[GitHub发布页](https://github.com/shadow3aaa/fas-rs/releases)和[蓝奏云镜像](https://wwr.lanzoui.com/b019jb8mb)，公开提取码：`22bw`。介绍帖标题版本不证明镜像当前文件仍是同版；未下载核验。
