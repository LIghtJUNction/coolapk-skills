---
name: fastboot-enhance-payload-tools
description: 使用Fastboot Enhance的公开资源与README区分Payload解包、fastbootd刷写、全量与增量包；保留Windows运行条件和固定版本下载链接。
---

# Fastboot Enhance 功能记录

原帖介绍的功能包括：

- 可视化解包 `Payload.bin`。
- 以图形界面操作 Fastboot 分区刷写。
- 支持在 Fastbootd 中直接刷写 `Payload.bin`。

原帖把下载入口指向 [AKR Developers 的 Fastboot Enhance 页面](https://www.akr-developers.com/d/506)。这里是原帖所给的项目入口；当前可用的下载文件、版本和使用要求未由原帖正文提供，不能据此确认。

来源：[酷安原帖](https://www.coolapk.com/feed/28656959)。

## 另读项目说明后的使用条件

[项目 README](https://github.com/libxzr/FastbootEnhance)说明这是 Windows 工具，需要 .NET Framework 4.5+；下载 Release.zip、解压后启动 FastbootEnhance.exe。除解包外，还列出查看 fastboot/Payload 信息、切换启动模式与 A/B 槽、选择镜像、管理逻辑分区及查看动态分区元数据等功能。

项目不完整支持增量包；其说明仅允许在校验通过时从增量包提取镜像，不能将此扩展成支持增量包刷写。未勾选 ignore checksum 时会进行校验。仓库页面标为已归档，不将旧功能说明视作仍在维护的承诺。

本次发布接口返回 [v1.4.0](https://github.com/libxzr/FastbootEnhance/releases/tag/v1.4.0)，日期为2022-11-04。资产直链：[Release.zip](https://github.com/libxzr/FastbootEnhance/releases/download/v1.4.0/Release.zip)，1,770,685字节；接口未给digest，不补造SHA-256。没有下载、运行程序或执行分区操作。
