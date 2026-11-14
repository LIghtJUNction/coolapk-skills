# AstrBot 与 NapCat 部署参考

## 架构与项目
AstrBot 是机器人框架，负责 AI 模型和机器人逻辑；NapCat 基于 NTQQ 并提供 OneBot v11 接口，负责 QQ 连接。教程采用 Docker，通过 OneBot 对接。项目仓库：

- AstrBot：<https://github.com/AstrBotDevs/AstrBot>
- NapCatQQ：<https://github.com/NapNeko/NapCatQQ>

上述能力描述来自原帖，未独立核对项目当前状态。

## 部署
Windows 11 PowerShell 执行：

```powershell
irm https://gitee.com/jun-wan/script/raw/master/astrbot/deploy_astrbot.ps1 | iex
```

原帖称脚本会检查 Docker、创建目录、下载 `astrbot.yml` 并启动容器。Linux 官方 compose 流程：

```sh
mkdir astrbot
cd astrbot
wget https://raw.githubusercontent.com/NapNeko/NapCat-Docker/main/compose/astrbot.yml
sudo docker compose -f astrbot.yml up -d
```

Linux 一键脚本：

```sh
curl -fsSL https://gitee.com/jun-wan/script/raw/master/astrbot/deploy_astrbot.sh | bash
```

文章示例本机界面为 `http://localhost:6185`（AstrBot）和 `http://localhost:6099/`（NapCat）。NapCat Web 登录页需要 Token；若控制台记录已清，可在 Windows PowerShell 查容器日志：

```powershell
docker logs napcat | Select-String "Token"
```

登录 NapCat 后扫码登录机器人 QQ。AstrBot 初始示例账号密码均为 `astrbot`，首次登录要求修改账号信息。创建机器人时选择“QQ个人号”并启用；教程建议用另一个 QQ 账号发送消息验证通信。

## 模型、人设与插件
在 AstrBot 的模型提供商设置中新增 DeepSeek，填入 API Key 并使用“测试”。API Key 只在服务端创建时显示一次，勿写入公开配置。新建人格时填写人格 ID 与系统提示词，可用 `/persona` 查询、`/persona list` 查看列表；切换示例命令：

```text
/persona <你要切换的人格设定ID名称>
/reset
```

切换人格后可用 `/reset` 清除上下文。配置文件中的“流式回复”开关可启用流式输出；对不支持流式的情况，原帖称会实时分段回复。插件从插件市场安装；文章以点歌插件为例，测试命令：

```text
/点歌 恋人（你想要点歌的歌曲名字）
```

## MCP 配置示例
文章以 12306 MCP 为例，给出服务器配置：

```json
{
  "mcpServers": {
    "12306-mcp": {
      "command": "npx",
      "args": ["-y", "12306-mcp"]
    }
  }
}
```

在 AstrBot 的 MCP 页面同步服务器，进入 ModelScope 项目配置页复制配置，新增服务器后测试并保存。文章报告测试时返回车次、出发/到达时间、座位和价格；没有提供接口版本及截图内容。

## cpolar 远程访问
Windows 安装 cpolar 后可用 `cpolar version` 查看版本，Web UI 示例 `http://127.0.0.1:9200`。在隧道列表编辑 `website` 隧道并指向 AstrBot 的 6185 端口，文章示例隧道名 `astrbot-6185`。随机域名约 24 小时变化；固定二级子域名需要先预留，再将隧道域名类型改为二级子域名。免费/付费和域名稳定性描述是原帖发布时的信息。

## 证据范围
原帖提供的命令链接已按其明确说明去除插入的“删”字，仓库地址使用正文链接上下文中的完整 URL。图片、视频、评论未读取；所有部署成功描述均为原帖报告，未独立验证。原帖列出 123 云盘人格资源提取码 `1314`，但没有分享 URL，因此无法据此下载。

来源：[68880036](https://www.coolapk.com/feed/68880036)（图片与评论未读，未在本库实机复现）。

