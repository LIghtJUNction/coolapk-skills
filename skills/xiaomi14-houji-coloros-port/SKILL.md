---
name: xiaomi14-houji-coloros-port
description: 整理小米14 houji的ColorOS16.1移植版下载、纯净刷入说明、已知问题与第二版改动。
---

# 小米 14（houji）：ColorOS 16.1 移植版

帖子说明此 ROM 仅适用于纯净刷入（clean flash），不是 GSI，SELinux 为 Enforcing。作者未提供安装包版本号或校验信息。

## 刷入说明

原帖描述的步骤只有：解压压缩包、重启手机进入 Fastboot、运行解压所得的 `.bat` 文件。帖子没有给出 BAT 文件名、脚本内容、包版本校验方式或更详细的设备操作流程，因此不能据此补造一键刷机脚本或 fastboot 命令。

## 作者列出的已知问题

- eSIM（作者称后续会加入）
- 相机夜景模式
- 内存占用较高
- LTPO 动态刷新率
- 重启后提示密码错误
- 屏幕唤醒时刷新率为 60 Hz
- 系统应用打开有延迟

## 第二版修复与增强

作者称第二版修复或加入了以下内容：

- 90W 旁路充电（Bypass Charging；这是作者的功能声明，本文未实机验证）
- 阅读模式（护眼模式）
- O-Haptic 演示视频
- HBM（高亮度模式）
- 静音与振动模式选择
- 色彩偏淡问题
- 20 倍变焦后相机闪退
- 颜色反转问题
- 自动亮度调节

作者还列出这些修改或增强：手电筒渐亮效果；内置“刻晴”和“胡桃”指纹动画；全屏 AOD 及平滑过渡动画；iOS 风格 Emoji；圈选即搜（Circle To Search）；国际版 ROM 通知样式。

## 下载与恢复资料

原始 Google Drive href 为 `https://drive.google.com/file/d/1EyLxvy0T8w7wZJnTuhxCk00Y9ng8t1qW/view?usp=drivesdk)`。下面的规范化链接仅舍去 `usp` 查询，保持确切文件 ID 与 `/view` 路径；未核验权限或实际下载：

<https://drive.google.com/file/d/1EyLxvy0T8w7wZJnTuhxCk00Y9ng8t1qW/view>

原帖还引用了 Telegram 截图页面：<https://t.me/screenshots20/13>。这是截图链接，不是 Play Integrity 修复包的下载地址；原帖没有提供单独修复包的 href。

## 验证范围

机型、ROM 特性、已知问题、第二版修复及 `.bat` 操作说明均为作者报告；没有可供核对的远端资产元数据，也没有下载文件或进行实机验证。链接能否下载以及包是否适用于目标设备，均未在本文中验证。

## 来源

- [Coolapk 帖子 75045221](https://www.coolapk.com/feed/75045221)
