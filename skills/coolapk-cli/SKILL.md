---
name: coolapk-cli
description: "基于帖子记录的 Rust 酷安命令行工具信息；包含已报告的安装条件、命令示例、鉴权机制及未验证范围。"
---

# 酷安 CLI：安装、认证与使用边界

`coolapk-cli` 是用 Rust 编写的酷安命令行工具，安装后执行的命令名为 `coolapk`。帖子称其以 JSON 输出为主，可用于终端、脚本或 Agent 工作流。API 为非官方接口，可能随服务端变化而失效。

## 安装条件

帖子给出的要求是 Rust 1.99 或更高版本，安装命令为：

```sh
cargo install coolapk-cli --locked
coolapk --help
```

Cargo 包名为 `coolapk-cli`，命令名为 `coolapk`。帖子还列出 `coolapk boards` 用于查看板块。

## 鉴权与设备信息

API 请求需要从用户自己的酷安 APK 提取鉴权数据，且提取参数应与 APK 版本对应。帖子示例使用 APK 版本 `16.6.5`、版本码 `2610162`：

```sh
coolapk auth --extract /path/to/coolapk.apk --app-version 16.6.5 --version-code 2610162
```

示例版本不代表当前最新版或兼容性保证。

若设备已 Root、酷安已登录且 ADB 已授权，帖子给出的流程是：

```sh
coolapk login --adb
coolapk whoami --online
```

作者报告 ADB 登录会同步手机设备信息。帖子也提到 Cookie 与短信登录入口，但其具体步骤和限制未在输入材料中展开。获取 Cookie 本身不保证写操作可用；帖子记录了发布曾因设备环境异常失败，调整为同步实际设备信息、机型、SDK 和 User-Agent 后成功。

## 命令示例与功能范围

```sh
coolapk search "Rust" --pretty
coolapk home --tab hot
coolapk board digital
coolapk user 123456 --feeds
```

帖子列举了搜索帖子、用户、话题和应用，读取首页、热榜、板块、帖子、评论、用户动态与通知，以及点赞、回复、关注、发帖和上传等能力。

实测范围按帖子自述包括 ADB 登录、搜索、通知、发帖和帖子回读。短信登录及其他写操作未全部测试；功能清单不应视为逐项验证结果。

## 证据与限制

本文仅整理帖子提供的信息，未实机验证安装、认证或 API 行为。README 和链接页面的内容未读取；如需确认当前依赖、登录流程、命令参数和版本兼容性，需补读项目文档及发布材料。

来源：[帖子 75045281](https://www.coolapk.com/feed/75045281)、[帖子 75044485](https://www.coolapk.com/feed/75044485)、[帖子 75044317](https://www.coolapk.com/feed/75044317)。
