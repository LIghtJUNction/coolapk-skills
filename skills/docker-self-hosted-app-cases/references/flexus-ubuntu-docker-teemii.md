# Ubuntu 22.04 上安装 Docker 与 Teemii 部署体验

## 环境记录

原帖记录 Flexus X 基础模式规格为 4 核、12G、100G、3M；Ubuntu 22.04.4 LTS，Docker 27.2.0，Docker Compose v2.29.2。内网 IP 属于个人环境信息，不在此复录。原帖另称安装使用 Ubuntu apt 仓库、配置 Docker 镜像加速，并介绍 Teemii；这是作者报告，未独立验证。

## Docker 安装（原帖命令）

```sh
# 安装必要工具包
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

# 添加Docker GPG秘钥
$ sudo curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

# 配置仓库源
$ sudo add-apt-repository \
      "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \
      $(lsb_release -cs) \
      stable"

# 更新apt包索引
$ sudo apt-get update

# 安装docker
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

`apt-get update` 更新包索引；后续命令安装 Docker CE、CLI 和 containerd。原帖的仓库命令将 `$` 提示符与命令一并展示，执行时应只输入命令本身。

## 镜像加速配置

原帖建议编辑 `/etc/docker/daemon.json`。正文中 JSON 片段的引号和逗号排版损坏，不能据此作为可直接使用的有效 JSON。列出的镜像地址为：

- `https://ustc-edu-cn.mirror.aliyuncs.com/`
- `https://docker.mirrors.ustc.edu.cn`
- `https://docker.nju.edu.cn`
- `https://ccr.ccs.tencentyun.com/`
- `https://docker.m.daocloud.io/`
- `https://dockerproxy.com`

应用配置后原帖执行：

```sh
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

## 原帖记录的检查结果

```text
root:~# docker -v
Docker version 27.2.0, build 3ab4256

root:~# systemctl status docker
Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
Active: active (running)

root:~# docker compose version
Docker Compose version v2.29.2
```

原帖还链接了 Docker 文档：[https://docs.docker.com](https://docs.docker.com)。

## Teemii 功能概述

原帖将 Teemii 描述为自托管漫画管理与阅读 Web 应用，支持 CBZ、CBR，浏览器阅读、阅读进度跟踪、元数据聚合、漫画推荐、多来源章节获取，以及与 Kitsu、AniList 同步阅读进度。原帖称该实例运行响应流畅，但搜索和漫画信息获取依赖的网络服务较慢；未提供部署命令、Teemii 项目链接或独立性能测试。

## 证据边界

来源：[原帖](https://www.coolapk.com/feed/61902973)。原帖图片 7 张未读取，评论未读取；部署 Teemii 的具体操作不能从现有正文恢复。链接是原帖明确给出的地址；没有对链接或软件进行下载验证。
