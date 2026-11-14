---
name: rancher-kubernetes-docker-case
description: 记录文章给出的服务器准备、离线 Docker 安装、Rancher 容器启动、安全组开放和自定义集群注册步骤；操作结果是原帖报告，未独立验证。
---

# Flexus X 上以 Docker 部署 Rancher 并创建 Kubernetes 集群

[来源文章](https://www.coolapk.com/feed/61930881)记录了以下历史操作与配置（图片未读，未在本库实机复现）。

## 环境与准备

原帖记录在华为云 Flexus X 实例上操作，使用 SSH 工具连接服务器。登录前先在云控制台重置服务器密码。文章中的磁盘检查结果为：根分区 `/dev/vda1` 总计 99G、已用 4.2G、可用 90G；`ufw status` 显示 `Status: inactive`。该结果只代表原帖所用实例当时的状态。原帖据此通过云平台安全组配置外部入站端口。

## 安装 Docker

原帖称当时无法直接在线安装 Docker，改为从 Docker 官方 Ubuntu Jammy amd64 软件包目录下载离线 `.deb` 包，上传服务器后执行：

```sh
dpkg -i *.deb
```

目录链接：<https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/>

原帖报告安装后版本为 `Docker version 27.1.2, build d01f264`。未提供具体下载的 `.deb` 文件名或校验信息。

## 启动 Rancher

原帖使用阿里云镜像仓库中的 Rancher `v2.8.4` 镜像：

```sh
docker run -d --restart=unless-stopped \\
  -p 80:80 -p 443:443 \\
  --privileged \\
  -e CATTLE_SYSTEM_DEFAULT_REGISTRY=registry.cn-hangzhou.aliyuncs.com \\
  --name rancher \\
  registry.cn-hangzhou.aliyuncs.com/rancher/rancher:v2.8.4
```

检查容器：

```sh
docker ps
```

原帖截图文字转录报告容器处于 `Up` 状态，宿主机映射了 TCP 80 和 443。要从公网访问，还需在华为云 Flexus X 安全组中放行 80、443，并将安全组关联到实例。原帖随后以 `弹性公网IP:80` 打开 Rancher。

## 初次登录与创建集群

原帖给出的初始密码获取方式是查看 Rancher 容器日志：

```sh
docker logs  58a6c708cd8a  2>&1 | grep "Bootstrap Password:"
```

这里的容器 ID `58a6c708cd8a` 是原帖当时输出中的值，应按实际环境替换。随后按界面提示设置密码。

创建集群时，原帖选择“自定义”集群，将容器网络设为 `flannel`，勾选跳过 TLS 验证，然后复制 Rancher 界面生成的注册命令到目标服务器执行。原帖所示命令包含临时公网 IP、注册 token 与 CA checksum；这些是实例专属连接参数，本文不复述其可用凭据。命令结构为：使用 `curl --insecure -fL` 获取 `/system-agent-install.sh`，再通过 `sudo sh -s -` 传入 `--server`、`--label 'cattle.io/os=linux'`、`--token`、`--ca-checksum` 和 `--etcd --controlplane --worker`。原帖日志报告 agent 下载、CA 检查、Rancher 连接测试及 systemd 服务启用成功；这是作者提供的运行记录，不构成独立复测。

## 证据边界

原文含 5 张图片，图片内容未读取；命令与步骤依据可见正文转录。评论未提供，历史评论也未完成读取。文章没有给出离线包文件名或 Kubernetes 集群最终状态的可读命令输出。
