# 平铺窗口管理器、WearOS 圆屏中转、仿澎湃相册与 DroidExtend USB 扩展屏

整理自酷安社区开发者发布记录（未读图片与评论，未做实机或源码安全审计）。

## 1. Android Root 平铺窗口管理器（`abaa wm`）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74665112)（`imgs=4`）
- **原理与交互**：需 Root 权限，为每个应用创建 Android 虚拟屏幕并由窗口管理器接管绘制与焦点调度；支持任务栏左右滑切换工作区、上下滑在最小化/窗口化/最大化间切换、长按标题拖动交换窗口位置（AI 全量生成代码，作者提示定位为实验玩具）。

## 2. `Android-roundscreen-adapter`（Watch Control）：免 Root WearOS 圆屏远程点选

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74607289)（`imgs=0`，`https://github.com/nowionlyask/Android-roundscreen-adapter`）
- **机制**：针对 OPPO Watch X 等圆形 WearOS 手表无需解锁 BL/Root，在同局域网电脑（Python 3.9+ + ADB）预览手表截图并选定坐标保存到中转服务，手表端悬浮窗调用坐标触发点击；演示了 Android 悬浮窗、ADB 调试、WebView、HMAC 签名与自签名证书信任。

## 3. 仿澎湃 4 悬浮底栏相册（WorkBuddy 开发早期版本）

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74417288)（`imgs=7`）
- **兼容性**：HyperOS 1–3 支持 MIUEX 悬浮底栏与渐进模糊；Android 13 以下与华为鸿蒙 3/4 仅显示半透明材质；支持文件夹名模糊搜图与跳转小米系统图片编辑。

## 4. `DroidExtend`：USB 数据线 + ADB 将安卓设备变为 Windows 扩展屏

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74351014)（`imgs=4`，`https://github.com/Blank2007/DroidExtend`）
- **架构**：Windows 端基于 Python + `ffmpeg`（NVENC 硬编）与虚拟显示驱动（首次安装需 UAC，日志显示 `device ROOT\DISPLAY\0001 status=OK`），安卓端开启 USB 调试与 USB 安装并运行随包 `dist/DroidExtend.apk`，通过纯 USB ADB 传输画面与触摸输入（支持扩展桌面与镜像模式）。
