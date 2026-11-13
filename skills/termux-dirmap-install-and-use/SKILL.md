---
name: termux-dirmap-install-and-use
description: 在 Termux 中安装并运行 dirmap
---

# 在 Termux 中安装并运行 dirmap

本文记录来源动态中给出的 Termux 安装与基本调用方式。仅对自己拥有或明确获准测试的域名使用扫描工具；不要将授权范围外的域名作为目标。

项目仓库：[LIghtJUNction/dirmap](https://github.com/LIghtJUNction/dirmap)。原项目链接：[H4ckForJob/dirmap](https://github.com/H4ckForJob/dirmap)。

## 安装

安装 uv 并更新 shell 配置：

```sh
pkg install uv
uv tool update-shell
```

安装项目：

```sh
uv tool install https://github.com/LIghtJUNction/dirmap.git
```

来源 HTML 中该命令的链接明确指向 `https://github.com/LIghtJUNction/dirmap.git`，此处按该链接恢复完整命令。

## 调用

来源给出的基本形式：

```sh
dirmap -i [域名]
```

将 `[域名]` 替换为自己有权测试的目标域名。来源没有提供其他参数、输出解释、扫描速率、兼容性信息或错误排查步骤；需要这些信息时查看仓库文档，不要自行假定参数含义。

来源：[Coolapk feed 69376510](https://www.coolapk.com/feed/69376510)。
本稿整理文字来源；未执行安装、构建、签名、扫描或设备测试。
