# scrcpy 5.0.1：硬件解码与平台使用条件

## 工作方式与解码
帖子介绍 scrcpy 通过 ADB 将 Android 画面传到电脑，手机端无需安装应用，也无需 root。帖子称 scrcpy 5.0 默认改用硬件视频解码，利用 Intel QSV、NVIDIA NVDEC、AMD VCN 或 Apple VideoToolbox；引用的官方数据称 CPU 占用约降至原来的 1/10。此性能数字是原帖转述，输入未附测试条件或测量结果。

指定解码器可用 `--hwdec`：Windows 为 `d3d11va`，Linux 为 `vaapi`，macOS 为 `videotoolbox`。强制软件解码使用 `--hwdec=disabled`。帖子称硬件解码不可用时会自动回退；Linux 使用 VA-API 还需安装显卡驱动，Debian/Ubuntu 可安装 `va-driver-all`。

## 5.0 与 5.0.1 变化
原帖报告 5.0 重写视频缓冲、增加 Windows ARM64 原生包、改进音频捕获以覆盖此前可能漏掉的游戏声音、支持 OTG 模式通过 `ANDROID_SERIAL` 指定设备，并修复部分 UHID 键盘布局掉键及 AltGr 转发问题。其列出的依赖版本为 adb 37.0.1、FFmpeg 9.0.2、SDL 3.4.18。

原帖报告 5.0.1 修复 Windows 上 H.265 硬解失效、多显卡 Linux 环境下解码与渲染落在不同显卡的问题，并增加 NV12 色度平面格式支持。版本日期及修复情况按原帖记载，未在输入中独立验证。

## 系统要求与使用条件
- Android 5.0 及以上；音频转发要求 Android 11 及以上；不支持 iPhone。
- 需要启用 USB 调试。原帖另称小米、红米需启用“USB 调试（安全设置）”才能控制键鼠。
- 包体积为原帖约数：Windows 解压后约 10 MB，含 win32、win64、winarm64；macOS 约 12 MB，含 Intel 与 Apple Silicon；Linux 约 17 MB。包内含 `scrcpy-server`，原帖称正常使用无需手动处理。
- 原帖称硬解收益取决于电脑的视频解码单元能力，较旧硬件提升有限；延迟“官方写 35 到 70 毫秒”，但未附官方页面或测量条件。
- 原帖称 scrcpy 是命令行工具，没有图形界面；QtScrcpy 是第三方图形外壳。与原帖提及的 Vysor、AirDroid Cast 相比，后两者需在手机安装应用，且其价格信息仅是原帖所述。

## 资源
- 原帖提供的跨平台打包下载分享页：[https://pan.quark.cn/s/393ce6048ebe](https://pan.quark.cn/s/393ce6048ebe)。输入未提供提取码，也未提供该分享页内的具体文件清单或校验结果。

## 来源与证据边界
来源：酷安帖子 [74424301](https://www.coolapk.com/feed/74424301)。输入有两张图片但未提供图片内容；本文只整理可读正文。原帖称“查看链接”处的分享页已按输入链接上下文关联。评论未提供，评论范围不完整。
