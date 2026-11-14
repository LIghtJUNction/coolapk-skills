---
name: docker-jenkins-jdk21-case
description: 原帖记录的 Flexus X 实例基础检查、Docker 离线安装和 Jenkins 容器启动步骤。
---

# Flexus X 上离线安装 Docker 并运行 Jenkins

[来源文章](https://www.coolapk.com/feed/61925581)记录了以下历史操作与配置（图片未读，未在本库实机复现）。

## 环境与检查

原帖描述在华为云 Flexus X 实例上操作，执行身份提示符为 `root`。先在云控制台重置服务器密码，再通过 SSH 客户端登录。作者报告根文件系统约 100 GB：

```text
root:~# df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           1.2G  1.1M  1.2G   1% /run
/dev/vda1        99G  4.2G   90G   5% /
tmpfs           5.7G     0  5.7G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           1.2G  4.0K  1.2G   1% /run/user/0
```

原帖报告 `ufw` 默认未启用：

```text
root:~# ufw status
Status: inactive
```

作者据此使用云安全组开放外部访问端口。`ufw` 状态和云安全组是不同层面的配置；该记录没有提供安全组规则截图或外部连通性验证。

## Docker 离线安装

原帖称通过下载 Ubuntu Jammy amd64 的 Docker `.deb` 包并上传服务器安装。资源目录：

- Docker Ubuntu Jammy amd64 稳定版包目录：[https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/](https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/)

在包含所需 `.deb` 文件的目录执行：

```sh
dpkg -i *.deb
```

作者记录的版本输出为：

```text
root:/home/docker# docker -v
Docker version 27.1.2, build d01f264
```

这是原帖在发布时报告的安装版本，不代表当前目录仍提供相同版本，也未给出包名、依赖处理过程或校验值。

## 运行 Jenkins JDK 21 镜像

拉取原帖使用的镜像标签：

```sh
docker pull jenkins/jenkins:latest-jdk21
```

原帖记录拉取成功，镜像摘要为 `sha256:32e6ea34c0944b8ede94b8bfaa3a268a3751c6514e5e7cf06d189ff77f4eb314`。启动前创建数据目录并设置权限，原文命令排版为一行：

```sh
mkdir /home/docker/jenkins/jenkins_home/ chmod +777 -R jenkins_home/
```

该行在 `mkdir` 与 `chmod` 之间没有命令分隔符，照录后不能作为有效的两个连续 shell 命令执行；原帖未明确给出正确分隔方式或 `chmod` 的工作目录，因此不擅自修正。

原帖容器启动命令（保留其换行损坏处）：

```sh
docker run --name=jenkins -d -p 31080:8080 -p 50000:50000 --privileged --restart=always -v /home/docker/jenkins/jenkins_home/:/var/jenkins_home/ -v /etc/localtime:/etc/localtime jenkins/jenkins:latest-jdk21
```

原始文本在第二个挂载参数的 `/etc/localtime` 前出现换行控制字符；上面仅按相邻文本恢复显示，不改变参数。容器端口 `8080` 映射到宿主机 `31080`，`50000` 映射到 `50000`；数据目录映射至 `/var/jenkins_home/`。原帖的 `docker ps` 输出报告容器处于 `Up`，并显示两组端口映射。之后还需在云安全组开放宿主机 `31080` 才能从外部访问管理页面。原帖没有给出 Jenkins 初始配置、解锁或插件安装步骤。

## 证据边界

本文整理单篇原帖中的环境、命令和作者所报告的输出；没有独立验证云资源状态、镜像可用性或部署结果。原帖标题和末尾还提到 SRS，但该篇正文没有给出可整理的 SRS 部署命令；相关 SRS 内容见另一篇技术文档。
