# Motorola X70 Air 解锁与 KernelSU Root 流程

来源：[Coolapk 帖子](https://www.coolapk.com/feed/68624173)

## 解锁

帖子指向 [ixmoe 论坛解锁教程](https://bbs.ixmoe.com/t/topic/25562)，并称按该教程步骤操作，“没啥坑点”。准备条件是稳定的全球互联网连接、一台电脑和一个 Google 账号。原帖未抄录教程中的具体解锁命令、设备选项或详细步骤；请以链接中的教程为准，不能仅凭这篇记录补全。

## KernelSU Root

原帖给出的 KernelSU 下载资源是 [KernelSU v2.0.0 启动镜像压缩包](https://github.com/tiann/KernelSU/releases/download/v2.0.0/android15-6.6.82_2025-04-boot-gz.img.gz)。原帖给出的解压后命令使用文件名 `android15-6.6.82_2025-04-boot.img`；压缩包内实际文件名未读取，若不相同需先核对，不能直接假定解压后自动得到该名称。先用 fastboot 临时启动该镜像，以确认下载的文件正确：

```sh
fastboot boot android15-6.6.82_2025-04-boot.img
```

随后安装 `KernelSU.apk`。原帖未提供 APK 下载链接，也未说明 APK 的版本、如何取得或安装命令。记录以“没有然后了”结束，没有进一步描述持久化安装、验证 Root 状态或排错步骤，因此不应将这些内容视为已记录或已验证。

## 适用信息与证据范围

- 设备：Motorola X70 Air（按原帖所述）。
- 启动镜像文件名包含 `android15-6.6.82_2025-04`；这是资源文件名，不足以单独证明设备系统版本或内核适配范围。
- 原帖描述的是作者记录的基本流程，没有给出测试日志、成功验证结果或其他设备的适配证据。
- 帖子有 0 张图片；本文不依赖图片补充步骤。
