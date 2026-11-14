# N1 上用 Micam 与 EasyNVR 本地存储小米摄像头录像

## 适用与证据范围
原帖以 2GB 内存的 N1、飞牛 NAS 为例，部署 Micam 获取小米摄像头 RTSP 流，再由 EasyNVR 本地录像。原帖称 Micam 方案需要三件套并适用于 Docker Compose Linux，声称支持 ARM64；这些兼容性结论未独立核验。文章有图片但未提供，无法复核界面配置和实际存储位置截图。

## 项目与 Micam 部署
项目地址（原帖给出 HTTP）：http://github.com/miiot/micam

启用 NAS SSH 后，在电脑终端连接并提权：

```sh
ssh n1@192.168.50.227
sudo -i
```

运行原帖部署命令（删除原文明确插入的“删”标记）：

```sh
curl -fsSL https://gitee.com/jun-wan/script/raw/master/micam-deploy/deploy-micam.sh -o /tmp/deploy-micam.sh && chmod +x /tmp/deploy-micam.sh && /tmp/deploy-micam.sh
```

脚本交互流程：按任意键或回车进行环境检测；填写基础服务配置；输入摄像头设备 ID 和流名称。原帖用米家 App 的设备信息页查看摄像头设备 IP 作为设备 ID 示例。等待镜像拉取和容器启动后访问 `https://NAS_IP:8000/login`，按页面提示设置并输入 6 位密码；如浏览器提示证书不安全，原帖建议进入高级选项继续。首次使用还需同意协议并登录小米账号授权。

若设备显示离线，原帖建议先检查相关端口是否被防火墙拦截。原帖 RTSP 示例：

```text
rtsp://192.168.50.227:8554/stream1
```

这是局域网示例地址。原帖称可用 PotPlayer、VLC 等网络播放器测试。

## EasyNVR 与飞牛相册
先在 NAS 上查找相册真实路径：

```sh
sudo find / -name Photos
```

原帖示例路径 `/vol1/1000/Photos`，并创建录像子目录：

```sh
mkdir -p /vol1/1000/Photos/EasyNVR
```

切换到适合存储数据的目录后运行原帖 Docker 命令。以下路径是原帖示例，需替换为实际相册路径：

```sh
docker run -d --name easynvr --restart always --network host --log-opt max-size=50M -v "$PWD/configs:/app/configs" -v "$PWD/logs:/app/logs" -v "$PWD/temporary:/app/temporary" -v /vol1/1000/Photos/EasyNVR:/app/r -v "$PWD/stream:/app/stream" registry.cn-shanghai.aliyuncs.com/rustc/easynvr_arm64:latest
```

访问 NAS IP 的 `10000` 端口初始化 EasyNVR，创建管理员账号。添加设备时选择 `PULL` 并填入 RTSP 地址；录像计划设为每天。原帖称 EasyNVR 可查看回放并按指定时间段下载，录像目录可出现在飞牛相册中。

原帖对协议的区分：RTSP 拉流但无设备控制；RTMP 主要推流且无设备控制；ONVIF 可提供设备发现、参数配置和云台控制，需摄像头本身支持。

## 2GB 内存设备的 swap 示例
先用 `free -h` 查看内存。原帖在 `/vol1` 创建 2GB swap 文件，说明 Btrfs 上需先关闭该文件的 CoW 属性：

```sh
touch /vol1/swapfile
chattr +C /vol1/swapfile
dd if=/dev/zero of=/vol1/swapfile bs=1M count=2048
chmod 600 /vol1/swapfile
mkswap /vol1/swapfile
swapon /vol1/swapfile
free -h
```

原帖称 `count=2048` 约为 2048MB；改为 `4096` 表示约 4GB。`/vol1/swapfile` 应位于空间充足的挂载点。原帖把启用 2GB swap 描述为“相当于有 4G 内存”，这是对物理内存加交换空间容量的说法，不代表 swap 与 RAM 性能等同。

## cpolar 隧道

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

Web UI 示例 `http://192.168.50.227:9200/`。默认隧道据原帖描述为 SSH/TCP 22 和 website/HTTP 8080；编辑 website 指向 EasyNVR `10000` 端口，选择 `China Top`。免费随机域名约 24 小时变化；固定二级子域名需升级任意付费套餐，示例名称 `easynvr`，然后在隧道编辑页设为二级子域名。

## 来源
- 原帖：https://www.coolapk.com/feed/69971416
- Micam 仓库：http://github.com/miiot/micam
- Micam 部署脚本：https://gitee.com/jun-wan/script/raw/master/micam-deploy/deploy-micam.sh
- 资源边界：配套刷机教程未提供；图片未提供。
