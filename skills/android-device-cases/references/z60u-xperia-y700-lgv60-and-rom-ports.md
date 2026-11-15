# Z60U/Xperia/Y700/LG V60/小米平板7Pro 移植与刷机实测汇编

> 整理自酷安公开动态正文；图片、评论及外链未逐一核验，实际操作请以对应硬件版本与上游仓库为准。

- **努比亚 Z60 Ultra (NX721J) 移植 Nothing OS 4.1/5.0 实测与功能矩阵**
  - [酷安原帖](https://www.coolapk.com/feed/75044907)（`imgs=6`，配图未读）
  - **设备与环境**：努比亚 Z60 Ultra (代号 NX721J)，由 Infinity X (Android 16 QPR2) 移植卡刷 Nothing OS 4.1 / 5.0 (Android 17)。
  - **刷入路径**：TWRP 下先卡刷 Nothing OS 5.0 成功引导启动，重启 1~2 次后卡刷 Nothing OS 4.1 成功开机。
  - **可用性测试**：
    - **可用**：电话与短信正常；通过 BCR 模块补全通话录音；VoLTE（4G 语音）正常；高亮度下屏幕指纹灵敏（原厂系统与 Infinity X 识别略快）；蓝牙互联与 Google 附近分享正常；内建努比亚相机及外置 Google 相机拍照/录像均正常；铁路 12306 刷脸免隐藏验证通过（支持置入 Xposed 作用域）。
    - **异常与限制**：运营商视频通话失效（主动发起几秒后摄像头画面消失、对端收不到邀请或提示未接受，退回 VoIP）；Google 延长解锁对可信设备维持免密积极性差；缺失 Android 12 标志性四数字大锁屏时钟；屏下前摄无原厂黑色遮盖层导致成像模糊（需手动下拉黑色状态栏遮挡或修改刘海挖孔参数修复）；机器易烧屏建议关闭 AOD。
  - **功耗实测**：`am force-stop` 强停用户应用并保持 Wi-Fi 和蓝牙开启，夜间 8 小时待机耗电 5%（对比 Infinity X 为 10%~15%）；100% 满电经近 24 小时（次日晚 8 点）剩余 87%。

- **真我 Neo8「苍穹信号芯片 S1」软硬件逆向取证**
  - [酷安原帖](https://www.coolapk.com/feed/75034970)（`imgs=6`，拆机等配图未读）
  - **设备与背景**：真我 Neo8 (ColorOS 17)，官方宣称搭载「苍穹信号芯片 S1」（宣称弱网优化、高铁与校园流畅度提升 15%）。
  - **逆向验证事实与排查命令**：
    - *应用层*：反编译解包 `NetworkAssistSys_plugin_CellularData.apk` 的 `resources.arsc`，发现 `NetworkBoost Chip S1` 仅作为提示语存在（同表预置 SignalBoost G2、山海增强芯片等文案，按机型组合匹配），无 AIDL 接口或调用 Bean。
    - *内核与总线*：`/sys/bus`、`/sys/class`、I2C 及 SPI 总线上均未检索到芯片设备节点；内核模块检索仅命中 `oplus_network_rf_cable_monitor.ko`（4 组 GPIO+IRQ 负责产测天线断线检测，非信号增益驱动）。
    - *设备树与调制解调器*：`dd if=/dev/block/by-name/dtbo_a of=/tmp/dtbo.img bs=4096` 提取 dtbo，字符串检索无天线调谐器（antenna tuner/rffe）节点；基带 EFS 备份（`EfsFiles/`）中检索 qat/tuner/antenna_switch 均无对应记录。
  - **结论与限制**：拆机视频可见实体 IC，但在系统驱动与设备树层面无控制链路与联动证据；升级 ColorOS 后相关文案被移除。

- **索尼 Xperia 1 II (XQ-AT72) 刷机后读卡但无信号故障排查**
  - [酷安原帖](https://www.coolapk.com/feed/75032903)（`imgs=1`，排查流程图未读）
  - **设备与版本**：索尼 Xperia 1 II (XQ-AT72)，Android 11 / 58.1.A.5.395。
  - **故障排查诊断证据**：
    - 刷入 boot 分区重启后，SIM 卡读取正常（`getprop gsm.sim.state` 返回 LOADED）、基带存在（`gsm.version.baseband` 返回 sdx55.rmtefs-00965.4-62），但状态栏无服务且双卡无法通信；
    - RILJ 通道正常，但 Android 框架下发 `RADIO_POWER on=true` 射频上电无任何响应，`mServiceState=POWER_OFF`；
    - 执行 dumpsys 查看 `ModemActivityInfo`，显示 `mIdleTimeMs=21875`，`mRxTimeMs=0`，证实外挂射频处于完全未接收状态。
  - **根因与解决**：骁龙 865 搭配独立外挂 SDX55 调制解调器，具备独立供电轨；系统软重启（`adb reboot`）或关机无法触发基带完全断电复位。执行 `adb reboot bootloader` 进引导加载器后再执行 `fastboot reboot`，强制基带冷复位走完初始化流程，25 秒后语音与数据恢复 `IN_SERVICE`。联通/电信 2G/3G 退网后语音依赖 VoLTE。

- **联想小新 Pad Pro GT (TB7F0) 移植 ColorOS 16.0.8 模块栈与调校**
  - [酷安原帖](https://www.coolapk.com/feed/75032695)（`imgs=9`，跑分与绑核截图未读）
  - **机型与系统**：联想小新 Pad Pro GT（TB7F0，SM8650Q-AB 骁龙 8 Gen 3 领先版），运行移植版 ColorOS 16.0.8（非动分区内核版本）。
  - **模块搭配清单**：
    - *Magisk 模块*：RaTempSpoof（温控伪装）、A-SOUL Games Optimization（适配 1+2+2+1 核心架构）、Astra_MaxRefresh_Pro（分应用独立锁定高刷）、BassConf（音效）、BSwitcher（调度管理）、TB7F0 ColorOS 修复模块、Easy ZRAM、犬神志-保时捷圆字体、TB7F0 原厂 ZUI 相机移植模块、Magic Mount-rs META 元模块、Oh My Keymint、PUI 安装器 For ColorOS 17、Uperf Game Turbo（SM8650Q-AB 专用）、LSPosed、NyaZygisk。
    - *LSPosed 模块*：PUI 安装器配套模块、TB7F0 ZUI 相机兼容模块、luckytool、Pro gt 基础修复模块。
  - **实测与避坑**：实测《鸣潮》《终末地》《异环》等游戏整体处于中画质 60 帧水准；明确指出严禁刷入 4.1.3 动分区内核版本，该版本内核连带 BSP 模块存在兼容缺陷。

- **一加平板 2 Pro 移植 OriginOS 7 固件修复与底层限制记录**
  - [酷安原帖](https://www.coolapk.com/feed/75032296)（`imgs=6`，配图未读）
  - **环境说明**：一加平板 2 Pro（LCD 屏幕），移植基于 OLED 固件底层的 OriginOS 7。
  - **修复项**：修复开机向导卡顿、充电低电量误触发重启、打开应用桌面崩溃、液态玻璃入口失效、分屏与小窗、第三方相机、手电筒、双击亮灭屏、LCD 显示频闪、B站打开软重启、OPPO 键盘触控板逆时针 90 度倒错、自动亮度、MTP 文件传输模式切换，以及 OPPO/OnePlus 手写笔与键盘连接。
  - **已知 Bug 与底层副作用**：
    - SELinux 宽容模式；抬手亮屏未修；跨设备互联、原子工作台及自带相机不可用；
    - LCD 屏幕频闪修复模块无法在开机早期加载，开机前 10 秒存在屏幕饱和度抖动；
    - 针对色彩抖动强制开启 GPU 渲染，导致 Dock 栏丢失或渲染成长方形色块；
    - 密码与人脸设置原生入口缺失，需依赖外置辅助 APP 唤起；人脸录入 UI 显示上限为 5 组，底层生物识别服务实际硬性限制最多录入 3 组；键盘 AI 独立按键失效。

---

- **LineageOS 24.0 (Android 17) 红米Note9 5G (cannon) 首发刷机包**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/75013694)（`imgs=0`）
  - **固件信息与网络条件**：
    - **版本信息**：文件名 `lineage-24.0-20261111-UNOFFICIAL-cannon.zip`，约1.36GB，内核 4.14.336，安全补丁 2026-11-01。
    - **校验哈希**：MD5 `baf7aa62263cc98f40fa8a56c6210aa3`，SHA-256 `b29b8f16464dcddddea810932c407dd37333f35c49527452d11280df8432ed5a`。
    - **网络与硬件状态**：电信/联通SA下VoNR与LTE下VoLTE正常注册IMS，支持长短信；**关键限制**：联发科Modem底层要求必须开启“移动数据”开关方可注册IMS；振动马达已修复；国行机型无NFC硬件（`ro.boot.hwc=CN -> .nfc=false`）无NFC开关属正常。
    - **刷机方式**：23.2可直接更新或sideload；MIUI跨版本需fastboot刷入recovery.img，格式化Data后再ADB sideload。
    - **公开仓库**：GitHub `top-android/releases` tag `v26.11.11`。

- **联想拯救者 Y700 (tb321fu) 移植 PixelOS 安卓17 (9008免解锁包)**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/74974453)（`imgs=9`，图片未读取）
  - **固件特性与刷机注意项**：
    - 底层移植自Pixel Tablet最新安卓17 Beta（DP11.261016.003）。
    - **刷机特性与Bug避坑**：固件为9008线刷包，已通过AVB签名，支持**免解BL锁**直接刷入；但由于联想国际版9008底层缺陷，刷入后**必须强制格式化Data分区**，否则系统存储会被锁死识别为64GB。
    - **硬件适配清单**：支持6GHz WiFi与热点；蓝牙支持硬件卸载及Aptx/LDAC协议；激发亮度调整为780nit；修复PixelTablet默认反向旋转；支持120Hz/165Hz切换；支持原厂双马达震动波形、双Type-C满速接口、旁路充电及微距相机移植。
    - **扩展功能**：集成Tango 32位兼容层支持老旧应用，内置Paranoid人脸解锁，全局Pixel伪装配合本地RKP通过Play认证，SELinux维持Strict/Enforcing。
    - **开源仓库**：`https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003`。

- **Redmi annibale NeoHyper OS4 主题自动还原默认排障与规避**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/74964016)（`imgs=0`）
  - **设备环境**：Redmi annibale，ROM为 NeoHyper BloomFlow 2.0.7.5 / OS4.0.5.0.XPKCNXM。
  - **故障机理分析**：
    - 现象：主题商店下载的主题应用20~60分钟后自动回滚为默认主题，或软重启后即刻复原。
    - 根因：排查日志与存储路径 `/storage/emulated/0/Android/data/com.android.thememanager/files/MIUI/theme/.data/content/theme/*.mrc`，发现下载的主题主包恒为 0 字节，导致整包校验报 `DRM_ERROR_ASSET_NOT_MATCH`；主题商店进程启动自检触发 `force_restore` 清空主题。
  - **可执行规避步骤**（需Root）：
    1. 备份桌面数据库与主题配置：
       `cp /data/user_de/0/com.miui.home/databases/launcher4x7.db* /sdcard/theme_backup/launcher_db/`
       `cp /data/data/com.android.thememanager/shared_prefs/selected_theme_components.xml /sdcard/theme_backup/state/`
    2. 主题商店应用主题后，立即执行冻结命令阻止校验守护进程启动：
       `su -c "am force-stop com.android.thememanager"`
       `su -c "pm disable-user --user 0 com.android.thememanager"`
    3. 重启验证进程不存在，主题可长效保留；切忌在系统运行中直接替换 `/data/system/theme/icons`，否则会引发SIGBUS导致系统软重启。

- **Pixel 9系列刷入 Infinity X Android 17 类原生 ROM 全流程实操**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/74963531)（`imgs=9`，图片未读取）
  - **特性与避坑注意**：
    - **特性**：自带灵动岛（进度环支持显示音乐播放与下载进度）、状态栏与QS面板自定义、免模块解锁国内5G与VoLTE通话。
    - **已知问题**：初次开机相机若闪退需通过Play商店更新至最新版；Infinity官方ROM的内置Hook可能触发腾讯《王者荣耀》安全封号。
  - **完整Fastboot分区刷入命令流**：
    1. 进入Bootloader模式：`.\adb reboot bootloader`
    2. 依次单刷镜像分区：
       `.\fastboot flash boot boot.img`
       `.\fastboot flash dtbo dtbo.img`
       `.\fastboot flash vendor_kernel_boot vendor_kernel_boot.img`
       `.\fastboot flash vendor_boot vendor_boot.img`
    3. 进入Recovery（`.\fastboot reboot recovery`）执行 `Format data / factory reset`。
    4. 侧载刷机：选择 `Apply from ADB`，在终端执行 `.\adb sideload <ROM_NAME>.zip`（进度停在47%时手机端弹出确认，点击Yes后重启）。
    5. Root与调度：使用WorkBuddy从ROM包 `payload.bin` 提取 `boot.img`，修补后刷入KernelSU；安装爱玩机工具箱救砖模块及针对Tensor G4调校的 Pixel 9 Series Supercharger 模块。

- **红米K50至尊版 / K60 骁龙8+ 官改澎湃OS4驱动移植与降压超频**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/74958490)（`imgs=3`，图片未读取）
  - **测试对象**：Redmi K50 Ultra（江南官改OS4.0.6）、Redmi K60（江南官改OS4.0.5），使用KernelSU v3.3.1。
  - **关键前置条件**：卡刷或线刷升级前必须提前提取并修补新系统的 `boot.img`，否则重启必掉Root且vendor替换的驱动失效。
  - **GPU驱动移植与震动修复**：
    - 采用MT管理器与MIO-KITCHEN在未解锁分区状态下直接向 `/system/vendor/` 移植替换 GPU 驱动 762.36 增强版 v1.6；
    - **震动失效排查**：若移植第一版驱动后震动不可用，提取第一版vendor后覆盖复制第二版修复驱动即可恢复；修改后执行GPU缓存清理脚本。
  - **GPU降压与编译调度**：
    - 刷入GPU降压超频表，在Scene中确认最高频率达到905MHz/910MHz；
    - 在Scene杂项中开启 `dex2oat` 全量编译（选择 `everything` 模式）；
    - 关闭系统高级材质以适应骁龙8+老旧芯片；在KsuWebUI中确认驱动版本为V0762.36，设置压缩依赖为25，水位线为150；ZenCore后台压制放行微信与QQ。

