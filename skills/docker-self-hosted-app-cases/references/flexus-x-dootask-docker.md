# Ubuntu 上部署 Docker 与 DooTask 的原帖记录

## 原帖环境与 Docker 安装

原帖报告的服务器环境为 Flexus X、Ubuntu 22.04.4 LTS、Docker 27.2.0（build 3ab4256）、Docker Compose v2.29.2。示例内网地址为文章所载的私网地址，非公开服务地址。

原帖命令如下。注意：输入材料将若干 URL 替换成“查看链接”，因此仓库地址和镜像列表无法逐字恢复；此处保留可读命令结构，不猜测被遮蔽内容。

```sh
# 安装必要工具包
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

# 添加Docker GPG秘钥
$ sudo curl -fsSL 查看链接 | sudo apt-key add -

# 配置仓库源
$ sudo add-apt-repository \
      "deb [arch=amd64] 查看链接 \
      $(lsb_release -cs) \
      stable"

# 更新apt包索引
$ sudo apt-get update

# 安装docker
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

# 添加docker镜像源
$ sudo vim /etc/docker/daemon.json
```

原帖展示的 `/etc/docker/daemon.json` 是 `registry-mirrors` 数组，但各项内容被替换为“查看链接”，不能形成可用配置。之后执行：

```sh
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

原帖以 `docker -v`、`systemctl status docker`、`docker compose version` 检查安装；记录到 Docker 27.2.0、服务 active (running)、Compose v2.29.2。另一段 DooTask 说明以 Docker 24.0.2、Compose v2.18.1 作为满足最低要求的版本示例；文中要求 Docker v20.10+、Compose v2.0+，建议硬件 2 核 4G 以上，支持环境列为 CentOS、Debian、Ubuntu、macOS、Windows。

## DooTask 部署

项目用途：开源 Web 协作与项目任务管理工具，原帖列出任务/项目管理、在线文档、思维导图、流程图、即时通讯、文件管理和任务日历等功能。

- 官网：https://dootask.com/site/zh/index.html
- GitHub 项目：https://github.com/kuaifan/dootask
- GitHub 克隆地址：https://github.com/kuaifan/dootask.git
- Gitee 克隆地址：https://gitee.com/aipaw/dootask.git

原帖给出的命令：

```sh
# 通过github克隆项目
git clone -b pro --depth=1 查看链接
# 或者你也可以使用gitee
git clone -b pro --depth=1 查看链接

cd dootask
# 自定义端口安装，如：./cmd install --port 80
./cmd install
./cmd repassword
./cmd port 80
```

两条克隆命令末尾的链接在输入材料中被遮蔽；上面列出的项目地址来自同文明确给出的链接上下文，但不把被遮蔽命令伪装成原文逐字 URL。`./cmd install --port 80` 是原帖给出的自定义端口示例；默认安装命令为 `./cmd install`。重置默认管理员密码使用 `./cmd repassword`；更换 HTTP 端口示例为 `./cmd port 80`。

## 安全组与证据边界

原帖说明安全组用于控制同一 VPC 内实例的访问策略，需按服务需求配置协议与端口，并在云控制台开放需要从外部访问的端口。不要因安装示例而无差别开放端口。

来源：酷安 feed 61904922。图片未读取；评论未提供。源文中的 GPG、APT 仓库、镜像列表和克隆命令链接有遮蔽，不能据此重建缺失 URL；原帖报告的 Docker 服务状态和版本未在本任务中独立验证。

来源：[61904922](https://www.coolapk.com/feed/61904922)（图片与评论未读，未在本库实机复现）。

