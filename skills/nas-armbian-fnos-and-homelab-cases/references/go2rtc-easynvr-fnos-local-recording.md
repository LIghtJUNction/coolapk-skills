# Go2RTC 与 EasyNVR 在飞牛 NAS 上本地录像

## 适用与证据范围
原帖演示 N1 上 ARM 版飞牛 NAS；称其他支持 Docker 的 NAS 也可参考。文章有 9 张图片，图中配置和页面无法复核。低内存设备上的性能、双容器占用改善及录像成功均为原帖报告，没有独立测量数据。

原帖提及 N1 可用 **453 版** ARM 固件，并称支持无线网卡驱动；123 云盘提取码 **1314**，但分享 URL 未提供。Docker 服务需先启动，原帖建议数据存储选外接存储。SSH 连接示例及提权：

```sh
ssh n1@192.168.50.228
sudo -i
```

## 部署 Go2RTC
在 NAS 终端执行原帖命令：

```sh
mkdir -p $(pwd)/go2rtc/config && touch $(pwd)/go2rtc/config/go2rtc.yaml && docker run -d --name go2rtc --restart always --network host -v $(pwd)/go2rtc/config:/config alexxit/go2rtc
```

检查容器：

```sh
docker ps
```

使用主机 IP 的 `1984` 端口访问 Web 页面。原帖外链示例 `http://192.168.50.228:1984` 是局域网地址。

在 Go2RTC 的 Add 页面展开 Xiaomi，填写小米账号信息并加载设备，复制流 URL。原帖给出的 YAML 示例：

```yaml
api:
  listen: ":1984"
  username: admin
  password: "password"

streams:
  xiaomi:
    - "you_xiaomi_url&subtype=2"
  xiaomi_4k:
    - "you_xiaomi_url&subtype=3"

xiaomi:
  "USER_ID": V1:EXAMPLE_TOKEN
```

用实际获取的 URL、用户 ID 和令牌替换示例值，保存并点击 `Save & Restart`，再从 stream 页面检查播放。示例中同时出现 API 用户名密码与 RTSP 地址写法，但字段关系在原帖中不完全一致；其 RTSP 示例：

```text
rtsp://宿主机IP:8554/xiaomi_4k
rtsp://admin:123456:8554/xiaomi_4k
```

第二条 URL 的认证语法看起来不完整，按原帖照录，不应视为已验证的可用格式。PotPlayer 可用 `Ctrl+U` 打开网络地址输入框测试。

## 部署 EasyNVR
先用 `df -h` 查看挂载和容量。原帖示例外置存储 239G，挂载路径 `/vol1`。创建目录：

```sh
mkdir -p /vol1/EasyNVR
cd /vol1/EasyNVR
```

Docker 命令（原帖屏蔽标记移除后）：

```sh
docker run -d --name easynvr --restart always --network host --log-opt max-size=50M -v "$PWD/configs:/app/configs" -v "$PWD/logs:/app/logs" -v "$PWD/temporary:/app/temporary" -v /vol1/EasyNVR:/app/r -v "$PWD/stream:/app/stream" registry.cn-shanghai.aliyuncs.com/rustc/easynvr_arm64:latest
```

通过 NAS IP 的 `10000` 端口访问。初始化时选择简体语言、确认局域网属性和开放端口，并创建管理员账号。将 Go2RTC 的 RTSP 地址作为拉流地址添加设备，选择 `PULL`。在通道录像计划中设为每天后可回放；支持按指定时间段下载是原帖描述。

原帖区分：RTSP 用于拉流观看，不支持云台控制；RTMP 主要用于推流，也不支持设备控制；ONVIF 除拉流外可支持设备发现、参数配置与云台控制，前提是摄像头支持相应功能。

## cpolar 远程访问
在 NAS 终端：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

原帖以服务正常启动为报告。Web UI 使用 NAS IP 的 `9200` 端口；编辑 `website` 隧道指向 EasyNVR `10000` 端口。原帖称 HTTP 隧道在线时有 HTTP 与 HTTPS 两个地址，免费随机域名约 24 小时变化；固定二级子域名需升级任意付费套餐。保留地区示例 `China Top`、名称 `easynvr` 后，在隧道设置中改为二级子域名。

## 来源
- 原帖：https://www.coolapk.com/feed/70396722
- Go2RTC Web UI 局域网示例：http://192.168.50.228:1984
- cpolar Web UI 局域网示例：http://192.168.50.228:9200/
- 资源边界：飞牛固件云盘链接缺失；图片未提供。
