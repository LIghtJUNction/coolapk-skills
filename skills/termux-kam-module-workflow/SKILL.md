---
name: termux-kam-module-workflow
description: 用 Kam 初始化与构建 Android 管理器模块
---

# 用 Kam 初始化与构建 Android 管理器模块

Kam 项目面向 KernelSU 模块开发，旨在复用构建流程并管理依赖。本文只覆盖来源动态明确提到的命令和能力；具体参数、环境要求及安装细节应以项目文档为准。

项目：[MemDeco-WG/Kam](https://github.com/MemDeco-WG/Kam)。示例模块项目：[LIghtJUNction/MagicNet](https://github.com/LIghtJUNction/MagicNet)。

## 使用流程

来源动态给出的基本构建命令：

```sh
kam build
```

初始化模块：

```sh
kam init -i
```

初始化前需要先拉取模板。来源没有提供模板地址、拉取命令或 `kam init` 的完整交互说明，因此这里不补造这些步骤。先查看 Kam 仓库当前文档，按其模板说明准备后再初始化。

## 为项目安装开发技能

来源建议将 Kam 技能安装到项目中：

```sh
npx skills add MemDeco-WG/Kam
```

此命令及其当前可用性未在本文中验证；运行前应核对仓库说明与本机 `npx` 环境。

## 来源所列能力

动态称 Kam 支持使用 KSU 开发者私钥为模块签名、复用模块构建工作流，以及配置 pre-build/post-build hooks。私钥属于敏感凭据，应保存在受控环境中，不要提交到公开仓库或日志。动态还称 Termux 用户可从 GitHub 仓库现场构建并安装模块，但只支持使用 Kam 构建的模块；来源没有给出该命令的具体语法。

## 缺失信息

来源没有给出 Kam 的安装命令、版本要求、模板获取方式、hook 配置格式、签名参数或 Termux 现场构建命令。需要这些步骤时，应直接查阅 Kam 仓库的当前文档，不要根据功能简介推断参数。

来源：[Coolapk feed 72569153](https://www.coolapk.com/feed/72569153)。
本稿整理文字来源；未执行安装、构建、签名、扫描或设备测试。
