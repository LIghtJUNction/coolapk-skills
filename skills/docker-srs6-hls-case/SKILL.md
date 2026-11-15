---
name: docker-srs6-hls-case
description: 原帖记录的 Docker 部署 SRS 6、云安全组端口、FFmpeg 推流脚本及 HLS 播放地址。
---

# Flexus X 上运行 SRS 6 并以 HLS 推流播放

[来源文章](https://www.coolapk.com/feed/61925508)记录了以下历史操作与配置（图片未读，未在本库实机复现）。

## 服务与协议

原帖将 SRS（Simple Realtime Server）描述为开源实时媒体服务器，列出 RTMP、WebRTC、HLS、HTTP-FLV、SRT、MPEG-DASH 和 GB28181 等协议，并说明其采用发布推流、订阅播放模型。HLS 使用 m3u8 索引文件、TS 分片及可选的 key 文件。作者称其相较 WebRTC 延迟约 5–10 秒，但播放较流畅且载入后可拖动进度；这是原帖描述，未附测量条件。

## Docker 部署

原帖使用 SRS 6 镜像及 `conf/hls.conf` 启动：

```sh
docker run -it --name=srs -d -p 1935:1935 -p  8080:8080  --restart=always ossrs/srs:6 ./objs/srs -c conf/hls.conf
```

命令中的 `-p 1935:1935` 与 `-p  8080:8080` 间距按原文保留。原帖还指出可使用 `https.hls.conf` 配置 HTTPS，但本次示例使用普通 HTTP；文中称 HLS 不像 WebRTC 那样强制要求 HTTPS。

作者运行 `docker ps` 后报告容器处于 `Up`，宿主机映射了 TCP 1935 和 8080；输出还列出未映射到宿主机的容器端口 1985、5060、9000，以及 UDP 8000、10080。配置中的 `CANDIDATE="127.0.0.1"` 被要求改成云服务器弹性公网 IP；原帖没有展示修改后的配置文件内容。

## 安全组与测试

原帖要求在华为云 Flexus X 安全组中开放 TCP 1935（RTMP 推流）和 TCP 8080（HTTP/HLS 访问）。作者说明 WebRTC 需要更多端口，但未列出完整规则。`ufw` 状态检查结果为 `Status: inactive`；这不等于云安全组已开放端口，外网访问仍依赖安全组设置。

原帖给出的服务页面示例为 `http://ip:8080/`，其中 `ip` 是占位主机地址；称页面可查看服务状态及负载、网速、CPU、磁盘等信息。该地址是原文明确使用 HTTP 的 URL，照录协议。

### Linux 推流循环

原帖脚本逐字保留如下：

```sh
for((;;)); do \
    ./objs/ffmpeg/bin/ffmpeg -re -i ./doc/source.flv \
    -c copy \
    -f flv rtmp://192.168.1.170/live/live200; \
    sleep 1; \
done
```

此示例输入文件为 `./doc/source.flv`，推流地址为 `rtmp://192.168.1.170/live/live200`，每轮结束等待 1 秒。原帖没有说明该内网地址如何替换为实际服务器地址。

### Windows 推流循环

```bat
@echo off
:loop
.\ffmpeg.exe -re -i test.mp4 -vcodec h264 -c copy -f flv rtmp://192.168.1.170/live/live200
timeout /t 1
goto loop
```

该示例使用当前目录的 `test.mp4` 和 `ffmpeg.exe`，推送到同一 RTMP 示例地址。

HLS 播放地址示例（原帖给出的 HTTP URL）：

- `http://ip:8080/live/live200.m3u8`

其中 `ip` 需替换为弹性公网 IP。原帖称在 SRS 播放器中打开该地址进行播放测试，但没有提供测试画面或独立验证结果。

## 证据边界

本文按原帖记录 SRS 6、配置文件名、端口、脚本和示例地址。图片内容未读取，评论未提供；原帖中的具体配置编辑界面及播放器结果因此未纳入。部署成功与延迟描述均为原帖报告。

## 补充案例

- [Flexus X 上 Docker 部署 SRS6 与 WebRTC/RTMP 互转配置](references/flexus-x-srs6-webrtc-rtmp.md)
