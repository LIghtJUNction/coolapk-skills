# Neo10 Pro 双模调度、AAC-ELD 编码、NativeComplements、焦点笔 Plus 与通话摘要模块

> 整理自酷安公开动态正文；图片、评论及外链未逐一核验，实际操作请以对应硬件版本与上游仓库为准。

- **iQOO Neo10 Pro (天玑 9400) KernelSU 双模调度 WebUI 模块**
  - [酷安原帖](https://www.coolapk.com/feed/75041893)（`imgs=3`，WebUI 截图未读）
  - **设备与环境**：iQOO Neo10 Pro（天玑 9400），基于 Scene 调度规则由 DeepSeek 生成的 KernelSU 模块。
  - **核心机制**：
    - *实时监控 WebUI*：基于 KSU WebUI 唤起纯白毛玻璃控制台，每 2 秒刷新 CPU 实时频率、电池温度、充电电流与可用内存（>40°C 变黄，>43°C 变红）。
    - *双模调度*：日常模式将超大核（CPU 7）最高主频硬锁在 0.8GHz，配合 30%~100% 无级滑块保存限频节点；游戏模式每 8 秒轮询检测指定游戏包名，启动瞬间唤醒超大核并拉高频率底限至 70%，底层解禁联发科 FPSGO 限制，并启用 TCP BBR 网络拥塞控制算法；退出游戏 8 秒后自动回退日常模式。
    - *内存清理*：控制台提供一键白名单内存深度清理按键。

- **小米 17 Pro Max (HyperOS 3/4) AirPods AAC-ELD 蓝牙音频移植模块**
  - [酷安原帖](https://www.coolapk.com/feed/75040014)（`imgs=1`，配图未读）
  - **开源项目**：`https://github.com/wasdwasd0105/xiaomi-airpods-aac-eld`
  - **功能与底层互斥**：将苹果专用的 AAC-ELD 蓝牙编码器做成 HyperOS 模块，实测小米 17 Pro Max + HyperOS 3/4 可用；声场分离度与高频表现显著提升。明确指明技术限制：由于该方案采用纯 CPU 软件编码，无法走高通 DSP 调音直出通道，因此开启后无法使用小米系统级风味空间音频特效。

- **类原生增强模块 NativeComplements 开源项目**
  - [酷安原帖](https://www.coolapk.com/feed/75038858)（`imgs=9`，界面图未读）
  - **开源与下载**：开源地址 `https://github.com/AIXINJUELUOAI/NativeComplements`；蓝奏云密码 `1sf2`。
  - **特性与避坑**：
    - 基于 AOSP freeform 移植魅族 Flyme 自由小窗功能；
    - 弹幕通知：拦截系统悬浮窗并将通知转换为弹幕飘字；
    - 桌面图标：适配 Pixel Launcher 图标裁剪与分层动画（素材来源于 Perfect-Icons-Completion-Project）；
    - 限制应用读取加速度传感器以屏蔽摇一摇开屏广告；
    - 横屏控制中心优化与状态栏间距微调；
    - 提供来电拦截功能，但明确警告与“智能 5G”冲突会导致 SIM 卡无服务掉网，需通过开关飞行模式重置恢复。

- **焦点笔 Plus v2.1.0 开源 LSPosed 模块 (小米平板 6 Pro)**
  - [酷安原帖](https://www.coolapk.com/feed/75036238)（`imgs=0`）
  - **开源与下载**：Apache-2.0 开源，`https://github.com/BaerYiTo/XiaomiSmartPenPlus/releases/tag/v2.1.0`，蓝奏云密码 `fe06`。
  - **功能实现**：针对小米平板 6 Pro 刷入 ColorOS 17 系统，将小米灵感触控笔二代伪装为 OPPO 原厂手写笔；系统设置可正常显示连接状态、电量、吸附/拿起 Toast 提示；支持拿起唤醒屏幕、吸附唤醒屏幕（独立开关），保留按键映射与左右滑翻页。
  - **安装注意**：包名变更为 `com.inkdye.xiaomipen`（需卸载旧版）；LSPosed 作用域中，ColorOS 系统级功能必须额外勾选 `com.oplus.ipemanager` 并授予 Root 权限。

- **SummaryForAll v1.0：ColorOS 17 陌生号码自动生成通话摘要 LSPosed 模块**
  - [酷安原帖](https://www.coolapk.com/feed/75032101)（`imgs=2`，截图未读，网盘提取码 `GieY`）
  - **机型与校验**：ColorOS 17 / Android 17（实测一加 Ace 5 竞速版）。APK 文件大小 20,748 字节，MD5 `13be51835d5a2f5b348d31c720037d34`；源码大小 38,269 字节，MD5 `ad8ca6e0f9a834da22dd9deb4bbcdc70`。
  - **Hook 点与机制**：
    - Hook `com.android.incallui.util.o.m()` 返回值，覆写 auto 标志为 true；
    - Hook `ListView.addHeaderView`，在设置页注入总开关（或执行 `settings put secure lonix_ai_summary_all_strangers 1`）；
    - 进程生效坑点：宿主为 `com.android.phone`（radio 常驻进程），`am force-stop` 无法杀死，安装或更新后必须在 Root 终端执行 `kill -9 $(pidof com.android.phone)` 强制重启生效；
    - 限制：若系统未开 AI 通话摘要总开关或开启了 Breeno「AI 自动接听」，更前层判定返回 false，模块无法介入。