- **摩托罗拉手机（MyUX / Hello UI）全面去广告、系统优化与手势设置**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/74950603)（`imgs=0`）
  - **测试机型与操作步骤**：
    - **系统基础优化**：连续点击版本号开启开发者选项；关闭虚拟内存扩展（`运行内存 → 内存扩展 → 关闭`以提升闪存读写流程度）；关闭开发者选项中的“系统自动更新”；屏幕刷新率设为高/120Hz自适应；位置权限改为“仅使用时允许”，关闭WLAN与蓝牙后台扫描。
    - **系统广告位彻底关停路径**：
      - 应用安装器：`应用安装设置`内关闭应用推荐、安装后推荐与安全检测推广全部3个开关；
      - 负一屏：长按桌面空白进入`桌面设置`关闭“我的屏幕”，或在应用管理中搜索“我的屏幕”点击右上角“卸载更新”恢复无资讯纯净版；
      - 应用商店/浏览器：关闭个性化推荐、自动下载安装包与推送；首页设为空白页；
      - 隐私与系统：删除广告ID，关闭用户体验计划。
    - **音效与手势实操**：杜比全景声音效自定义开启“低音增强”与“音量平衡”；开启摇一摇开相机、甩两下开手电、翻转勿扰手势；AI百宝箱仅保留“MAXE系统引擎”，其余停用以释放后台内存。

