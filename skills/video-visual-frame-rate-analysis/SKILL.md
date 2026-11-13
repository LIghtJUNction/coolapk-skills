---
name: video-visual-frame-rate-analysis
description: 使用相邻视频帧差异理解视觉帧率估计，区分差异阈值、录屏开销与固定相机记录的条件。
---

# 用 trdrop 从视频分析视觉帧率

原帖用 trdrop 分析已有视频并估算视觉帧率：比较相邻帧的像素变化，只有变化超过 Difference Threshold（差异阈值）时才将其计为新帧。原帖将其作为“所见即所得”的视觉帧率核对方法，适用于怀疑 `adb dumpsys` 结果与实际观感不一致时辅助验证。

trdrop 支持调整 Difference Threshold。若用视频评估性能，录屏本身会产生开销，测试结果可能与未录屏时不同。原帖建议可尝试使用固定摄像头进行外部录制，并调高容错；没有提供阈值数值、录制帧率或量化测试结果。

## 资源

- 项目仓库：https://github.com/cirquit/trdrop

这是项目仓库链接；原帖没有提供独立下载文件或版本号。

## 来源与证据边界

依据酷安文章《通过视觉分析帧率》的文字内容。原帖含一张图片，但图片内容未提供；未据此补充操作界面或参数。

来源：[酷安原帖](https://www.coolapk.com/feed/70359494)。


视觉差异计数不等于渲染完成事件计数。静止内容、压缩噪声和拍摄采样率都可能影响结果；不能把一次差异判定直接当作 GPU 渲染时间或上屏时间的证明。


## 另读项目 README 的输入与输出条件

本次读取的 [README](https://github.com/cirquit/trdrop/blob/master/README.md) 标题为 v1.1.1，说明主要输入是固定采样率捕获的未压缩视频，以帧间变化估计内容帧率和单帧显示时长，并尝试检测撕裂。它列出最多三路视频比较、帧率/帧时间 CSV、图像序列、视觉叠加和差异帧导出。

README 给出的程序入口是 [Windows 安装包发布页](https://github.com/cirquit/trdrop/releases)；Linux/macOS 在该文档中仍列为计划，不据此生成 Linux 安装命令。README 同时说明这属于算法演示，不能当作精密科学测量保证。软件未安装，样例视频也未运行。


本次发布接口返回标签 `a0b4563`，其资产为 [trdrop-release-a0b4563.7z](https://github.com/cirquit/trdrop/releases/download/a0b4563/trdrop-release-a0b4563.7z)，26,360,775 字节。它是压缩包，未下载或运行；GitHub 此资产未给出 digest，因此不补造 SHA-256。发布标签与 README 标题号分别记录，不假定相等。
