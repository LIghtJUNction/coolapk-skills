# Flexus X 上运行 Radarr 的 Docker Compose 示例

## 原帖环境

原帖报告环境为 Ubuntu 22.04.4 LTS、Docker 27.2.0（build 3ab4256）、Docker Compose v2.29.2。登录方式包括控制台 VNC、SSH 工具或 Linux/macOS 主机执行 SSH；示例 SSH 命令为 `ssh 弹性公网IP`，默认 SSH 端口为 22。安全组需按需开放服务端口。

原帖给出的 Docker 安装过程与仓库、镜像源 URL 被“查看链接”遮蔽的版本相同，详见《Ubuntu 上部署 Docker 与 DooTask 的原帖记录》；该文中被遮蔽的 URL 不能恢复为可执行安装步骤。

## Radarr 功能与服务配置

原帖将 Radarr 描述为电影库自动化管理工具，列出的功能包括电影搜索和导入、RSS 同步、下载失败后尝试其他版本、质量升级、元数据管理，以及与 SABnzbd、NZBGet、Kodi、Plex 的集成。官网：https://radarr.video/；项目仓库：https://github.com/Radarr/Radarr。

原帖的 `docker-compose.yml`：

```yaml
version: '3.5'

services:
  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - ./config:/config
      - ./movies:/movies #optional
      - ./downloads:/downloads #optional
    ports:
      - 7878:7878
    restart: unless-stopped
```

原帖说明 `volumes` 用于将容器路径映射到本地，`ports` 用于配置访问端口。示例将主机 7878 端口映射到容器 7878；配置、电影与下载目录分别映射到当前目录下的 `config`、`movies`、`downloads`。`movies` 与 `downloads` 行标有 `#optional`。容器以 UID/GID 1000 运行，时区为 `Etc/UTC`。

## 来源与证据边界

来源：酷安 feed 61665442。以上是原帖配置示例，未提供启动命令、索引器或下载客户端配置，也没有实际部署日志。图片未读取，评论未提供。

来源：[61665442](https://www.coolapk.com/feed/61665442)（图片与评论未读，未在本库实机复现）。

