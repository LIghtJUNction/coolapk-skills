# Flexus X 上以 Docker Compose 部署 Jitsi Meet

## 环境记录

原帖记录服务器根文件系统约 99G，执行 `df -h` 的 `/dev/vda1` 行显示 `99G  4.2G  90G  5% /`；执行 `ufw status` 得到 `Status: inactive`。因此原帖将入站放行交由华为云安全组处理。该状态是作者当时的实例记录，不代表其他实例默认配置。

## 安装 Docker 与 Compose

原帖以 Ubuntu Jammy amd64 为例，从 Docker 的 Jammy amd64 stable 包目录下载 `.deb` 包，上传服务器后执行：

```sh
dpkg -i *.deb
```

记录的 Docker 版本为 `27.1.2, build d01f264`。Docker 包目录： https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/

从 Docker Compose 标签页 https://github.com/docker/compose/tags 获取 `docker-compose-linux-x86_64`，上传后执行：

```sh
sudo cp -f docker-compose-linux-x86_64 /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo docker-compose -v
```

原帖记录 Compose `v2.29.2`。这些版本是 2024 年原帖记录，不代表当前最新版。

## 部署 Jitsi

项目与版本资源：

- GitHub Releases API： https://api.github.com/repos/jitsi/docker-jitsi-meet/releases/latest
- 原帖实际使用的 stable-9646 ZIPball： https://api.github.com/repos/jitsi/docker-jitsi-meet/zipball/stable-9646
- 下载重定向后的归档地址： https://codeload.github.com/jitsi/docker-jitsi-meet/legacy.zip/refs/tags/stable-9646

原帖命令排版为 `wget $(curl -s 查看链接 | grep 'zip' | cut -d\" -f4)`；其中 URL 被采集文本替换为“查看链接”，不可据此补造完整命令。作者记录下载后文件名为 `stable-9646`，随后执行：

```sh
unzip stable-9646
cp env.example .env
./gen-passwords.sh
mkdir -p ~/.jitsi-meet-cfg/{web,transcripts,prosody/config,prosody/prosody-plugins-custom,jicofo,jvb,jigasi,jibri}
docker-compose up -d
```

`gen-passwords.sh` 用于生成 `.env` 安全部分所需密码。Compose 文件启动时出现 `version` 属性已废弃的警告，原帖记录服务仍开始拉取镜像。

原帖的 `docker ps` 记录显示 `web`、`jicofo`、`jvb`、`prosody` 容器运行；端口映射包括 Web 的 `0.0.0.0:8000->80/tcp`、`0.0.0.0:8443->443/tcp`，JVB 的 `0.0.0.0:10000->10000/udp`，以及仅绑定本机的 `127.0.0.1:8888->8888/tcp` 和 `127.0.0.1:8080->8080/tcp`。外网访问还需按实际部署配置华为云安全组；原帖后续安全组规则内容未包含在所给正文中。

## 来源与边界

来源： https://www.coolapk.com/feed/61721187 。原帖是作者的部署记录，记录容器启动状态但未提供独立测试或完整安全组配置。原帖介绍 Jitsi 的会议、音频、屏幕共享、聊天、Jibri 录制集成、WebRTC 浏览器访问和移动应用等功能；其“端到端加密”等概述未给出具体配置或验证步骤。
