---
name: enuma-lineageos-rom-versions
description: 按版本核对小米平板5Pro5G enuma的非官方LineageOS23.2和24.0底包、IMG、ZIP、下载与刷入流程。
---

# Xiaomi Pad 5 Pro 5G（enuma）：非官方 LineageOS 23.2 与 24.0

两篇帖子分别介绍 enuma 的 LineageOS 23.2 和 24.0。以下安装前提和流程依帖子整理；GitHub 资产文件名、下载地址、大小及 SHA-256 来自提供的远端 release 元数据快照。没有下载二进制或在设备上刷机。

## 共同前提与流程

- 刷机底包要求为 `HyperOS1.0.3.0.TKZCNXM`。
- 开始前需解锁 Bootloader；刷入 ROM 后不要重新锁定 Bootloader。
- 刷机有数据丢失或设备故障风险，应先备份重要数据。作者称 ROM 未经全面测试，且不保证稳定性。
- 首次刷入时，在 Recovery 选择 **Factory reset**，这会清除全部数据；之后选择 **Apply update → Apply from ADB**。
- GApps 可选。帖子没有提供 GApps 下载地址。

**Fastboot 阶段：**先确认当前工作目录的两个 IMG 来自同一目标版本。

```sh
fastboot flash boot boot.img && fastboot flash vendor_boot vendor_boot.img
```

手动重启进入 Recovery，按上文完成首次清除及 Apply from ADB，再按目标版本选择下面一条命令，不能连续执行两版：

**23.2：**

```sh
adb sideload lineage-23.2-20261107-UNOFFICIAL-enuma.zip
```

**24.0：**

```sh
adb sideload lineage-24.0-20261112-UNOFFICIAL-enuma.zip
```

每个版本的 ROM ZIP、`boot.img`、`vendor_boot.img` 必须成套，不可跨 23.2 / 24.0 混用。同名 IMG 并不表示内容相同，下表摘要不同。按帖子流程，先在 Fastboot 刷入两者，再重启进入 Recovery；首次安装执行 Factory reset，然后 Apply update → Apply from ADB 并 sideload 对应版本 ZIP，之后可选刷入 GApps，再重启。

## 版本与资产

GitHub 的确切文件名取代帖子中的日期占位写法。表格记录的是所提供的那次 release 资产快照，不代表当前永远最新。大小为字节；SHA-256 是远端元数据所列摘要，未下载文件重算验证。

### LineageOS 23.2

Release：<https://github.com/awkox/enuma-release-artifacts/releases/tag/v3-user-official-kernel-20261107>

| 文件 | 大小（字节） | SHA-256 | 下载 |
|---|---:|---|---|
| `boot.img` | 201326592 | `774bfbe7d1137ee8ff3917e7e55bbd118d55699fa5c561fc3724a7dc6f221f8b` | [下载 boot.img](https://github.com/awkox/enuma-release-artifacts/releases/download/v3-user-official-kernel-20261107/boot.img) |
| `vendor_boot.img` | 100663296 | `ae77dce2740cc3fadec70b4e46fe23c4cecd2688094499b1272e3319df4619a0` | [下载 vendor_boot.img](https://github.com/awkox/enuma-release-artifacts/releases/download/v3-user-official-kernel-20261107/vendor_boot.img) |
| `lineage-23.2-20261107-UNOFFICIAL-enuma.zip` | 1448471729 | `81c6d202a00ad640920cac8cb097512c9b6fbcac659ebbcccf2deb1c6a2277b4` | [下载 ROM ZIP](https://github.com/awkox/enuma-release-artifacts/releases/download/v3-user-official-kernel-20261107/lineage-23.2-20261107-UNOFFICIAL-enuma.zip) |
| `SHA256SUMS` | 266 | `c869dcef26ed2760fe1c501a21a98f26344665347444c29bae6a417ddc421b1b` | [下载 SHA256SUMS](https://github.com/awkox/enuma-release-artifacts/releases/download/v3-user-official-kernel-20261107/SHA256SUMS) |

帖子另提供 123 云盘链接：<https://1847093067.share.123pan.cn/123pan/0f1Tvd-7upxh>。未提供提取码。

### LineageOS 24.0

Release：<https://github.com/awkox/enuma-release-artifacts/releases/tag/LineageOS-24.0>

| 文件 | 大小（字节） | SHA-256 | 下载 |
|---|---:|---|---|
| `boot.img` | 201326592 | `4045fcb4148fb91950a0f7a7e48e8a71acb37beba09533ee5d4efe4ef1191dc1` | [下载 boot.img](https://github.com/awkox/enuma-release-artifacts/releases/download/LineageOS-24.0/boot.img) |
| `vendor_boot.img` | 100663296 | `85851ee8380202a502cc590d62ba0758cc724059c2a7f04bedda74efef3d726e` | [下载 vendor_boot.img](https://github.com/awkox/enuma-release-artifacts/releases/download/LineageOS-24.0/vendor_boot.img) |
| `lineage-24.0-20261112-UNOFFICIAL-enuma.zip` | 1488170288 | `01e4c303a76a5aea17f56331136fa66cafdf405e4607d9ec5f3ee2864dddc91c` | [下载 ROM ZIP](https://github.com/awkox/enuma-release-artifacts/releases/download/LineageOS-24.0/lineage-24.0-20261112-UNOFFICIAL-enuma.zip) |

帖子另提供 123 云盘链接：<https://1847093067.share.123pan.cn/123pan/0f1Tvd-IBsxh>。未提供提取码。

## 验证范围

作者提供了刷机说明，并提示未全面测试。GitHub 远端元数据核对了两次 release 的资产文件名、下载 URL、字节大小和 SHA-256 字段；这不等同于下载后重算校验。本资料整理没有下载二进制，也没有进行实机验证。

## 来源

- [Coolapk 帖子 75048113](https://www.coolapk.com/feed/75048113)
- [Coolapk 帖子 74924573](https://www.coolapk.com/feed/74924573)
- [LineageOS 24.0 GitHub release](https://github.com/awkox/enuma-release-artifacts/releases/tag/LineageOS-24.0)（远端资产元数据）
- [LineageOS 23.2 GitHub release](https://github.com/awkox/enuma-release-artifacts/releases/tag/v3-user-official-kernel-20261107)（远端资产元数据）
