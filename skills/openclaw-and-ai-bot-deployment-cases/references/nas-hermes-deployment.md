# 飞牛 NAS 上部署 Hermes 并接入微信与 cpolar

## 适用范围与证据边界
原帖以 RK3566 小主机、飞牛 NAS ARM 版和 Docker 为例。文中报告完成 Hermes 初始化、DeepSeek 对话、微信消息接入和公网 Web UI 访问；没有独立复测。图片未读取，截图中的界面细节不纳入本文。评论未提供。

## Hermes 部署与 DeepSeek 初始化
1. 在飞牛 NAS 桌面确认 Docker 服务已开启；在系统设置中启用 SSH。
2. 从 Windows 终端连接 NAS，再切换 root：

```sh
ssh n1@192.168.50.212
sudo -i
```

3. 原帖脚本链接中明确说明应删除插入的“删”字。按该规则恢复后的地址为 `https://gitee.com/jun-wan/script/raw/master/fnos-hermes/fnos-hermes.sh`。原命令：

```sh
curl -fsSL https://gitee.com/jun-wan/script/raw/master/fnos-hermes/fnos-hermes.sh -o /tmp/fnos-hermes.sh && chmod +x /tmp/fnos-hermes.sh && /tmp/fnos-hermes.sh
```

4. 菜单选择 Hermes Agent 安装；初始化向导选择 Quick setup 和 DeepSeek，填入 DeepSeek API Key，模型使用默认 `deepseek-chat`。消息平台步骤先跳过；初始化后运行 Hermes，原帖以对话测试，再输入 `/exit` 退出。
5. 原帖给出的局域网 Web UI 地址为 `http://192.168.50.212:9119`。

## 微信接入
重新运行同一部署脚本，在菜单输入 `6` 配置聊天平台网关，选择 WeiXin 并按提示扫码登录。配对审批模式和禁用群聊消息均按默认项，随后选择 Done 返回菜单，输入 `8` 重启 Hermes 服务。

原帖示例的配对审批命令为：

```sh
hermes pairing approve weixin QKHBWHAJ
```

然后在菜单输入 `10` 执行容器内 Hermes 原生命令，并输入审批命令。该审批码是原帖中的示例值；实际使用时应以待审批的配对请求所给信息为准。原帖报告微信用户获批后可以对话。

## cpolar 远程访问
在 NAS SSH 终端安装 cpolar 并查看服务状态：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

`https://get.cpolar.sh` 是根据原帖“删除删字”的明确说明恢复的 URL。原帖以 `active (running)` 作为服务正常运行的状态标志；其 Web UI 地址示例为 `http://192.168.50.212:9200`。

在 cpolar Web UI 的“隧道管理 > 隧道列表”编辑 `website` 隧道，使其指向 Hermes 的本地 9119 端口；从在线隧道列表取得生成的 HTTP 或 HTTPS 公网地址。原帖称免费随机域名约每 24 小时变化。固定二级子域名需要升级付费套餐：在官网预留子域名，再将隧道域名类型改为二级子域名并填入预留名称。

为公网 Web UI 配置基础认证：编辑隧道的高级设置，在 HttpAuth 项填写 `用户名:密码`，英文冒号分隔。原帖展示的 `admin:123456` 仅为格式示例，并明确不建议使用弱密码；应设置独立强密码。HttpAuth 是隧道层认证，不等同于 Hermes 自身账号系统。

## 来源
- 原帖：<https://www.coolapk.com/feed/71755940>
- Hermes 部署脚本：<https://gitee.com/jun-wan/script/raw/master/fnos-hermes/fnos-hermes.sh>（删除原帖明确插入的“删”字后恢复）
- cpolar 安装脚本：<https://get.cpolar.sh>（删除原帖明确插入的“删”字后恢复）
