---
name: gitlab-container-deployment-case
description: 参考历史 GitLab CE 容器部署案例，核对持久化目录、端口映射、外部 URL 和初始化状态。
---

# GitLab CE 容器部署案例

[来源文章](https://www.coolapk.com/feed/61925666)在 Ubuntu 上将 GitLab CE 的配置、日志和数据分别绑定到宿主目录，并映射宿主 `30080`、`30081`、`30082` 到容器 `80`、`443`、`22`。其配置涉及 `external_url`、SSH 主机名和 `gitlab_shell_ssh_port`，修改配置后在容器内运行 `gitlab-ctl reconfigure`。这只是历史案例，不是已验证的当前版本安装教程。

复查类似部署时，应核对以下关系：

- 确认数据、配置和日志目录持久化，并考虑备份和恢复；容器运行状态的 `health: starting` 仅表示仍在启动，不能当作服务健康。
- 外部访问 URL、反向代理、TLS、宿主映射端口和 GitLab 生成的克隆地址必须彼此一致。原帖映射 HTTP 到宿主 30080，但示例 `external_url` 未包含该端口，也未说明反向代理，因此不能直接复制作为完整配置。
- SSH 发布端口与 `gitlab_shell_ssh_port` 应对应；网络访问还受云安全组、宿主防火墙及实际监听地址共同影响。
- 原帖使用 `latest` 标签和 `--privileged=true`，没有解释为何需要特权模式；它们不应被当作生产部署的默认要求。原文的离线 Docker 包下载目录是 [Ubuntu Jammy amd64 目录](https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/)，实际包版本、依赖、架构和完整性需另行核验。

原帖图片未读取，配置未在本库运行；部署具体版本时应核对该版本的官方要求和升级路径。