- **小米四姑娘山超级壁纸逆向分析、功耗实测与中兴Axon 20冻结定位**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/74949978)（`imgs=5`，图片未读取）
  - **逆向工程与架构分析**：
    - **资源结构**：从小米15（HyperOS 3.307）提取，APK体积66.9MB（2MB dex + 33MB native so + 47.3MB 场景资产），纯 arm64-v8a。
    - **渲染引擎**：静态链接 Google Filament PBR 引擎，通过 filamat 预编译 mobilevulkan / mobileopengl 双后端着色器，优先 Vulkan。场景使用小米自研 ECS 引擎，`CameraCtrl.lua`（1030行）承载完整交互逻辑。
    - **显存与ASTC压缩**：模型为 fmesh，纹理全为 ASTC 压缩（天空约 4x4/2bpp、云层约 5x4/1.25bpp、山体约 6x5/0.85bpp），常驻显存不解压。当前时段常驻显存约5.9MB，时段切换淡入淡出峰值约11.8MB。
    - **动态帧率控制**：AOD/锁屏/桌面过渡前1.2秒跑满60/90/120Hz；桌面滑动强制限制30fps；静置后降至20fps@60Hz或30/40fps（调用 `setRenderFrameInterval(3)`），显卡vsync负载仅常规1/6。
  - **模拟器与实机移植实验**：
    - **MuMu模拟器失败根因**：DX转译层缺乏ASTC硬件解码；Houdini ARM->x86转译层在大块纹理 memcpy 路径存在内存拷贝越界Bug，导致引擎崩溃后WMS回落默认壁纸。
    - **ZTE Axon 20（骁龙765G，安卓10）静止Bug排查**：Adreno 620 GLES 3.2跑通并成功渲染，但绑定4~6秒后进程被中兴ROM直接投入 Freezer cgroup（`6:freezer:/uid_10319`），导致schedstat归零、SurfaceFlinger停止出帧变成静止画面。
    - **能耗实测**：解锁过渡动画带来瞬时额外功耗约1.08W，持续1.5~2秒，单次解锁增加能耗约1.9J（0.52mWh），反证超级壁纸不是MIUI系统卡顿的根本原因。

