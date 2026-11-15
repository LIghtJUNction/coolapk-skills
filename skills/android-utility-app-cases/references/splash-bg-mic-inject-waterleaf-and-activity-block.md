# 启动页深色适配、麦克风注入、水面落叶壁纸与 ActivityController 禁启

整理自酷安社区独立开发者发布记录（未读配图与蓝奏云 APK，未做安全审计与真机测试）。

## 1. 分应用设置 Splash 启动页背景色（Root）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74039865)（`imgs=4`，`https://wwawn.lanzouw.com/b0wa2igsh`）
- **机制与对比**：需 Root 权限，按应用单独覆盖全屏 Splash 启动动画背景色（解决高德地图等忽略系统/应用内深色模式的白色闪光）；作者对比指出系统自带「强制深色」虽免权限但不能分应用设置且易导致拼多多等 UI 色彩异常。

## 2. 麦克风注入器（Android 12+ / ADB·Shizuku·Root）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/73542372)（`imgs=3`，`https://wwawn.lanzouw.com/b0wa0euda`）
- **行为**：通过 ADB/Shizuku/Root 启动服务，生效期间将麦克风收音全量替换为指定音频（队友仅能听到音频），播放结束后自动恢复。

## 3. `禁止启动`（`v3` / `v5`，ADB·Shizuku）

- **来源**：[v5](https://www.coolapk.com/feed/73406261)（`imgs=1`）、[v3](https://www.coolapk.com/feed/69051752)（`imgs=1`，`https://wwawn.lanzouw.com/b0w88awof`）
- **原理与边界**：使用 `ActivityController` 拦截目标 Activity 启动（而非 `pm disable`），因此禁用主 Activity 后桌面图标不会消失，但无法拦截服务/广播且重启后需重新激活；`v5` 修复了 Android 16 上激活后状态不立即刷新的问题，`v3` 新增按包名/应用名/Activity 名搜索。

## 4. 轻量化复刻工具（水面落叶壁纸 & 侧滑返回箭头）

- **水面落叶动态壁纸**：[酷安原帖](https://www.coolapk.com/feed/73457226)（`imgs=2`，`https://wwawn.lanzouw.com/iuJYP44w0zof`，550KB，Android 5.0+）— 复刻 Android 4 经典动态壁纸，支持切换 GLES3/GLES2 后端及 20fps 至屏幕最高帧率。
- **侧滑返回手势复刻**：[酷安原帖](https://www.coolapk.com/feed/73381102)（`imgs=3`，`https://wwawn.lanzouw.com/i40Bl442zz0b`，45KB，Android 5.1+，需无障碍）— 仅提供原生 Android 10–16 侧滑返回箭头外观与动效。
