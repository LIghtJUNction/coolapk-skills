# MIUI 显示、相机与 Swap 历史案例

以下为特定版本的社区经验，链接、文件、操作及效果未经独立验证；图片与完整评论未读。修改系统资源或内核配置前核对设备版本并准备恢复方式。

## 屏幕缺口和手势

[22939938](https://www.coolapk.com/feed/22939938) 提供 Redmi 10X 5G、MIUI 12.0.5 的隐藏水滴补丁，作者称 Pro 和其他系统版本会被安装检查阻止；指出横屏翻转圆角异常及部分录像/视频壁纸场景掉帧。[下载](https://omarea.lanzoui.com/iaZaPiebtmj)，未见公开提取码。

[25293224](https://www.coolapk.com/feed/25293224) 的 MIUI 12 第三方手势案例先切导航键模式，通过主题资源 `navigation_bar_height`、`navigation_bar_height_landscape`、`navigation_bar_width`、`system_bar_height` 设零，再调整 `com.miui.home` 的 `search_bar_margin_bottom_navigation` 和 `search_bar_margin_bottom`；示例为 20dp。系统和桌面版本可能使用不同资源，改错可能影响导航。

[23509251](https://www.coolapk.com/feed/23509251) 记录小米 10 系列 Android 11、MIUI 12.2.1（10 Pro）的全屏模块：[下载](https://omarea.lanzoui.com/iMGaaja6eze)。方案通过 framework 的 cutout 布尔值或生效 overlay 的 cutout 形状改变应用所见缺口，并调整状态栏左右和顶部间距；原帖 177px、90px、24px、27px 是该设备的例子，不能用于其他机型。

## 内存与相机

[23214246](https://www.coolapk.com/feed/23214246) 的 Scene Swap 附加模块 2.7.2 处理启动时 ZRAM/Swapfile 与 VM/LMK 参数，作者称相比 2.6.0 调整容量和回收阈值；内存和 Swap 被占满时切换可能卡顿。版本 [2.7.2](https://omarea.lanzoui.com/iWsaLpqzsqf)、[2.6.0](https://omarea.lanzoui.com/istLwpivkpe)、[相关说明](https://www.coolapk.com/feed/22581283)；未见公开提取码，关联说明未读。帖子将 2.7.2 日期写作 2012-06-07，与 2.6.0 日期顺序冲突，日期待核实。

[25817270](https://www.coolapk.com/feed/25817270) 以小米 9 的旧版 MIUI/微信为例，描述相机辅助镜头白名单及 Hook `Camera.getNumberOfCameras()`、`Camera.open(cameraId)` 的实验；返回的 camera ID 数量不一定等于物理镜头数量，须逐个核验。扫码界面类名、白名单属性与行为均高度依赖版本，不保证当前可用。

## 图形性能实验

[27466964](https://www.coolapk.com/feed/27466964) 使用小米 11 Pro 骁龙 888 对 GPU 各频点降压测试；作者称功耗为整机值、逐频点测试，电压数据位于未读图片。不能据此复用电压或判定其他芯片体质。

[27675744](https://www.coolapk.com/feed/27675744) 比较降压后的 Mi 9/855、Mi 10 Pro/865、Mi 11 Pro/888，在 GFXBench 曼哈顿 3.1 离屏、1080p/60Hz、最低亮度等条件下记录 73fps/4.5W、99fps/5.1W、90fps/5.3W（888 540MHz）、113fps/6.8W（888 738MHz）；功耗为整机、设备与电压不同，空载扣减是作者假设而非实测。不能直接当作 GPU 芯片能效排名。

[26473463](https://www.coolapk.com/feed/26473463) 说明 Scene 4 的 `/data/powercfg.sh` 接收 `init`、`powersave`、`balance`、`performance`、`fast`，严格模式下前台切换可提供 `top_app`；手动切换或 init 等场景可能为空。对应 Scene 5 的扩展另见本目录 [调度案例](scene-scheduling-touch-performance-cases.md)。
