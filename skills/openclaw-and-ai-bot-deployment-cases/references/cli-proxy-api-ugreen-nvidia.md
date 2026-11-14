# CLI Proxy API 作为统一模型入口

## 作用与能力
CLI Proxy API 是位于上游模型服务和客户端之间的代理层。原帖称其提供 OpenAI、Gemini、Claude、Codex 兼容 API，并支持兼容客户端和 SDK、多账号访问、部分上游轮询、流式与非流式响应、工具调用、文本和图片输入，以及 OpenAI-compatible 上游。原帖还称较新版本内置管理页面，可通过 `management.html` 管理配置、API Keys、凭据、日志和用量；没有注明该 UI 功能的最低版本。

项目仓库：<https://github.com/router-for-me/CLIProxyAPI>。

## 绿联 NAS 部署
原帖以绿联 NAS 为例，先安装 Docker 应用并启用 SSH。PowerShell 示例连接命令：

```sh
ssh susu@192.168.50.99
sudo -i
```

在 NAS 上以 root 身份执行部署脚本。原文明确要求删除插入的“删”字，恢复后的命令为：

```sh
curl -fsSL https://gitee.com/jun-wan/script/raw/master/cliproxyapi_deploy/cpa_docker_deploy.sh -o /tmp/cpa_docker_deploy.sh && chmod +x /tmp/cpa_docker_deploy.sh && /tmp/cpa_docker_deploy.sh
```

脚本交互界面可选 `1` 或回车采用默认项；后续也可一路回车。原帖称部署完成后终端会显示管理地址和管理员登录信息。具体输出因环境而异，帖中未提供静态凭据。

## 添加 NVIDIA 上游
在 CLI Proxy API 的“AI 提供商”中添加 OpenAI 兼容提供商，填写 NVIDIA Base URL：

```text
https://integrate.api.nvidia.com/v1
```

在 NVIDIA Build 获取 API Key，在 CLI Proxy API 页面用 `/models` 获取模型列表；若未自动列出，可再次获取后手动选择。填入 API Key，选择模型做连接测试，原帖推荐 `gpt-oss-120b`，并报告页面提示密钥测试通过后保存。NVIDIA 模型和 Key 的可用性、额度及账号要求以服务当时状态为准；原帖没有给出固定模型清单或实测响应正文。

## OpenClaw 客户端
在 Windows CMD 或 PowerShell 中启动配置向导：

```sh
openclaw configure
```

原帖操作路径为 Local → Model → Custom Provider，并填入 CLI Proxy API 的统一地址、调用 API Key 和模型信息；具体界面输入值在原帖图片中，图片未读取，因此不补造。验证成功后选 Continue (Done) 保存。若当前会话未切换到新增模型，可重启 gateway，或在网页端切换模型后发送 `/new` 开新会话。原帖报告 OpenClaw 能返回回答并识别当前接入模型。

## cpolar 公网管理页面
原帖在 NAS 上执行：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

配置 HTTP 隧道时，隧道本地端口填写 `8317`，地区示例为 China Top。管理页路径为 `/management.html`；原帖示例 URL `https://你的公网地址/management.html` 中域名是模板值，不是可直接访问的实际地址。固定二级子域名需在 cpolar 预留后，再编辑隧道域名类型并填写对应子域名。原帖未展示实际保留的子域名或隧道 URL。

## 来源与边界
来源：酷安帖子 [我用绿联 NAS 搭了一个 AI 统一入口：CLI Proxy API 接入 NVIDIA大模型（保姆级教程）](https://www.coolapk.com/feed/71734366)。NVIDIA Build、CLI Proxy API 管理界面和 cpolar 官网仅以名称或操作步骤提及，未提供可解析的直达链接。原帖配图未读取；脚本 URL、仓库 URL 和占位管理页 URL 已按原文信息保留。
