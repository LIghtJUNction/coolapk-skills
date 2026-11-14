# 在 Android 设备上测试受保护的 Microdroid

来源：[Coolapk 原帖](https://www.coolapk.com/feed/66587066)。原帖讨论在 Android 设备上测试 pKVM（受保护的基于内核的虚拟机）及 Microdroid；作者说明自定义 VM 的运行流程仍在摸索，因此本文仅记录 Microdroid 测试。

## 背景与适用条件

原帖提到两篇较早的免 root 运行 pKVM 帖子：[帖子一](https://www.coolapk.com/feed/60032562)和[帖子二](https://www.coolapk.com/feed/60427443)。原帖认为 Polygraphene 的 `gunyah-on-sd-guide` 项目需要 root。Pixel 在 Android 13 上的支持情况只作为背景提及，原帖没有讨论该设备路径。

Android 虚拟化框架（AVF）的资料位于 [AOSP Virtualization 仓库](https://cs.android.com/android/platform/superproject/main/+/main:packages/modules/Virtualization/)。原帖建议可用出厂搭载 Android 15 的手机测试 pKVM 支持；这是一项建议，不是对所有 Android 15 设备的兼容性保证。

## 方法一：安装 Microdroid demo app

构建 demo app 需要克隆整个 AOSP 仓库。原帖使用此前克隆的 LineageOS 22.2 仓库构建；作者指出它可能与 AOSP 有差异，但可用于测试设备是否支持 pKVM。

使用 adb 安装 APK 并授予虚拟机管理权限：

```sh
adb install -t -g MicrodroidDemoApp.apk
adb shell pm grant com.android.microdroid.demo android.permission.MANAGE_VIRTUAL_MACHINE
```

运行时需使用 adb 安装并授予权限，且测试时要勾选界面中的两个选项。原帖在红米 K80 Pro 上测试发现：通过 demo app 无法进入 Microdroid 终端执行命令；改用 Microdroid 测试脚本则正常。该结果仅是原帖所述设备上的测试观察。

## 方法二：使用 AOSP 测试脚本

原帖给出的脚本是 [vm_shell.sh](https://cs.android.com/android/platform/superproject/main/+/main:packages/modules/Virtualization/android/vm/vm_shell.sh)。在 Linux 环境中运行：

```sh
./vm_shell.sh start-microdroid --auto-connect -- --protected
```

其中 `start-microdroid` 启动 Microdroid，`--auto-connect` 请求自动连接，`--protected` 指定受保护 VM。原帖报告此脚本在红米 K80 Pro 上可正常运行 Microdroid 测试。

## 无 Linux 环境时

原帖建议可查看上述脚本的具体内容，并提出理论上也能通过 adb 或 Shizuku 执行以下命令测试：

```sh
/apex/com.android.virt/bin/vm run-microdroid --protected
```

这是原帖的可行性推测，不是已报告完成的测试结果。具体权限、调用方式及不同设备上的行为，原帖未提供验证记录。

在 adb shell 中查看命令帮助：

```sh
/apex/com.android.virt/bin/vm --help
/apex/com.android.virt/bin/vm run-microdroid --help
/apex/com.android.virt/bin/vm run --help
```

## 范围与资料缺口

原帖没有给出 demo app 的完整构建命令、AOSP/LineageOS 仓库检出步骤、APK 下载链接或构建产物链接；也没有提供自定义 VM 的完整运行教程。原帖含有 3 张图片，但图片内容未提供在本篇文字中，本文不据此补充细节。

## 已读官方说明的历史入口

2026-11-14重新读取的[AOSP入门说明](https://android.googlesource.com/platform/packages/modules/Virtualization/+/refs/heads/main/docs/getting_started.md)另提供vm_shell.sh路径与准备条件；不能把其支持设备泛化到所有安卓机。本文没有在K80 Pro运行任何命令。
