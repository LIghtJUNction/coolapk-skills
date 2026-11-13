---
name: kam-module-cli
description: 使用 Kam 搜索下载模块、委托 root 管理器安装及检查项目时使用；区分 0.5.x 历史语义。
---

# Kam 0.5.x 命令与环境功能

本文按帖子所述 Kam v0.5.0、v0.5.x 与 v0.5.12 行为整理，未实机验证。不同版本的命令状态可能不同。

## 模块仓库搜索与下载

v0.5.x 帖子称 Kam 可搜索并下载 KernelSU 官方模块仓库内容，不计划支持 MMRL 仓库。v0.5.12 帖子给出的命令语义如下：

```sh
kam -S asl
kam -Ss asl
kam -Syu id
```

- `kam -S asl`：搜索并下载匹配的 `asl` 模块。
- `kam -Ss asl`：仅搜索，不下载。
- `kam -Syu id`：此工具中用于更新/刷新模块仓库索引 JSON，`id` 是搜索关键词。它不更新已经安装的模块。

最后一条与常见 pacman 用法不同：帖子特别指出 pacman 的 `-Syu` 表示软件包更新，通常不接模块 ID；不可把该语义直接套用到 Kam。

## 安装模块

Kam 0.5.0 起增加 `install`：

```sh
kam install 模块路径
kam install asl.zip
```

帖子称命令会依据配置调用相应 root 管理器的安装接口：KernelSU 使用 `ksud`，APatch 使用 `apd`，Magisk 使用其对应安装方式。使用前需手动设置管理器，示例：

```sh
kam config set root.manager ksu
```

帖子称这是一次性配置；未说明所有管理器的配置值及版本兼容矩阵。安装速度受网络影响的说法属于原帖经验，未提供测量。

## 检查文件

v0.5.12 帖子描述 `kam check` 支持 `--json` 与 `-v` 输出选项，功能仍处于试验阶段。检查目标包括非二进制文件的行尾控制符、Shell 脚本，以及 Markdown、JSON、YAML；这些是目标范围，不是已实现规则的完整清单。帖子称 Shell 脚本检查依赖 ShellCheck，且当时 Markdown、JSON、YAML 检查计划在 0.6.0 前完善；因此不能视为所有规则已稳定或齐备。

ShellCheck 安装示例：

```sh
pkg install shellcheck
apt install shellcheck
sudo pacman -S shellcheck
```

分别对应 Termux、Ubuntu 与 Arch。帖子表示 Windows 与 macOS 不在支持范围内，需自行测试。

## 语言与环境

帖子记录国际化目标为中文和英文。配置中文的两条版本相关命令如下：

```sh
kam config --global set ui.language zh
kam config --global list
```

较新的 v0.5.12 帖子另给出：

```sh
kam config --global ui.language zh
```

这是不同帖子的原始写法，不推定其等价；请以安装版本的 `kam config --help` 为准。早期帖子称 `kam config show` 可查看内置配置项。

`kam env` 用于查看相关环境变量。构建框架还会注入 `KAM_*` 环境变量；具体完整列表指向子项目文档，当前输入未提供该文档内容。

## MCP 项目状态工具

同帖记录 kamcp 增加 `kam_status`，返回项目状态与检查结果；具体兼容版本未给出。安装和传输配置参见[独立 kamcp 技能](../kamcp-mcp-setup/SKILL.md)。

## 资源

- Kam 项目：<https://github.com/MemDeco-WG/Kam>
- kamcp 对应帖子提到的上文：<https://www.coolapk.com/feed/69234886>

来源：<https://www.coolapk.com/feed/69234886>、<https://www.coolapk.com/feed/69243761>、<https://www.coolapk.com/feed/69250473>、<https://www.coolapk.com/feed/69269555>。外链页面内容未在本次资料中提供。
