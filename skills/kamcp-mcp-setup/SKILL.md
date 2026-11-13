---
name: kamcp-mcp-setup
description: "按原帖将 kamcp 接入 MCP 客户端，包含 pip/uv 安装、stdio/sse/http 启动方式、有效服务字段片段及工具版本边界。"
---

# kamcp 的 MCP 启动方式

帖子将 `kamcp` 描述为辅助 AI 查询和执行 KAM 命令的 MCP 服务器。未实机验证软件版本、客户端配置格式和实际工具能力。

安装示例：

```sh
pip install kamcp
```

若使用 uv：

```sh
uv tool install kamcp
```

帖子列出三种启动形式：

```sh
kamcp stdio
kamcp sse
kamcp http
```

其中推荐本地使用 `stdio`，不推荐 `sse`，并称 `http` 使用 streamable-http 方式。客户端配置示意为将命令设为 `kamcp`、参数设为 `["stdio"]`。配置文件的具体外层格式取决于 MCP 客户端，帖子未提供完整可直接复制的配置，也未解释网络模式的监听地址或认证设置。

相关先前说明见[原帖链接](https://www.coolapk.com/feed/69234886)。

## 安装前提和可复制字段

[PyPI 项目页](https://pypi.org/project/kamcp/) 在本次查阅时列出版本 0.1.5、Python >=3.12，并给出 `pip install kamcp` 与 `uv tool install kamcp`。这是包元数据，不代表已经在本机安装或测试全部客户端。

原帖服务条目的两个字段可写为有效 JSON 片段；具体应放入客户端要求的位置，不是所有客户端通用的完整配置文件：

```json
{"command":"kamcp","args":["stdio"]}
```

KAM 0.5.12 更新帖还报告新增 `kam_status` 工具，用于读取项目状态与检查结果；没有核对它是否包含在 PyPI 0.1.5 中，不把两个版本号强行对应。原帖未给出网络模式的监听地址、认证和权限配置。

来源：[kamcp 启动说明](https://www.coolapk.com/feed/69250473)、[kam_status 更新记录](https://www.coolapk.com/feed/69269555)。整理日期：2026-11-14。
