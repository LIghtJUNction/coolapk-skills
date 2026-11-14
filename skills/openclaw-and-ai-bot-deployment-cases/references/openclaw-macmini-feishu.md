# Mac mini 上的 OpenClaw 与飞书

## Homebrew、Node.js 与 Git
先检查 Homebrew：

```sh
brew -v
```

如未安装，原帖给出的官方脚本命令在移除原文明确插入的“删”字后为：

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

安装后按终端提示配置 shell。原帖示例（其中用户名路径需按本机调整）：

```sh
echo >> /Users/susu/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> /Users/susu/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv zsh)"
```

安装 Node.js 22：

```sh
brew install node@22
echo 'export PATH="/opt/homebrew/opt/node@22/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
node -v
npm -v
git --version
```

原帖称 Node.js 需 22 及以上，Git 一般由 macOS 提供。

## OpenClaw 与模型
原帖的一键安装命令：

```sh
curl -fsSL https://openclaw.ai/install.sh | bash
```

配置向导若退出，可重新执行：

```sh
openclaw onboard --install-daemon
```

原帖选择 QuickStart、Custom Provider，Base URL 为 `https://api.siliconflow.cn/v1`，Endpoint 选 OpenAI-compatible，模型 ID 示例为 `Pro/MiniMaxAI/MiniMax-M2.5`。API Key 从服务商账户生成后粘贴；不要复用帖子之外的密钥。原帖报告模型验证成功。

## 飞书机器人接入
配置向导中选择 Feishu 插件，下载方式选 Download from npm；原帖注明较新 OpenClaw 版本可能已有内置飞书插件。飞书开放平台创建企业自建应用、添加机器人并申请权限。原帖给出权限 JSON，核心范围包括文件、消息、机器人、文档、表格与 Wiki 权限；其清单与 Android 教程中的 JSON 相同，完整权限项见 [Android 飞书接入文档](#)（此处为同主题记录，不代表原帖另有链接）。由于原文 JSON 排版有空行和缩进不一致，且权限列表很长，本记录不在此重复第二份；来源帖子中的具体权限列表可按 Android 文档所列逐项对照，但两帖分别独立提供权限 JSON。

在 Mac 终端按向导粘贴 App Secret 和 App ID，选择 WebSocket (default)、Feishu - China、Allowlist；群聊 allowlist 可先留空。其余初始化项中 Search provider 选 Skip for now、skills 选 NO，hooks 按需启用。Gateway 启动后，在飞书开放平台把事件订阅设为长连接并添加“接收消息”事件，创建并发布应用版本。原帖提示 Gateway 未运行或渠道未添加时，长连接设置可能保存失败。

用户首次给机器人发消息后，按返回的配对码在 Mac 终端执行：

```sh
openclaw pairing approve feishu <配对码>
```

原帖报告配对后飞书机器人能响应。配对码是一次性授权信息，本文不保留原帖具体码。

## 来源与边界
来源：酷安帖子 [它来了！Mac mini + OpenClaw + 飞书接入：保姆级详细图文教程（上篇）](https://www.coolapk.com/feed/70693335)。原帖图片未读取。原帖提及续篇但未给出可解析链接。权限配置具体 JSON 在原帖中可读；本记录未复写全部清单，故飞书权限部分应以原帖 JSON 为准，不应把 Android 帖子视为本帖权限配置的唯一依据。
