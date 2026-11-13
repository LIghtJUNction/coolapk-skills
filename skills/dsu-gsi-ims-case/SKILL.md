---
name: dsu-gsi-ims-case
description: 查阅 DSU Sideloader 启动 GSI 的原帖步骤、20G 用户空间、回主系统再进 DSU 的过程及特定 Treble IMS 修复案例。
---

# DSU 启动 GSI 与 IMS 通话修复案例

## 工具与镜像入口

原帖提到 DSU Sideloader 和 GitHub 的 `Doze-off`，没有提供具体镜像文件或构建版本。整理时通过 GitHub 仓库检索确认下列公开项目入口：

- [VegaBobo/DSU-Sideloader](https://github.com/VegaBobo/DSU-Sideloader)。
- [Doze-off/lineage_treble](https://github.com/Doze-off/lineage_treble)。
- [Doze-off/EvoX_treble](https://github.com/Doze-off/EvoX_treble)。
- [Doze-off/PixelOS_GSI_treble](https://github.com/Doze-off/PixelOS_GSI_treble)。

这是补充找到的项目入口，不是已确认与原帖测试相同的文件；未下载镜像或核对哈希，不能把仓库当前分支当作原帖的测试版本。

## 原帖流程

1. 原帖在官方 Hyper 1、Hyper 3 系统中尝试成功；某个 Hyper 4 移植包不能进入 DSU。正文没有给出明确机型、系统完整版本和镜像文件名。
2. 下载所选 GSI 的 IMG 镜像。
3. 安装 DSU Sideloader，本案例通过 Root 授权使用它，不据此推断该工具所有使用方式都必须 Root。
4. 选择 IMG，用户空间设为 **20G**，其余选项按原帖保持默认。
5. 安装后重启手机，再打开工具选择重启到 DSU。

## 测试现象要分开

| GSI | 原帖报告 |
|---|---|
| LineageOS、Evolution X | 指纹不可用；初始可用移动数据，但不能通话 |
| Pixel OS | 按拨号键没有通话界面，但实际可以通话 |
| 某个 Hyper 4 移植主系统 | DSU 无法启动；不能推广为全部 Hyper 4 系统都失败 |

原帖没有详细测试所有功能，也没有证明所有 GSI 均可用；“理论上都可以”不能当作兼容清单。

## 特定 IMS 设置路径

原帖对前述不能通话的环境使用：

1. 打开 Treble 设置 → `IMS setting`。
2. 先选择 `Install Mediatek` 安装。
3. 开启 `Request IMS network`、`force the presence of 4G calling`、`Create IMS APN`。
4. 重启后先回到主系统，再用 DSU 工具重启进入 DSU。

作者报告之后通话恢复，但步骤显式涉及 Mediatek 安装项，正文又未说明具体平台、运营商和 IMS 包版本，不能作为高通或全部设备的通用修复。指纹问题仍未解决；原帖还称 DSU 不能隐藏 BL 状态，没有提供相关测试方法。

来源：[原帖](https://www.coolapk.com/feed/74074935)。整理日期：2026-11-13。原帖经验未做实机复现，图片与评论未全部读取；公开分享页及文件内容未逐一验证。
