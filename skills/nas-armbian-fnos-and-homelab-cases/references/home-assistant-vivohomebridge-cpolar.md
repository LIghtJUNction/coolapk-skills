# Home Assistant Docker 部署、HACS 集成与 cpolar 反向代理

## 适用与证据范围
原帖演示 Windows Docker 部署，并介绍 Linux 命令、HACS、Xiaomi Home、VivoHomeBridge 和 cpolar。图片未提供；部分下载链接、快捷指令配置截图和后台状态无法复核。结果是原帖报告。

## Docker 部署
Windows 命令（原帖插入的“删”标记按其说明移除）：

```powershell
docker run -d --name homeassistant --restart=always -p 8123:8123 -v C:\docker\homeassistant:/config --dns <服务器IP> --dns <服务器IP> --dns <服务器IP> docker.1ms.run/homeassistant/home-assistant:stable
docker ps
```

Linux 命令：

```sh
sudo docker run -d --name homeassistant --restart=always -p 8123:8123 -v /srv/homeassistant:/config --dns <服务器IP> --dns <服务器IP> --dns <服务器IP> docker.1ms.run/homeassistant/home-assistant:stable
```

原帖以容器状态 `UP`、端口 `8123` 为运行示例，访问 `http://localhost:8123` 初始化管理员、位置等设置。镜像地址是原帖明确给出的镜像仓库路径，未核验其内容。

## 安装 HACS 与接入设备
Windows 宿主机进入容器：

```sh
docker exec -it homeassistant sh
```

容器内任选原帖两种安装命令之一：

```sh
wget -O - https://get.hacs.vip | bash -
```

或

```sh
curl -fsSL get.hacs.vip | bash
```

退出并重启：

```sh
exit
docker restart homeassistant
```

随后在 Home Assistant 的“设置 > 设备与服务”添加 HACS。原帖在 HACS 下载 Xiaomi Home 集成，重启 Home Assistant 后添加集成并登录米家。若 OAuth 回调显示 `homeassistant.local` 而无法访问，原帖建议把回调主机名改为部署机器 IP，保留 `:8123/api/webhook/...` 路径，再提交授权。原帖示例地址属于局域网地址，不可照搬。

在 HACS 搜索并安装 `VivoHomeBridge`，重启后添加该集成；通过集成二维码连接 vivo 智慧生活 App 并选择设备。原帖称蓝心小 V 快捷指令要求 **OriginOS 6.0 或更高版本**；快捷指令具体配置图片未提供。

## cpolar 与反向代理
原帖先在 cpolar 官网下载 64-bit 安装包，解压安装，并用 `cpolar version` 检查；官网下载页和注册网址没有实际 URL。登录本机 Web UI 后编辑 `website` 隧道，指向 Home Assistant `8123` 端口。原帖称默认隧道包括 TCP 3389 的 `remoteDesktop` 与 HTTP 8080 的 `website`，后者在线时生成 HTTP 与 HTTPS 地址。

遇到隧道访问 HTTP 400 时，原帖将原因归于 Home Assistant 不信任代理请求，并建议进入容器追加配置：

```sh
docker exec -it homeassistant /bin/sh
cat << 'EOF' >> /config/configuration.yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - <服务器IP>/16
EOF
exit
docker restart homeassistant
```

这是追加写入；若配置文件已有 `http:` 段，重复追加可能产生重复配置，原帖未说明如何合并。代理网段 `<服务器IP>/16` 是原帖示例值，应与实际代理来源相符。原帖称免费随机域名约 24 小时变化；固定二级子域名需付费套餐，地区示例 `China Top`、名称示例 `homeassistant`。

## 来源与资源
- 原帖：https://www.coolapk.com/feed/70438786
- HACS 安装地址：https://get.hacs.vip
- Home Assistant 仓库链接、HACS 官方仓库、国内极速版介绍及 cpolar 官网下载/注册页面的实际 URL 未在正文中给出。