---

- **索尼 Xperia 1 III (pdx215) 官方 Android 11 移植 SmartisanOS**
  - [酷安原帖](https://www.coolapk.com/feed/74939041)（`imgs=9` 未读）
  - 底包与环境：索尼 Xperia 1 III（`XQ-BC72` / `pdx215`），基于已解锁 BL 的官方 Android 11 底包（`61.0.A.23.20`，构建日期 2026-11-08）。
  - 分区替换方案：包内仅含 3.56GB `system.img` 与自签 `vbmeta.img`，只写入 super 逻辑分区 system，保留原厂 vendor、boot 和基带。相机、侧边指纹、通话与 120Hz 刷新率全部正常。
  - 核心调校优化：
    1. 亮度曲线重做：0~100% 完整映射背光量程，单调无台阶跳动；
    2. 强光激发：环境光 >10000 lux 自动激发 +40% 峰值，<6000 lux 回落，过热或电池 >42℃ 强制退出；
    3. 快捷开关：打入单字节补丁解除 3 列限制，扩展为 4×5 共 20 个开关一屏排布；
    4. Xperia 设置集成：CPU/GPU 顺控调度（均衡/性能/省电三档）、80% 停充保养电池，实时监控健康度、循环、UFS 寿命、三簇频率与温控负载。
  - fastbootd 刷入四条标准命令：
    ```sh
    adb reboot fastboot
    fastboot flash system images/system.img
    fastboot --disable-verity --disable-verification flash vbmeta images/vbmeta.img
    fastboot reboot
    ```
    无需 Magisk 与 Root，Windows 首次需装 `tools\driver\install_fastbootd.bat` 驱动。

- **LG V60 ThinQ 美版 LineageOS 23.2 深度适配实测**
  - [酷安原帖](https://www.coolapk.com/feed/74911731)（`imgs=0`）
  - 运行环境：LG V60 T-Mobile 美版，刷入 LineageOS 23.2（20261018 Nightly），Magisk Root + VoLTE&IMS 修复模块，中国联通单卡（4G频段）。
  - 硬件适配状态：VoLTE 通话、Wi-Fi、蓝牙、Hi-Fi Quad DAC、屏下指纹、线性马达正常工作，未见恶性重启崩溃；纯净系统后台极轻快，整晚待机耗电个位数。
  - 功能差异与缺失：
    1. 通话降噪佳，但原生录音音质稍干且缺少指定号码自动录音；原生录音机缺少原厂多麦克风指向调节与无损录音；来电等待正常但拨号器无开关；
    2. 联通卡刷入 VoLTE/VoWiFi 模块后收发短信彩信正常，移动据报需额外插件；电信/广电未测；
    3. 原生相机简陋，需搭配魔改版谷歌相机（Gcam），原厂专业手动录像与 ToF/红外硬件未调用；
    4. 本地化缺少农历与放假安排；`*#*#4636#*#*` 可见双卡选项。

- **小米平板 7 Pro (`muyu`) 6 款第三方类原生 ROM 汇总**
  - [酷安原帖](https://www.coolapk.com/feed/74897110)（`imgs=9` 未读）
  - 前置条件：设备已解锁 Bootloader。
  - 收录 ROM 体系与特性：
    - PixelOS 16（Android 16 / AOSP，Material Design 3 Expressive 界面与完整 GMS 套件）；
    - crDroid 12.7（Android 16 QPR2，LineageOS 底层，内置 crDroid Settings 深度定制、Game Space、PIF 完整性欺骗与无限 Google Photos 备份）；
    - Project Mica（MicaOS BP4A-OSS-muyu，2025-12-21 构建）；
    - LunarisAOSP 3.7（LineageOS 稳定底层，集成 Pixel 特性与性能调度）；
    - AxionAOSP 2.3 Telos（Android 16 QPR1，轻量性能导向，内置 GameSpoofing、PIF、Tricky Store、Spiteful 内核与杜比全景声 UI）；
    - AviumUI 16.2.1（LineageOS 底层轻量本地化，万象时钟锁屏框架、状态栏歌词、NFC 无感支付、手势小窗与应用分身，经作者修复无法开机问题，附 123pan 下载链接，提取码 `L3SG`）。
  - 资源：附各项目 SourceForge 镜像直链与原帖链接。

- **小米 14 Pro (`shennong`) 澎湃底包刷入 RestlessOS (GrapheneOS GSI) 避坑**
  - [酷安原帖](https://www.coolapk.com/feed/74887979)（`imgs=5` 未读）
  - 底包与镜像：小米 14 Pro（澎湃 `OS3.0.306.0.WNBCNXM`，Android 16），镜像为 `RestlessOS-arm64-ab-16.0.0-202606230923.img`。
  - DSU 兼容性验证：蓝牙、WiFi 正常，相机 0.6x~10x 正常；通话、移动数据、指纹不可用。
  - 刷机步骤与参数避坑：
    1. 解锁 BL，从原厂线刷包提取 `images\vbmeta.img`，与解压后的 system 镜像放至 ADB 目录；
    2. 进入 fastbootd：`adb reboot fastboot`，确认 `fastboot getvar is-userspace` 显示 `yes`，查询槽位 `fastboot getvar current-slot`（以 a 槽为例）；
    3. 刷入 vbmeta：必须【只加 `--disable-verity`，绝不能加 `--disable-verification`】以防不开机：
       `fastboot --slot=a --disable-verity flash vbmeta vbmeta.img`
    4. 刷写 system：`fastboot --slot=a flash system RestlessOS-arm64-ab-16.0.0-202606230923.img`；
    5. 扩容空间不足报错处理：若报 `Resizing 'system_a' FAILED (remote: 'Not enough space to resize partition')`，执行删除同槽位 product 逻辑分区命令：
       `fastboot delete-logical-partition product_a`
       然后再重新执行写入命令；
    6. 双清开机：`fastboot -w`，`fastboot reboot`。
