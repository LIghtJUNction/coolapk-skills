# Windows 上的 OpenClaw 部署记录

## Node.js 与 Git
原帖要求 Node.js 22 及以上。使用 nvm-windows 的安装器示例目录为 `D:\nvm`，在 `settings.txt` 中配置镜像：

```text
node_mirror: https://npmmirror.com/mirrors/node/
npm_mirror: https://npmmirror.com/mirrors/npm/
```

原帖命令：

```powershell
nvm -v
nvm install 22
nvm use 22.20.0
node -v
npm -v
git --version
```

Git 安装器使用默认选项即可；原帖未要求特定 Git 版本。原帖提到 123 云盘可下载相关文件，公开提取码 `1314`，但未提供可恢复的实际分享 URL。

## 安装 OpenClaw
若 PowerShell 阻止脚本，原帖建议当前用户范围调整执行策略：

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

一键安装命令（按原帖说明移除人为插入的“删”字）：

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

初始化时原帖选择 QuickStart、硅基流动 Custom Provider，API Base URL 为 `https://api.siliconflow.cn/v1`，Endpoints 选 OpenAI-compatible，模型示例为 `Pro/MiniMaxAI/MiniMax-M2.5`。原帖称自定义 API 的默认上下文 tokens 为 4096，并报告首次对话提示异常；之后在 `C:\Users\admin\.openclaw\openclaw.json` 中把模型参数改为：

```json
"contextWindow": 200000,
"maxTokens": 8192
```

其中 `admin` 是原帖示例用户名，应替换为实际 Windows 用户目录名。原帖报告修改后再次提问成功；未提供 OpenClaw 版本号或独立复测。

## cpolar 远程访问 OpenClaw
原帖将 OpenClaw 本地端口 `18789` 映射为 HTTP 隧道，访问时遇到 `origin not allowed`。其给出的命令示例是：

```powershell
openclaw config set gateway.controlUi.allowedOrigins "[\"https://1cf1b8b8.r1.cpolar.top\"]" --strict-json
openclaw gateway restart
```

域名是帖中示例，实际需替换为自己的公网 origin。原帖后续描述要求在远程 Control UI 输入网关 token，并在终端批准设备配对：

```powershell
openclaw devices list
openclaw devices approve <requestId>
```

固定子域名案例把允许来源改为 `https://openclaw.cpolar.top`，再重启 gateway；示例配置命令：

```powershell
openclaw config set gateway.controlUi.allowedOrigins "[\"https://openclaw.cpolar.top\"]" --strict-json
openclaw gateway restart
```

固定子域名需先在 cpolar 预留并绑定隧道。原帖另展示 cpolar 映射 OpenList、HTML 服务和 TCP 3389 的用法，但重点操作参数分别是本地服务端口与协议：HTTP 映射 Web 服务，TCP 映射 RDP 3389。原帖提醒公网暴露 OpenClaw 时保护 token 和访问密码。

## 来源与边界
来源：酷安帖子 [狂揽230K Stars！2026神级项目OpenClaw，Windows一键部署，带你30分钟极速上手！](https://www.coolapk.com/feed/70473575)。配图未读取；原帖提到的 123 云盘分享地址没有实际 URL。版本、令牌授权和对话成功均为原帖报告；不要把示例 cpolar 域名视作有效服务地址。
