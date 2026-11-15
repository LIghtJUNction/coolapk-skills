# Flexus X 上 Docker 部署 SRS6 与 WebRTC/RTMP 互转配置

本记录整理自 Ubuntu 环境（Docker `27.1.2, build d01f264`）离线安装 Docker 后部署 `ossrs/srs:6` 并修改 `https.docker.conf` 的社区操作记录；原帖含 5 张图片（未读取），未在本库实机复现。

## 1. 环境准备与离线 Docker 安装

- 原帖在 Ubuntu Jammy 环境下采用离线包安装 Docker（包目录参考：[Ubuntu Jammy Docker 稳定版仓库](https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/)）。
- 检查 `ufw status` 为关闭状态，端口放行通过云主机安全组配置。

## 2. 启动 SRS6 容器并导出配置

先设置 `CANDIDATE` 为服务器弹性公网 IP（示例中先占位为 `127.0.0.1`）：

```bash
export CANDIDATE="<弹性公网IP>"
docker run -it -d \
  -p 1935:1935 -p 1985:1985 -p 8080:8080 -p 1990:1990 -p 8088:8088 \
  --restart=always \
  --env CANDIDATE=$CANDIDATE \
  -p 8000:8000/udp \
  ossrs/srs:6 ./objs/srs -c conf/https.docker.conf
```

如需同时支持 WebRTC 与 RTMP 相互转换，原帖从容器复制出默认配置进行修改：

```bash
mkdir -p /usr/local/srs/conf
cd /usr/local/srs/conf
docker cp <容器ID>:/usr/local/srs/conf/https.docker.conf https.docker.conf
```

修改 `https.docker.conf` 中的 `http_remux` 与 WebRTC/RTMP 互转配置（具体配置项参考 SRS 官方文档：[RTMP to RTC](https://ossrs.net/lts/zh-cn/docs/v4/doc/webrtc#rtmp-to-rtc)、[RTC to RTMP](https://ossrs.net/lts/zh-cn/docs/v4/doc/webrtc#rtc-to-rtmp)），随后删除旧容器并挂载修改后的配置文件重新启动：

```bash
docker stop <容器ID> && docker rm <容器ID>
docker run -it -d \
  -v /usr/local/srs/conf/https.docker.conf:/usr/local/srs/conf/https.docker.conf \
  -p 1935:1935 -p 1985:1985 -p 8080:8080 -p 1990:1990 -p 8088:8088 \
  --restart=always \
  --env CANDIDATE=$CANDIDATE \
  -p 8000:8000/udp \
  ossrs/srs:6 ./objs/srs -c conf/https.docker.conf
```

并在云主机安全组放行对应 TCP 端口（`1935`、`1985`、`8080`、`1990`、`8088`）及 UDP 端口（`8000`）。

## 证据边界

- 原帖未给出 `https.docker.conf` 修改前后的完整 diff，具体配置块需对照 SRS 官方 WebRTC 文档与图片确认。
- 来源：[Coolapk 帖子 61921045](https://www.coolapk.com/feed/61921045)。
