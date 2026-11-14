# 已获取 Root 的 Android 模块建议与配置

> 来源：[酷安 feed 64833996](https://www.coolapk.com/feed/64833996)。原文主要基于作者个人使用，偏向小米设备；这是经验分享，不是各设备通用的兼容性保证。原文有 9 张图片，但本参考依据提供的文字和链接锚点整理；图片中的额外信息无法确认。

## SukiSU Ultra / SUSFS 内核的功耗排查

原文称，部分带 SUSFS 支持、用于获取 Root 的内核刷入后功耗较高。作者建议按以下方向排查：

- 移除自己添加的设置，例如 ZRAM 格式或大小参数，以及额外的调度配置。第三方内核作者可能已内置调度，用户配置可能与之冲突。
- 改善墓碑或冻结机制，减少待机功耗。
- 若上述调整无效，换用其他来源的 AK3 内核包。
- 也可刷入官方 `boot` 恢复默认内核，再修补 `boot` 或 `init_boot`，改用 Magisk 获取 Root。
- 官改包没有单独 `boot` 文件时，原文建议等包作者提供更新包后刷入全量包以恢复内核；具体是否适用需按设备和 ROM 判断。

这是原文给出的排查经验，没有提供功耗测量数据或跨设备测试结果。作者另称 SukiSU 官方提供的功耗表现较高、优化不足，属于其经验评价。

## Zygisk、Sui 与 LSPosed

### Zygisk-Next

项目：[Dr-TSNG/ZygiskNext](https://github.com/Dr-TSNG/ZygiskNext)

原文描述它提供 Zygisk API，供需要 Zygisk 的模块挂载使用；KernelSU 和 SukiSU Ultra 用户需挂载此模块，Magisk 用户通常可直接启用内置 Zygisk。原文特别提醒，Delta 面具内置的 Zygisk API 较旧，建议在管理器中关闭内置 Zygisk，再刷入该模块。需要 Sui 或 LSPosed 时，原文建议提供 Zygisk 支持；Magisk 用户可在 App 设置中开启相应选项。原文认为旧版可能造成一定程度的环境泄露，并建议保持更新，未给出版本号或量化测试。

### Sui

项目：[RikkaApps/Sui](https://github.com/RikkaApps/Sui)

原文称 Sui 提供 Shizuku API 支持并在开机时自启；应用申请 Shizuku 权限时会弹出授权窗口。原文认为它适用于需要文件权限访问的场景，并称执行速度优于 Root、授权相对安全；同时明确这属于 Shizuku Root 级权限，不是通常的 Shizuku ADB 权限。

原文记载 Sui 依赖 Zygisk，并建议先安装 Zygisk-Next。这是原文所述的依赖与安装顺序；不要据此推断必须使用 ZygiskNext 的特定分支或后续版本。

### LSPosed

项目：[LSPosed/LSPosed](https://github.com/LSPosed/LSPosed)

原文称其用于 LSPosed Hook，并称官方当时不再对外提供某些版本、仍有版本流出；未提供具体版本或可靠获取渠道。原文指出当前开源版本可能不适用于 Android 15，但没有列出版本号或兼容性测试。作者不建议使用所谓“去日志版”等修改版，理由是无法确认其相对原版的改动及安全性；原文也认为此类修改并不可靠地实现环境隐藏。

原文称该模块依赖 Zygisk-Next。此为该帖的历史经验记录，不代表已核实的当前项目要求。

## 温控、充电与文件系统维护

### 温控解除

作者使用的设备为 Redmi K70 Pro；其推荐对象是用于 MIUI / HyperOS 的移除温控模块。原文称理论上适用于小米系设备，官改 ROM 可能无需另刷；其他品牌或机型应找对应模块。原文没有给出该温控模块的项目或下载链接，不能据此识别具体模块。

### PID 充电模块

原文描述此类模块可依据设备温度调整充电功率，以减轻快充造成的发热。作者所用模块对设备有要求，原文没有提供项目或下载地址；非小米设备需另找适配模块。作者称与前述温控模块搭配效果好，但没有提供测试数据。

### GC 类模块

原文讨论 F2FS 环境下的 GC：通过回收旧区块，意在减少 UFS 寿命损耗。作者承认对这一概念的解释较粗略，并将长期使用类原生系统的体感改善作为个人观察，不能视作量化验证。

原文称其提供的资源包含一次性 GC 模块，适用于不想长期让设备随机处于 GC 状态，或无法安装 GC 模块触发的设备；操作方式是使用 MT 管理器并授予 Root 权限。该帖未提供该资源的可恢复项目链接或具体文件信息。若系统已有对应功能（例如澎湃系统的“焕新存储”）且能正常触发，原文建议不必安装；虽可刷入模块查看 GC 状态，作者不建议这样做。

## 图标与完整性检查相关模块

### 完美图标计划

项目：[pzcn/Perfect-Icons-Completion-Project](https://github.com/pzcn/Perfect-Icons-Completion-Project)

原文称模块可补全 MIUI 图标，并可一定程度改善应用启动遮罩与动画。它不是直接刷入：先下载并安装 APK，再按设备情况和需求选择选项，下载生成或对应的模块后自行刷入。原文标明仅适用于小米设备或搭载 MIUI / HyperOS 的设备；未提供具体 APK 版本号。

### TrickyStore

项目：[5ec1cff/TrickyStore](https://github.com/5ec1cff/TrickyStore)

原文将其描述为修改 Android Keystore 生成的 Android KeyAttestation 证书链的模块，称适用于 Android 10 及以上，并用“提供一个 key 模板”作通俗解释。原文没有提供配置步骤或测试结果。

### Play Integrity Fix

项目：[chiteroman/PlayIntegrityFix](https://github.com/chiteroman/PlayIntegrityFix)

原文称模块尝试修复 Play Integrity 和 SafetyNet 判决以取得有效认证；作者强调其本意不是“隐藏”，而是在设备具备设备完整性（例如 TEE）的条件下维持其状态报告。原文推测将 TrickyStore 与 Play Integrity Fix 一起刷入后，小米设备理论上可通过 Momo 检测，并称可达到日常使用的隐藏效果；这是原文推测，不是来源提供的实测结论或兼容性保证。

## 本机 DNS 过滤：AdGuardHomeForRoot

项目：[twoone-3/AdGuardHomeForRoot](https://github.com/twoone-3/AdGuardHomeForRoot)

原文称该模块能在 Android 设备本地部署 DNS 服务并配置上游过滤，以减少网络跟踪器和垃圾广告加载。刷入并重启后，按原文步骤配置：

1. 用手机默认浏览器打开 `http://127.0.0.1:3000/#`。
2. 登录 AdGuard Home 后台；原文给出的初始账号和密码均为 `root`。
3. 进入“过滤器”→“DNS 黑名单”→“添加黑名单”→“从列表中选择”。
4. 选择 `AWAvenue Ads Rule`，然后保存。已添加的规则在列表中会变灰。
5. 点击“检查更新”。如果更新一直转圈，在 Magisk、KernelSU 或 SukiSU 管理器的模块页，连续点击 AdGuardHomeForRoot 模块下的按钮两次，以重启服务。

原文称配置后设备的 DNS 请求会经过此服务；若某些服务异常，可在模块页点击按钮暂停，再点击一次恢复。此处是原文的使用描述，实际流量覆盖情况取决于设备与配置。

规则项目：[TG-Twilight/AWAvenue-Ads-Rule](https://github.com/TG-Twilight/AWAvenue-Ads-Rule)。原文将其称为秋风广告规则，并报告个人使用体验良好；同时提醒若误拦截过多，可按上述步骤移除规则。

## 文件占用整理：苏柚

项目：[YumeYuka/SUU-Nyanya](https://github.com/YumeYuka/SUU-Nyanya)；原帖另明确指向相关酷安说明：[feed 62491945](https://www.coolapk.com/feed/62491945)。

原文介绍该 App 和模块用于梳理、规范 Android 设备文件占用，适合没有文件分类习惯、找文件不方便的用户体验。原文称相关说明与资源分类见对应文档，并提醒因更新较快，帖中索引可能不是最新，应前往项目了解最新版本。未提供进一步操作步骤或版本号。

## 对其他常见模块的取舍说明

- **救砖模块：**原文作者认为 Magisk 用户安装自动救砖模块有用；但其称 KernelSU 已提供默认开启的核心模式，SukiSU Ultra 作为下游分支继承了这一点，因此认为额外救砖模块已非必要步骤。此为作者对功能与使用趋势的判断。
- **Shamiko：**项目：[rushizgithub/shamiko](https://github.com/rushizgithub/shamiko)。原文称 Magisk / KernelSU 用户过去常用该模块；作者认为采用带 SUSFS 支持的 SukiSU Ultra Root 方案时，是否刷入已成为可选项。此为原文对特定方案的经验判断。
- **Non-GKI 与 SUSFS：**原文称 Non-GKI 内核也可针对对应机型编译以支持 SUSFS，但提醒注意功耗问题；没有给出编译命令、设备列表或内核版本。
- **调度与 ZRAM 模块：**项目：[yinwanxi/Uperf-Game-Turbo](https://github.com/yinwanxi/Uperf-Game-Turbo)。原文不建议在第三方内核已经提供调度时再叠加调度或线程优化模块，认为可能使处理器状态不稳定。内核级调度、ZRAM 支持或强制配置可能优先于模块调整；遇到调度失控或 ZRAM 不可控时，先移除自加的调度模块或停用软件调度（例如 Scene），并检查是否与内核作者的配置冲突。作者认为倾向自主控制时可用 Magisk、避免刷第三方内核；这是其建议，不是普遍测试结论。

原文最后概括其使用趋势判断：刷入第三方作者提供的 AK3 内核包，可能已完成基本优化，未必需要逐项修正。此为作者观点；实际结果依设备、内核与配置而异。

## 来源未提供的信息

原文没有给出温控解除模块、PID 充电模块及 GC 模块的可恢复项目链接，也没有给出具体版本、安装包名称或命令。LSPosed 当时提及的特定流出版本没有版本号或获取地址。原文虽有 9 张图片，但图片中的内容未纳入可见文字，无法据此补充其他链接、设备清单或参数。
