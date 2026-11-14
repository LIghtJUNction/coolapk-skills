# Huawei Cloud EulerOS 上部署 Mininote

## 实验环境

原帖记录的环境为华为云 Flexus 云服务器 X 实例、Huawei Cloud EulerOS 2.0 (x86_64)，内网地址 `192.168.0.169`，Node.js `v18.20.4`。作者称公共镜像默认未安装 Node.js。该地址是原帖中的内网示例，不是可从公网访问的服务地址。

原帖将 Mininote 描述为自托管 Markdown 笔记应用，使用 Vue 3 和 Express。其功能说明包括个人笔记、日记、项目/学习记录及共享笔记等；“加密存储”等能力为原帖描述，未附实现细节或安全验证。

## 安装 Node.js 与 Yarn

下载包的原始 URL：

- Node.js v18.20.4 Linux x64 压缩包：<https://mirrors.aliyun.com/nodejs-release/v18.20.4/node-v18.20.4-linux-x64.tar.xz>

原帖命令和路径如下：

```sh
tar -xvJf node-v18.20.4-linux-x64.tar.xz
ln -s  /root/node-v18.20.4-linux-x64/bin/node /usr/local/bin/node
ln -s  /root/node-v18.20.4-linux-x64/bin/npm /usr/local/bin/npm
vim /etc/profile
```

在 `/etc/profile` 中加入并使配置生效：

```sh
export NODE_HOME=/root/node-v18.20.4-linux-x64/bin/
export PATH=$PATH:$NODE_HOME:/usr/local/bin/
source /etc/profile
```

原帖报告 `npm -v` 输出 `10.7.0`，然后设置 npm registry 并检查：

```sh
npm config set registry https://registry.npmmirror.com
npm get registry
npm install -g yarn
yarn -v
```

registry 原文在正文中被替换成“查看链接”，此处采用同帖外链清单中对应的完整地址 `https://registry.npmmirror.com`；尾斜杠版本 `https://registry.npmmirror.com/` 也出现在该清单中。作者报告 Yarn 版本为 `1.22.22`。

## 获取源码、安装依赖和构建

项目 Git 地址：<https://github.com/muety/mininote.git>

```sh
git clone https://github.com/muety/mininote.git
cd mininote/
yarn
cd webapp/
yarn
yarn build
```

原帖列出的源码结构包含服务端 `index.js`、`config.js`、`controllers`、`routers`、`utils`，以及 `webapp` 前端目录。先在项目根目录安装依赖，再进入 `webapp` 安装前端依赖并执行 `yarn build`。原帖报告根目录依赖安装完成、前端执行构建；没有提供构建产物或独立验证记录。

## 创建自签名证书并启动

```sh
mkdir /root/certs && cd /root/certs
openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out mininote.crt -keyout mininote.key
export HTTPS_CERT='/root/certs/mininote.crt'
export HTTPS_KEY='/root/certs/mininote.key'
cd /root/mininote/
yarn start
```

证书是 4096 位 RSA、自签名、SHA-256、有效期 365 天且不加密私钥的证书。环境变量只在当前 shell 环境中设置；原帖没有说明如何持久化变量或以守护进程运行服务。

## 防火墙、安全组和访问

原帖称该实例默认关闭 firewalld 和 SELinux；若未关闭，给出的命令是：

```sh
systemctl disable firewalld
systemctl stop firewalld
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
setenforce 0
```

原帖要求在云安全组入方向放行 TCP 3000，并用以下地址访问：

```text
https://弹性公网IP地址:3000/
```

将地址中的占位文字替换为实际弹性公网 IP。作者报告可访问首页并创建带密码的笔记本、新建笔记；原帖没有提供实际 IP、证书信任验证或独立复测结果。

来源：[61924013](https://www.coolapk.com/feed/61924013)（图片与评论未读，未在本库实机复现）。

