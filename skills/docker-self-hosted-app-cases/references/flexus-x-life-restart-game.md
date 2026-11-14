# Flexus X 部署网页游戏“人生重启”的运行记录

## 环境检查

原帖要求服务器有 Node.js 12.0 及以上和 Git。检查命令：

```sh
git --version
node -v
```

若 Git 未安装，原帖给出的发行版命令为：

```sh
sudo apt update
sudo apt install git
```

```sh
sudo yum install git
```

```sh
sudo dnf install git
```

```sh
sudo pacman -S git
```

原帖还记录使用 nvm 时可选择并设定默认 Node.js 12：

```sh
nvm use 12
nvm alias default 12
node -v
```

只有一个 Node 版本时，作者称无需执行切换命令；有多个版本时可用上述命令。正文未提供 Node.js/nvm 安装步骤、游戏仓库地址或克隆命令。

## 启动与端口

原帖在项目目录运行：

```sh
npm run dev
```

随后以服务器 IP 的 8080 端口访问：`你的服务器IP:8080/view/index.html`。访问失败时，作者指出需在云服务器安全组放行 8080；若装有宝塔面板，还需在宝塔“安全”中添加入站端口 8080 规则。

终端关闭后进程退出时，原帖建议修改 `package.json` 增加脚本：

```json
"start": "nohup npm run dev &",
```

然后执行：

```sh
npm run start
```

作者报告这样可在关闭 SSH 终端后继续访问。此方式没有提供进程管理、日志处理或重启策略，且项目原始 `package.json` 结构未给出，片段需放在正确的 `scripts` 对象内。

## 来源与边界

来源： https://www.coolapk.com/feed/61769938 。原帖是个人部署记录，未提供项目公开地址、完整安装命令或独立复测。正文写有 Node.js 12 及以上要求，另展示 `nvm use 12` 示例；按原帖保留，不推断更高版本兼容性。
