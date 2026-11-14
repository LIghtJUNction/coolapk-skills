# OpenClaw + cpolar 使用记录

## cpolar 本地管理界面
原帖给出的本地 Web UI 地址为 `http://127.0.0.1:9200`。cpolar 隧道可映射 HTTP 服务或 TCP 服务。文中示例包括：OpenList 服务、在本地启动的贪吃蛇 HTML 服务、Windows RDP TCP 3389，以及 OpenClaw Web UI 端口 `18789`。这些是不同服务的示范场景，端口不可互相套用。

原帖报告的示例公网地址包括：

- OpenList：`https://54e9cc9b.r3.nas.cpolar.cn`
- 贪吃蛇页面：`https://62f62d1.r3.nas.cpolar.cn/snake.html`
- RDP：`tcp://1.tcp.nas.cpolar.cn:11748`，Windows 远程桌面客户端填写时去掉 `tcp://`，使用主机与端口。

原帖正文中的 OpenList 局域网 URL 被写成“查看链接”，无法恢复，因此不列出。公网示例地址是原帖记录，不保证仍可用。

## OpenClaw Control UI 的来源校验
将 OpenClaw 的 `18789` 端口映射到公网后，原帖遇到 `origin not allowed`，并称需将穿透域名加入 `gateway.controlUi.allowedOrigins`。Windows CMD 示例命令如下，域名按原帖明确插入“删”字的规则恢复：

```sh
openclaw config set gateway.controlUi.allowedOrigins "[\"https://75de4fe1.r1.cpolar.top\"]" --strict-json
openclaw gateway restart
```

这是来源帖子中的示例域名，实际使用应填自己的隧道域名。之后原帖报告页面从 Origin 错误转为要求网关 token，并要求在 Control UI 输入 token；连接时若提示设备需要配对批准，在终端运行：

```sh
openclaw devices list
openclaw devices approve <requestId>
```

`<requestId>` 必须替换为 `devices list` 输出中的实际请求 ID。原帖报告批准后 UI 健康状态正常并可对话。

## 固定二级子域名
原帖称免费随机域名约每 24 小时变更；通过 cpolar 预留二级子域名，再编辑隧道域名类型并填写预留名称。示例名称为 `openclaw`，示例域名为 `openclaw.cpolar.top`。更新固定域名后，仍需把新的 HTTPS origin 写入配置并重启网关：

```sh
openclaw config set gateway.controlUi.allowedOrigins "[\"https://openclaw.cpolar.top\"]" --strict-json
openclaw gateway restart
```

之后仍需提供网关 token 并批准设备配对。子域名是否可预留取决于账号和可用性。

## 安全与证据边界
OpenClaw 被原帖描述为可读取文件和控制鼠标键盘的本机代理；把管理界面映射到公网会扩大可访问范围。原帖特别提醒保护网关 token 与访问密码。帖子中的连接成功、页面错误变化及远程访问均为作者报告，没有独立测试证据。配图未读取。

来源：酷安帖子 [部署了OpenClaw不知道怎么玩？OpenClaw + cpolar解锁它的 N 种正确打开方式！](https://www.coolapk.com/feed/70542257)。正文提及的 OpenList 局域网链接在采集文本中被隐藏；不据此猜测其主机地址。
