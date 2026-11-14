# Huawei Cloud EulerOS 安装 Docker 与 containerd 启动故障记录

## 原帖给出的安装步骤

原帖建议先更新系统并安装仓库管理工具：

```sh
yum update -y
yum install -y yum-utils
```

添加华为云 Docker CE 仓库：

```sh
yum-config-manager --add-repo https://repo.huaweicloud.com/docker-ce/linux/centos/docker-ce.repo
```

该仓库文件的公开地址为 [https://repo.huaweicloud.com/docker-ce/linux/centos/docker-ce.repo](https://repo.huaweicloud.com/docker-ce/linux/centos/docker-ce.repo)。若添加仓库时遇到 XML 路径或版本不匹配，原帖给出的处理方式是下载仓库文件至 `/usr/local/docker`，把 `$releasever` 替换成 `7`，再从本地文件添加仓库：

```sh
wget https://repo.huaweicloud.com/docker-ce/linux/centos/docker-ce.repo -P /usr/local/docker
vi /usr/local/docker/docker-ce.repo
:%s/\$releasever/7/g
yum-config-manager --add-repo /usr/local/docker/docker-ce.repo
```

替换成 `7` 是原帖针对其遇到的仓库路径/版本问题所记步骤，不代表适用于所有欧拉版本或仓库配置。

安装命令：

```sh
yum install -y docker-ce docker-ce-cli containerd.io
```

原帖提醒可能仍有依赖包需要手动安装，但没有列出具体缺失包或命令。

## Docker 启动失败排查

启动服务：

```sh
systemctl start docker
```

原帖记录的错误为：

```text
start request repeated too quickly for docker.service
```

建议查看 Docker 服务日志：

```sh
journalctl -u docker.service
```

所示日志表明 `containerd.service` 找不到 `/usr/bin/containerd`，并以 `status=203/EXEC` 退出：

```text
containerd.service: Failed to locate executable /usr/bin/containerd: No such file or directory
containerd.service: Failed at step EXEC spawning /usr/bin/containerd: No such file or directory
containerd.service: Main process exited, code=exited, status=203/EXEC
```

这段证据定位到 containerd 可执行文件缺失；原帖未提供后续修复步骤或验证结果，不能据此断言安装组件后已恢复。

## 证据边界

来源：[原帖](https://www.coolapk.com/feed/61832502)。正文未明确记录具体 Huawei Cloud EulerOS 版本；图片 3 张未读取，评论未读取。仓库地址由原帖链接上下文恢复；未下载或校验仓库文件。
