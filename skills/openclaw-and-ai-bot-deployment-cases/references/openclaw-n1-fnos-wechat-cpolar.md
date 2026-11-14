# OpenClaw 在 ARM 飞牛 NAS 上的部署记录

## 适用环境与证据范围
原帖以斐讯 N1、ARM 版飞牛 OS（fnOS）为例，并称流程适用于其他 ARM 版飞牛 NAS。要求设备已刷入 ARM 版 fnOS、网络稳定，建议网线连接；操作端电脑与 NAS 在同一局域网。原帖报告完成了 OpenClaw 安装、微信机器人连接及 cpolar SSH 隧道连接；未提供可复核的命令输出或硬件测试数据，图片内容未纳入本记录。

## SSH 与 Node.js、Git
在 fnOS 设置中开启 SSH，从 Windows 管理员 PowerShell 连接（用户名和地址需替换为自己的值）：

```sh
ssh n1@192.168.50.228
```

在 NAS 终端创建当前用户名家目录并调整属主和权限：

```sh
sudo mkdir -p /home/$(whoami) && sudo chown -R $(whoami):$(id -gn) /home/$(whoami) && sudo chmod 755 /home/$(whoami) && ll /home
```

初始化 `.bashrc`：

```sh
cd ~ && touch .bashrc && ls -a
```

原帖 NVM 安装命令中人为插入了“删”字；按原帖明确说明逐字移除后，命令为：

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

使配置生效并安装 Node.js LTS：

```sh
source ~/.bashrc
nvm install --lts
node -v
```

原帖也给出手动加载 NVM 的替代方式：

```sh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # 加载 nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

检查 Git；若尚未安装，原帖在该 NAS 系统中使用：

```sh
git --version
sudo apt install git -y
```

## 安装与初始化 OpenClaw
查看 npm 版本列表末尾十项：

```sh
npm view openclaw versions | tail -n 10
```

原帖推荐 `2026.3.13`，同时表示也可安装最新版本；这是原帖当时的版本建议，不代表当前最新版本或兼容保证。安装命令：

```sh
npm i -g openclaw@2026.3.13
```

运行新手引导并安装 daemon：

```sh
openclaw onboard --install-daemon
```

原帖引导选择 QuickStart、模型供应商 Custom Provider、接口兼容选项 OpenAI-compatible，并以硅基流动为例。其 API Base URL 原文写作 `https://api.siliconflow.cn/v1`（移除原文明确插入的“删”字）。模型示例为 `Qwen/Qwen3.5-397B-A17B`；模型别名可留空或自定。channel 与 Search provider 可跳过，skills 选择 No；hooks 按需选择，原帖选择全部启用。API Key 应从服务商账户自行取得，原帖没有公开账户密钥。

## 微信接入
原帖在 OpenClaw 插件位置复制安装命令，并在 NAS 终端执行：

```sh
npx -y /openclaw-weixin-cli install
```

原帖报告命令完成后出现二维码，手机微信扫码连接机器人账号，随后可以在微信对话。原帖没有给出该 CLI 的独立项目链接、版本、二维码图片内容或复测记录。

## cpolar 公网 SSH
原帖提供的一键安装命令（移除明确标记的“删”字）：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

原帖称服务正常时状态显示为运行中；cpolar Web UI 地址示例为 `http://192.168.50.228:9200/`。默认 SSH 隧道指向 TCP 22 端口，示例公网地址为 `tcp://1.tcp.nas.cpolar.cn:11225`。其 SSH 命令中的用户名被传输层替换为占位符，无法恢复真实用户名；原帖命令形态如下，需将用户名和端口按自己的隧道配置填写：

```sh
ssh <原文用户名字段已脱敏> -p 11225
```

固定 TCP 地址是账号各自预留的示例，原帖展示 `6.tcp.cpolar.top:14729`，并将 SSH 隧道端口类型改为固定 TCP 端口。相应连接示例中的用户名同样已脱敏：

```sh
ssh <原文用户名字段已脱敏> -p 14729
```

公网映射 SSH 会使 NAS 登录面暴露在公网。原帖建议使用强密码；该建议不替代额外访问控制。上述隧道域名和端口是帖中示例，不是通用固定值。

## 来源
来源：酷安帖子 [N1飞牛Nas成功部署OpenClaw接入微信，看这一篇就够了（避坑指南）](https://www.coolapk.com/feed/71240646)。教程中刷机教程、硅基流动注册页面和 cpolar 官网只以文字提及，未给出可解析的具体 URL；配图共 9 张，未读取。
