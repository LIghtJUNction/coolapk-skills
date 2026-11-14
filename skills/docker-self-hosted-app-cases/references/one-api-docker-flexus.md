# 使用 Docker 在 Flexus X 上运行 one-api v0.6.0

## 原帖环境与功能介绍

原帖介绍 one-api 为 OpenAI 接口管理与分发系统，列出的接入包括 Azure、Anthropic Claude、Google PaLM 2/Gemini、智谱 ChatGLM、百度文心一言、讯飞星火、阿里通义千问、360 智脑及腾讯混元。所列功能包括镜像与第三方代理配置、多渠道负载均衡、stream 流式响应、多机部署、令牌过期时间/额度/IP 范围/模型访问限制、兑换码管理、渠道及用户分组、倍率设置、额度明细与邀请奖励。这些是原帖发布时的功能描述，不保证适用于其他版本。

## 拉取到本地的镜像记录

原帖 `docker images` 输出显示使用镜像：

```text
swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/justsong/one-api:v0.6.0
```

## 运行容器

原帖以 root shell 执行：

```sh
docker run --name one-api -d --restart always -p 3002:3000 -e TZ=Asia/Shanghai -v /root/data/one-api:/data swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/justsong/one-api:v0.6.0
```

参数含义按原命令：容器名 `one-api`，后台运行，重启策略 `always`，宿主机 TCP 端口 `3002` 映射到容器端口 `3000`，时区环境变量 `TZ=Asia/Shanghai`，宿主目录 `/root/data/one-api` 挂载到容器 `/data`。

原帖输出的容器状态为 `Up`，端口显示 `0.0.0.0:3002->3000/tcp` 及 `[::]:3002->3000/tcp`。这是原帖运行记录，未独立验证。

## 外部访问

原帖要求在云服务器安全组准入规则中加入 3002 端口。正文没有写出该规则的协议、来源网段或具体控制台配置，也没有记录从外部访问的验证结果。开放范围应按实际访问需求配置；不能从容器运行状态推断公网访问已通。

## 证据边界

来源：[原帖](https://www.coolapk.com/feed/61832418)。图片 5 张未读取，评论未读取。正文没有提供上游项目地址或分享链接；镜像地址是正文明确记录的完整镜像引用，不推断其仓库页面 URL。
