---
name: docker-jenkins-gitee-cicd-case
description: 汇总原帖的 Portainer 容器运行方式、Jenkins/Gitee 触发配置、Docker 权限处理及 ASP.NET Core 监听端口修改记录。
---

# Docker、Jenkins 与 Gitee 的 C# Web API 自动部署记录

[来源文章](https://www.coolapk.com/feed/61832035)记录了以下历史操作与配置（图片未读，未在本库实机复现）。

## 环境与 Portainer

原帖环境为华为云 Flexus X 自定义实例，配置记为 4 vCPU、12 GiB，Ubuntu 镜像；作者称前一篇实验已安装 Git 与 Docker。Portainer 是用于管理 Docker/Kubernetes 的轻量级 UI。原帖运行命令如下：

```sh
docker run -d -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/volumes:/var/lib/docker/volumes portainer/portainer-ce
```

还需在云服务器安全组开放 9000 端口，再通过服务器 IP 与端口访问界面并首次设置账号密码。原帖称 LiveConnect 可查看宿主机、容器、镜像、网络和卷。

## Jenkins 任务与 Gitee Webhook

原帖记录的 Jenkins 配置流程：新建自由风格项目；在 Source Code Management 选 Git，填入 Gitee 仓库地址；创建凭据时填写 Username 与 Password，文章建议 Password 使用 Gitee 私人令牌；选择构建分支（示例为 `master`）；选择 `Build when a change is pushed to Gitee`；点击 Generate 取得 Webhook 地址与密码，并将两项配置到 Gitee；构建环境勾选每次构建前清理工作区；Build Steps 添加 `Execute shell` 管理镜像和容器。

Webhook 密码和仓库凭据属于实际账户凭据，原帖没有在可读正文中公开具体值，本文不补造。

## Jenkins 用户访问 Docker

原帖给出创建 Jenkins 用户并加入 `docker` 组的命令：

```sh
sudo adduser jenkins
sudo usermod -a -G docker jenkins
sudo systemctl daemon-reload
sudo systemctl restart docker
```

若上述处理不生效，原帖提出将 Docker socket 改为所有用户可读写：

```sh
sudo chmod 666 /var/run/docker.sock
```

原帖明确指出此权限会允许所有用户访问 Docker 守护进程并带来安全风险，并建议生产环境仅将特定用户加入 `docker` 组。`docker` 组成员通常拥有近似宿主机 root 的控制能力，采用哪种方式应结合主机权限边界评估；本文保留原帖步骤及其风险说明，不将其描述为安全加固。

## 应用端口配置

原帖发现容器端口映射为 5000，但 .NET 应用未配置监听端口时默认监听 8080，可能与 Jenkins 默认端口冲突，因此在 `appsettings` 中加入 Kestrel 配置并提交代码：

```json
{
  "Kestrel": {
    "EndPoints": {
      "Http": {
        "Url": "http://+:5000"
      }
    }
  }
}
```

正文中 URL 片段由资源记录保留为 `http://+:5000"`，其中末尾引号来自 JSON 字符串边界；按上下文可读配置值为 `http://+:5000`。原帖称 Gitee 推送后会触发 Jenkins 构建，之后需在云安全组开放 5000 端口，再访问接口。构建和接口访问成功是作者报告，未提供可独立复现的仓库、流水线脚本或请求响应记录。

## 证据边界

原帖依赖另一篇实验作为 Git/Docker 安装背景；其链接文字可见，但没有实际 URL，故不补造链接。图片共 6 张且未读取，评论未提供，历史评论读取未完成。
