# Android OpenClaw 与飞书接入

## Android 运行架构与版本
上游项目通过 Flutter + proot Ubuntu 在 Android 上运行 OpenClaw Gateway，无需 root；Android 应用将 Ubuntu rootfs、Node.js 和 Gateway 初始化流程封装起来。原帖给出的最低要求为 Android 10（API 29）及以上，首次安装建议预留约 500 MB，初始化需联网下载 rootfs 和依赖约 500 MB，原帖估计约 5–10 分钟。示例测试机是 iQOO Neo 5；这是发帖设备，不代表唯一适配机型。

上游项目：<https://github.com/mithun50/openclaw-termux>
中文整合版仓库：<https://github.com/JunWan666/openclaw-termux-zh>

版本记录：原帖早期预热帖发布 v1.8.5 汉化版，含上游 v1.8.4 英文版和汉化 v1.8.5；后续教程称中文整合版由 v1.8.5 迭代到 v1.8.8，并提及 UI 优化、检查 OpenClaw 更新、编辑 JSON 配置等功能。教程截图对应 v1.8.7 期间编写，正文称当时 Releases 最新为 v1.8.8。此为历史版本信息，不代表目前版本。

后续教程提供的 123 云盘提取码为 `1314`，称分享中包含上游 v1.8.4 与中文整合版 v1.8.5–v1.8.8；预热帖也给出提取码 `1314`，称含 v1.8.4 与 v1.8.5。两帖均未提供实际云盘 URL，不能据提取码恢复分享链接。APK 架构选择说明：不确定时选 `universal`；较小包可选 `arm64-v8a`。

## 安装与运行
安装 APK 后，首次启动点击开始安装。原帖描述流程为下载并解压 Ubuntu RootFS、安装基础依赖、安装 Node.js、安装 OpenClaw Gateway。建议按手机系统设置允许后台运行，并可开启后台自启动；具体菜单因厂商系统不同。原帖称需使用备用机以降低误操作影响，并报告上游 v1.8.4 之前 issue 曾有误删文件问题、v1.8.4 之后已修复；未提供对应 issue 链接或验证材料。

首次配置在应用内进入 API Key 配置终端，选择风险确认 Yes、QuickStart、Custom Provider。原帖举例使用硅基流动，但本段未以可解析文本列出 API 地址或完整字段值；不从相邻帖子补入。channel 与搜索服务可跳过，skills 选择 No，hooks 可按需启用。安装 Gateway 后从应用控制台启动，原帖以日志和运行状态报告 Gateway 启动成功。

## 模型切换
原帖称可通过终端修改配置或运行 `openclaw configure`，该中文整合版还在“AI 提供商”页面加入自定义 OpenAI 兼容入口，便于切换模型。示例中转服务配置：

- Base URL：`https://codex.0u0o.com/v1`（按原帖明确的“删”字移除规则恢复）
- 模型名：`gpt-5.4`
- API Key：原帖曾公开一串疑似临时中转密钥。该值具有账户凭据性质，不在知识文档复述。

原帖称该中转额度每日 100 美元、有效至当月 29 日且可能并发受限，并报告切换后模型显示为 gpt-5.4；这些是当时活动和作者测试记录，不是当前服务承诺。若使用此类服务，应在服务方重新取得自己的凭据。

## 飞书机器人
在飞书开放平台创建企业自建应用并添加机器人能力。原帖提供的权限批量导入 JSON 为：

```json
{
  "scopes": {
    "tenant": [
      "aily:file:read",
      "aily:file:write",
      "application:application.app_message_stats.overview:readonly",
      "application:application:self_manage",
      "application:bot.menu:write",
      "cardkit:card:write",
      "contact:user.employee_id:readonly",
      "corehr:file:download",
      "docs:document.content:read",
      "event:ip_list",
      "im:chat",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat.members:bot_access",
      "im:message",
      "im:message.group_at_msg:readonly",
      "im:message.group_msg",
      "im:message.p2p_msg:readonly",
      "im:message:readonly",
      "im:message:send_as_bot",
      "im:resource",
      "sheets:spreadsheet",
      "wiki:wiki:readonly"
    ],
    "user": ["aily:file:read", "aily:file:write", "im:chat.access_event.bot_p2p_chat:read"]
  }
}
```

将应用的 App ID 与 App Secret 填入 Android 整合应用的飞书渠道配置，保存后重启 Gateway。飞书开放平台的事件订阅方式设为长连接；原帖提示 Gateway 未启动或渠道未添加时保存会失败。添加“接收消息”事件，创建并发布应用版本。用户首次给机器人发消息后，需在运行 Gateway 的终端按机器人返回的配对码执行：

```sh
openclaw pairing approve feishu <配对码>
```

原帖报告授权后飞书机器人能回复。权限 JSON 与配对过程为原帖内容；未验证其是否仍符合当前飞书平台权限目录。

## 来源与证据边界
来源：酷安帖子 [久等了，安卓Android部署OpenClaw并接入飞书详细图文教程来了！（上篇-附最新安装包）](https://www.coolapk.com/feed/70844051)，以及 [安卓（Android）部署OpenClaw教程预热（附安装包）](https://www.coolapk.com/feed/70607902)。原帖链接到的仓库 URL 已逐字符保留；云盘 URL 缺失。配图未读取，因此未从截图推断 APK 文件名、手机规格或配置字段。
