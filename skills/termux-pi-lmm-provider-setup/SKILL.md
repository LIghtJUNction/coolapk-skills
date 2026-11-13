---
name: termux-pi-lmm-provider-setup
description: 在 Termux 中配置 Pi 的 LMM Provider
---

# 在 Termux 中配置 Pi 的 LMM Provider

本文记录一份面向 Termux 用户的 Pi LMM Provider 配置流程。它适合已有 Termux 环境、希望通过 OAuth 使用 LMM Provider 的用户。原帖提醒 token 可能产生费用；实际价格与可用模型应以服务方当前信息为准。

## 前置条件

- 已安装 Termux。
- 需要 Node.js、Termux API 和 Git。原帖列出它们的安装命令，但没有说明如何安装 Pi CLI 本身。
- Pi 版本至少为 0.86.1，Node.js 至少为 22.19.0，并需要 LMM 账户。原帖未提供这些版本检查或账户注册步骤。

## 安装 Provider

在 Termux 中安装依赖：

```sh
pkg update && pkg upgrade
pkg install nodejs termux-api git
```

当前建议使用 source-switching 安装器：

```sh
npm exec --yes --package=@tokennotincluded/pi-lmm-provider@alpha -- lmm-pi-provider git:github.com/TokenNotIncluded/pi-lmm-provider
```

只有在新源安装成功后，才移除同一 Provider 包的其他配置。安装后重启 Pi。

原帖采用的历史写法是：

```text
pi install git:github.com/TokenNotIncluded/pi-lmm-provider
```

该写法仅作为原帖记录；当前安装建议见上方命令。仓库入口：[TokenNotIncluded/pi-lmm-provider](https://github.com/TokenNotIncluded/pi-lmm-provider)。

## 登录与选择模型

先在终端运行 `pi` 打开 Pi，然后在交互界面执行：

```text
/login
```

按提示打开网页并完成 OAuth 授权。原帖随后使用 `lmm` 选择 Provider、使用 `/model` 选择模型；其中 `lmm` 是提供商选择项；这些是原帖记录的交互步骤，未在此处验证它们与当前版本的兼容性。

## 已知缺口

原帖没有 Pi CLI 自身的安装步骤。安装 Node.js 不等于安装 Pi，因此应先按 Pi 当前官方文档准备 Pi CLI；本文不补写缺失的安装命令。本文也不承诺 token 免费、特定模型可用或 OAuth 流程在所有设备上都相同。

来源：[Coolapk feed 73747860](https://www.coolapk.com/feed/73747860)。
版本前提及来源切换行为补充自项目 README 的安装/登录段；其余长篇文档未完整阅读。未安装、登录或产生模型费用。

本稿整理文字来源；未执行安装、构建、签名、扫描或设备测试。
