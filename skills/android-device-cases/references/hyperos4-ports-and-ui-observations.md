# HyperOS 4 移植案例、Dash-Flash 保留数据刷机与界面观测

整理自酷安社区开发者发布与体验记录（未读配图与评论，未做实机刷入验证）。

## 1. 红米 Turbo 5 Max（`Dash`）移植 HyperOS 4 保留数据刷机指南

- **来源**：[酷安原帖](https://www.coolapk.com/feed/73965444)（`imgs=9`）
- **要点**：从红米 K90 Max 移植，目前使用配套 Windows 便携应用 `Dash-Flash`（路径不可含中文/空格，电脑缓存盘需预留约 24 GiB）；主系统开启 USB 调试后先备份启动镜像与设备分区（需设备已有 Root 并授予 shell Root 权限，无 Root 亦可按流程刷入但无法备份/恢复原 Root 镜像）。

## 2. 小米 MIX Flip 1 移植 MIX Flip 2 HyperOS 4 Beta（`OS4.0.0.10`）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/73911817)（`imgs=9`）
- **改动摘要**：基于 MIX Flip 2 Beta `OS4.0.0.10.XOHCNXM.D01` 与 MIX Flip 1 底包 `OS3.0.306.0.WNICNXM.C09` 制作，采用 EROFS 打包、替换 MIX Flip 1 原版内外屏壁纸、GPU 频率调校为 `152–918 MHz` 并降压、内置 TWRP 与 KernelSU LKM（需覆盖安装一次 APK）、Data 伪加密及修复开机亮度/震动。

## 3. `HyperOS-stone-4.0.10.0.XPACNXM`（Android 17）移植更新记录

- **来源**：[酷安原帖](https://www.coolapk.com/feed/73253470)（`imgs=0`）
- **修复与已知问题**：修复息屏显示、SKU 注册表识别、桌面崩溃与人脸解锁异常；说明 Rust+Flutter 桌面受 GPU 驱动限制实时模糊较吃力，小米账号暂无法登录、暂无法导入 MTZ 主题，若锁屏样式选择闪退可在「设置 → 显示」关闭「高级材质」。

## 4. ColorOS 17 焕新凝光视效与小米 Pad 8 HyperOS 4 Beta 界面观察

- **ColorOS 17 vs iOS 27 设计对比**：[酷安原帖](https://www.coolapk.com/feed/73837524)（`imgs=9`）— 记录 ColorOS 17 第一方应用统一“焕新凝光视效”（底栏高光、渐变阴影与模糊嵌套）及 iOS 27 减弱玻璃实时高光渲染后的取舍。
- **小米 Pad 8 HyperOS 4 Beta**：[酷安原帖](https://www.coolapk.com/feed/73265674)（`imgs=9`）— 记录并行动画、桌面堆叠小部件、文件管理/时钟/相册新版柔性框架（顶置可放大 Tab 栏）、侧边任务栏快捷小窗与锁屏时间无极调节。
