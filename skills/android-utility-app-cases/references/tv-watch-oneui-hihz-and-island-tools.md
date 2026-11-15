# 电视/手表管理（GenTV / 通用手表管理器）、OneUI 9 跳转限制、HiHz 与 LiquidIsland BETA7

本参考整理酷安社区中针对 Android TV 管理（GenTV）、PC 端手表管理（通用手表管理器 20260920）、海外版三星 OneUI 9 摇一摇跳转限制、vivo HiHz 分应用刷新率，以及 LiquidIsland 2.5.0b BETA7 的实测与功能边界。

---

## 1. GenTV v3.7.0：免电脑/免 Root 管理 Android TV

- **来源**: https://www.coolapk.com/feed/75012221
- **主要功能**: 通过 Android 手机经同一 Wi-Fi 无线调试连接 Android TV / Google TV，安装无广告桌面（电视安装包 `< 1MB`）、清理预装应用、遥控与键盘输入，并支持 Dock、应用网格及 HDMI/AV/Live TV 信号源列表（具体切换需实机验证）。
- **前置步骤**: 电视端在“关于”页面连续点击 7 次版本号开启开发者选项与调试（Android 11+ 部分机型需无线调试配对码），授权配对后先读取型号与包列表再执行操作。

---

## 2. PC 端通用手表管理器（版本 `20260920`）

- **来源**: https://www.coolapk.com/feed/74879894
- **适用范围与连接方式**: 面向 OPPO Watch 等智能手表，支持 USB 直连、无线 ADB、Wear OS 配对码、蓝牙 ADB、局域网 mDNS 扫描（`5555`/`8710` 端口探测）及华为 HDC，内置 `adb`/`hdc` 与 `scrcpy`。
- **核心能力**: 电脑投屏遥控与按键映射、独立/蓝牙模式一键保活、消息通道逐项诊断（通知渠道/Doze 白名单/待机桶/后台权限/网络连通/欢太推送）、DPI/分辨率/权限批量配置、冻结卸载与 `ilink` 授权导入导出。

---

## 3. 海外版三星 OneUI 9（Android 17）Shizuku 摇一摇跳转限制

- **来源**: https://www.coolapk.com/feed/75010114
- **问题背景**: 海外版三星无国行跳转控制功能；升级 Android 17 / OneUI 9 后原 `App Ops` 失效且 `App Ops Next` 无法识别加速度传感器权限。
- **方案**: 针对 OneUI 9 开发的轻量控制器，需 **Shizuku** 权限，提供两种限制模式用于拦截淘宝等应用利用加速度传感器触发的摇一摇跳转（未保证其他机型/系统兼容性）。

---

## 4. vivo HiHz（按应用固定刷新率，汉化优化版）

- **来源**: https://www.coolapk.com/feed/74889289
- **前置与权限**: 基于 GitHub `jp.androplus` 原版全量汉化并加入深色主题；免 Root/ADB，需授权「使用情况访问」与「无障碍」（仅读取前台窗口所属应用包名）以及 vivo 自启动权限，支持 Android 11+（实测 `vivo V2507A` / Android 15）。
- **已知帧率特性**: 若应用自身限制 `120fps`，在 HiHz 中强行固定 `165Hz` 时实际仅跑 `82.5Hz`（半频）；此类应用建议单独设档或设为「自动」。

---

## 5. LiquidIsland 2.5.0b BETA7：内置无线 ADB 配对与挖孔避让

- **来源**: https://www.coolapk.com/feed/74937314
- **关键更新**:
  - **内置无线 ADB 配对**: 不再只依赖 Shizuku，可在岛内界面直接输入系统「无线调试 → 使用配对码配对设备」的 6 位配对码完成授权，支撑截图、取色等 shell 能力。
  - **挖孔避让与水滴屏模式**: 自动检测前置挖孔位置并支持 `0.5dp` 步进手动微调，新增水滴屏贴合模式与状态栏电池内电量数字、实时 GPU 等选项。
