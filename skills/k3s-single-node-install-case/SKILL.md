---
name: k3s-single-node-install-case
description: 整理 K3s 的轻量化组件构成、安装脚本与原帖报告的 v1.30.4+k3s1 安装结果，并保留 Rancher 镜像脚本资源。
---

# Flexus X 上安装 K3s 单节点集群

[来源文章](https://www.coolapk.com/feed/61925095)记录了以下历史操作与配置（图片未读，未在本库实机复现）。

## K3s 组件概览

原帖将 K3s 描述为轻量级 Kubernetes 发行版：可打包为单个二进制文件，默认以 SQLite 作为存储，也支持 etcd3、MySQL 和 PostgreSQL；控制平面组件由单个二进制文件和进程管理。文章列出的内置组件包括 containerd、Flannel（CNI）、CoreDNS、Traefik（Ingress）、Klipper-lb（Service LoadBalancer）、网络策略控制器、local-path-provisioner，以及 iptables、socat 等主机工具。文章还称其要求现代内核与 cgroup 挂载，并支持 systemd 或 OpenRC 安装。以上为原帖的功能概述。

## 原帖安装命令与结果

原帖先给出官方安装脚本入口 `https://get.k3s.io`，并列出通用安装形式：

```sh
curl -sfL 查看链接 | sh -
```

该处链接在输入正文中被替换为“查看链接”，不能据此还原该行的精确命令文本。文章随后给出使用中国镜像的命令：

```sh
curl -sfL 查看链接 | INSTALL_K3S_MIRROR=cn sh -
```

对应的脚本资源可见为 <https://rancher-mirror.rancher.cn/k3s/k3s-install.sh>；正文中的“查看链接”与实际展开 URL 的对应关系按上下文识别，原帖报告使用 `INSTALL_K3S_MIRROR=cn`。不将被替换的命令片段伪装成原文逐字命令。

原帖安装日志报告：stable channel 解析到 `v1.30.4+k3s1`，下载的二进制安装到 `/usr/local/bin/k3s`，建立 `kubectl` 和 `crictl` 符号链接，创建 `/usr/local/bin/k3s-killall.sh` 与 `/usr/local/bin/k3s-uninstall.sh`，并建立 systemd 服务 `/etc/systemd/system/k3s.service`。因 `/usr/bin/ctr` 已存在，安装器跳过创建 `ctr` 符号链接；同时创建环境文件 `/etc/systemd/system/k3s.service.env` 并启动服务。

原帖建议用以下命令检查节点：

```sh
kubectl get node
```

其记录的输出为节点 `flexusx-ebe8` 状态 `Ready`、角色 `control-plane,master`、版本 `v1.30.4+k3s1`，并据此称单节点 Server 已运行。该输出是原帖报告，未独立验证；不能视作当前版本或其他实例的保证。

## 相关资源

- K3s 官方安装入口：<https://get.k3s.io>
- Rancher 中国镜像安装脚本：<https://rancher-mirror.rancher.cn/k3s/k3s-install.sh>

原帖标题还提到 Kuboard 图形化管理工具，但可读正文没有 Kuboard 的安装、配置或资源链接，相关内容无法提取。正文外另有 6 张图片未读取，评论未提供，历史评论读取未完成。
