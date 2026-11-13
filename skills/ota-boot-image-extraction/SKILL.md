---
name: ota-boot-image-extraction
description: 从匹配机型版本的 OTA 中提取 boot 分区，并核对原帖 PowerShell 内核替换示例及相关资源时使用。
---

# 一加 OTA 包提取 boot.img

## 从匹配版本的 OTA 提取 boot.img

先安装原帖指定的工具，以下地址由原帖 GitHub 链接展开：

```sh
uv tool install git+https://github.com/5ec1cff/payload-dumper
```

未使用 uv 时，原帖另给 pip 方式，两者任选其一：

```sh
pip install git+https://github.com/5ec1cff/payload-dumper
```

[上游 README](https://github.com/5ec1cff/payload-dumper#usage)给出的完整选项是 `--partitions`。将下例占位符替换为实际 OTA 包地址：

```text
payload_dumper --partitions boot <OTA包URL>
```

原帖写作 `--partitio`；这里采用上游文档中的完整拼写，不据此认定所有版本均接受缩写。上游说明支持从含 `payload.bin` 的在线 ZIP 按分区提取，避免先下载完整包；实际仍取决于输入包及服务端访问条件。原帖“约一分钟”的说法没有独立测速支持。

工具项目：[payload-dumper](https://github.com/5ec1cff/payload-dumper)。一加 ROM 下载目录：[OnePlus_Roms](https://yun.daxiaamu.com/OnePlus_Roms/)。目录链接不保证包含任意版本或机型的 OTA 包，使用时需从中取得所需 OTA 包的实际链接。

## 提取后替换内核镜像

另一篇原帖展示了 OnePlus Ace 2、OxygenOS `15.0.0.820` 的例子：用上述工具从 OTA 包提取 `boot`，默认结果位于 `output` 文件夹；进入该目录查看文件。若有 `anykernel3.zip`，原帖建议取出其中的 `Image`，再用 `magiskboot` 解包 boot、替换内核镜像并重新打包。原帖给出的命令为（先确认 `boot.img` 与解出的 `Image` 在当前目录）：

```powershell
magiskboot unpack boot.img
mv -Force Image kernel
magiskboot repack boot.img
```

`mv -Force` 是原帖所示 Windows PowerShell 写法；不要当作 Linux 命令。原帖说 Linux 可通过包管理器安装 `magiskboot`，Windows 则需下载后放入 `PATH` 可搜索目录。工具发布页：[magiskboot 1.0-3](https://github.com/svoboda18/magiskboot/releases/tag/1.0-3)。

## 依证据边界使用

这是原帖记录的提取和内核替换流程，不是刷机步骤，也没有给出刷入命令或设备恢复方法。示例机型与系统版本为 Ace 2 / `15.0.0.820`；不能据此保证其他机型或版本适用。原帖图片和评论未读，文件未下载或校验。本例只处理 boot，不据“Root 丢失”推断所有设备均应修补 boot；目标分区和恢复方法应按具体设备方案确认。

来源：酷安动态 [65499194](https://www.coolapk.com/feed/65499194)、[66785167](https://www.coolapk.com/feed/66785167)。

## 原帖 OTA 样例地址

[Ace 2 / 15.0.0.820 原帖所附 OTA ZIP](<https://gauss-componentotacostmanual-cn.allawnfs.com/remove-f8f3a50549d773ee1cf0a341f5273f5a/component-ota/25/05/12/34cf28d983f24e0d9621d69ff04e60f6.zip>)。这是一条历史原始地址，不是跨版本通用包；本次没有核对其当前可下载状态或文件完整性。

## 内核构建资源

另一帖仅分享以下项目入口，没有给出编译参数、分支、设备适配或产物安装步骤，不能当作完整构建教程：

- [OnePlus MKSU / SUSFS Actions](https://github.com/ShirkNeko/Action_OnePlus_MKSU_SUSFS)（原帖标签 sukisu）
- [KernelSU-Next Actions](https://github.com/Numbersf/Action-KernelSU-Next)（原帖标签 ksun）
- [OnePlus kernel manifest](https://github.com/OnePlusOSS/kernel_manifest/)

来源：[资源分享帖](https://www.coolapk.com/feed/65499121)。上游命令文档核对日期：2026-11-14；未运行提取、替换内核、刷写或构建操作。


## 内核配置与 LKM 的原帖计划

另一帖介绍 Ace 2 的后续内核计划：基于开源内核、按需要启用部分配置，并计划重点支持 Mountify 一类元模块。原帖用“ramdisk 侧注入、kernel 侧配置”说明自编译内核与 LKM 方案可以并存；这只是该帖的简化解释，不作为所有设备 boot/init_boot 镜像布局或 KernelSU 版本机制的通用描述。

原帖关于 KernelSU v3 模式变化及未来支持的说法按历史背景和发布计划记录，没有给出此次内核的源码、构建产物、固件适用条件或刷写步骤，也没有提供 Mountify 下载地址，不能据此执行刷写或认定计划已经交付。来源：[Ace 2 内核计划](https://www.coolapk.com/feed/68964164)。
