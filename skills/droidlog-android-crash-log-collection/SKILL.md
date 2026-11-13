---
name: droidlog-android-crash-log-collection
description: 使用Droidlog整理Android多来源日志、结构化崩溃、AMS连带死亡与PID时间线，并查询0.1.4程序和开机日志模块资源。
---

# Droidlog：Android 日志采集与崩溃溯源

Droidlog 是一款桌面端 Android 日志采集与分析工具。帖子介绍其技术栈为 Tauri 2、Rust、React 和 TypeScript，并称界面采用 Material Design 3；另有内置 ADB 的便携版，解压后即可使用，无需另配环境。

## 帖子介绍的能力

- 从 logcat、内核日志、Recovery、Dropbox、墓碑和 ANR 等来源采集日志。
- 支持多设备并行采集，并按级别和规则过滤。
- 识别结构化崩溃，尝试还原 Android Activity Manager Service（AMS）的连带死亡链路，关联进程身份和 PID 重启。
- 标记资源异常并提供完整性告警和时间线视图，可从分析结果跳转回原始日志行。

这些能力是帖子中的项目介绍，本任务没有安装或实机验证。

## 下载与连接

1. 打开 [Droidlog GitHub 仓库](https://github.com/starf233-2/Droidlog)。
2. 按帖子所述，在仓库的 Releases 页面选择 MSI 安装包或 setup 版下载。本次另查发布集合，核对到标签为 `0.1.4`（没有 `v` 前缀），资产链接见下表；未下载或执行。
3. 在手机上开启 USB 调试并连接电脑，然后开始采集和导出。

帖子称项目采用 Apache-2.0 许可。此许可信息未在本任务中核对仓库元数据。

## 0.1.4 更新说明

帖子注明 0.1.4 新增配套 KSU 模块，可在本机记录完整开机日志，并在 Droidlog 中读取、分析崩溃和导出；模块据称可从 Release 下载或从源码构建。此版本号是帖子中的更新记录，不代表当前最新版本。完整开机日志记录功能也不等于所有日志均可在无需 root 或其他授权的情况下采集；帖子未列明模块所需的具体权限与配置。

来源：[酷安帖子 74193981](https://www.coolapk.com/feed/74193981)。

## 0.1.4 发布资产快照

[发布页](https://github.com/starf233-2/Droidlog/releases/tag/0.1.4)。下表 SHA-256 来自 GitHub 资产元数据，未下载后重算；版本号不代表永远最新。

| 文件 | 下载 | SHA-256 |
|---|---|---|
| `Droidlog-BootLog-v1.0.0.zip` | [下载](https://github.com/starf233-2/Droidlog/releases/download/0.1.4/Droidlog-BootLog-v1.0.0.zip) | `df82e5125962650149ecb178354b1adb34e16d3e9120327cf0c4adfd8037aa8c` |
| `droidlog-standalone.exe` | [下载](https://github.com/starf233-2/Droidlog/releases/download/0.1.4/droidlog-standalone.exe) | `ac915ab8042a8335d74b0034e72e8b8589972a86f7879bece13b0840f43c01f0` |
| `Droidlog_0.1.4_x64-setup.exe` | [下载](https://github.com/starf233-2/Droidlog/releases/download/0.1.4/Droidlog_0.1.4_x64-setup.exe) | `0021201093f0df4bb819285efc0ac876d31e220c0395b7a065b4a1aab8244df4` |
| `Droidlog_0.1.4_x64_en-US.msi` | [下载](https://github.com/starf233-2/Droidlog/releases/download/0.1.4/Droidlog_0.1.4_x64_en-US.msi) | `58e7ff513e5e0111aa26c555e1278fb43be2029bf821ded3f0c7b38696dec720` |
