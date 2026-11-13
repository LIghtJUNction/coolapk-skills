---
name: openwrt-home-server-setup
description: J1900 x86 小主机运行 OpenWrt 旁路由，并按原帖记录配置 Alist、Home Assistant、青龙和 Tailscale 的步骤与限制。
---

# J1900 小主机运行 OpenWrt 与家庭服务

本文整理自 [Coolapk 原帖](https://www.coolapk.com/feed/45524197)，未经目标设备复现。原帖图片、评论及多数旧外链未读；补查的上游文档以下文标注为准；固件、应用与命令的兼容性未独立验证。按现有资料保留可用步骤、命令和参数，缺失部分明确标出。

## 硬件与准备

原帖实例使用 J1900 小主机，配置为 2GB DDR3 内存、32GB mSATA，另有一块可选的 250GB 硬盘。准备 U 盘、网线及电源、VGA、USB、硬盘等所需线材。作者提示，部分代码可能需要能访问境外网站的网络环境，但没有指出具体步骤。

原帖选择 KoolCenter 版本，并提到也可按需选择固件或自行编译。可从 [KoolCenter iStoreOS x86_64 固件目录](https://fw.koolcenter.com/iStoreOS/x86_64/)查看固件；原帖没有指定镜像文件名或版本。

## 写入固件并配置 LAN 旁路由

### 步骤

1. 下载适用于目标设备的 OpenWrt 固件镜像。原帖没有提供具体镜像文件名或版本。
2. 使用 balenaEtcher 选择解压后的镜像和目标 U 盘，然后写入。写入会格式化 U 盘，操作前备份所需资料。
3. 将 U 盘插入小主机并开机。原帖作者的设备可从 U 盘启动；其他设备可能需要按启动热键选择 U 盘启动。
4. 用网线连接小主机 LAN 口和电脑。在电脑网络设置中查看 IPv4 网关地址，并尝试在浏览器打开该地址进入 OpenWrt 管理界面。原帖记录的旧镜像默认登录信息是 `root` / `password`；这不是当前上游默认值，实际信息取决于镜像。
5. 在“网络-接口-LAN”中将传输协议设为“静态地址”，配置旁路由地址、子网掩码、网关和 DNS。
6. 原帖在 LAN 接口中选择物理接口 `eth0`、防火墙区域 `lan`；设备接口名称和界面选项可能不同。
7. 原帖还记录了关闭若干未具体列出的防火墙选项、选择 LAN 的 IP 动态伪装并保存应用。由于具体选项未提供，这些设置仅是原帖记录，不能据此作为通用防火墙配置建议。
8. 原帖建议启用“忽略此接口”，避免旁路由同时提供 DHCP。连接小主机的 LAN 口到主路由 LAN 口后，通过刚设置的旁路由地址访问管理界面。

### 地址与 DHCP 条件

- 旁路由管理地址应处于主路由所在网段。原帖示例为主路由 `192.168.123.1`，旁路由地址写作 `192.168.123.X`。
- 原帖示例子网掩码为 `255.255.255.0`，实际应符合本地网络规划。
- 旁路由自身的默认网关应指向主路由。原帖给出的示例是主路由地址；不要把旁路由自身的默认网关设为它自己。
- 原帖提到若由旁路由提供 DHCP，客户端可使用旁路由作为网关。这是旁路由发给客户端的 DHCP 网关选项，与旁路由自身默认网关是不同设置。
- 本帖采用单一 DHCP 服务的拓扑，不启动第二个未协调的 DHCP 服务。若由旁路由提供 DHCP，应先按网络拓扑妥善停用主路由 DHCP，并确认客户端获得的地址、网关和 DNS 正确；原帖没有给出完整的旁路由 DHCP 配置步骤。
- DNS 可填写运营商 DNS；原帖称不确定时可填主路由地址。
- 原帖建议没有 IPv6 时停用“IPv6 分配”。

## Alist 与 Infuse WebDAV

原帖所用固件带有 iStore。作者通过 iStore 安装 Alist，启用后在 Alist 后台添加“阿里云 Open”存储，并将挂载路径设为 `/dav`。刷新令牌按 Alist 官方说明获取；令牌属于凭据，不应公开。

在 Infuse 中新增文件来源并选择 WebDAV：地址填写旁路由地址，账户信息使用 Alist 账户；高级设置中按原帖填写端口 `5244`、路径 `/dav`。原帖记录的旧镜像默认账户为 `admin` / `admin`，不能视为当前上游或其他安装方式的默认凭据。

原帖没有提供 Alist 版本、令牌获取页面的具体链接或挂载失败的排查步骤。iStore 是否可用取决于所安装的固件。

## Home Assistant、米家与 Apple 家庭

原帖称其固件已集成 Docker，并通过 iStore 搜索安装 Home Assistant。若自行安装 Home Assistant，原帖提到还需安装 HACS，但没有提供实际安装命令；相关参考文章为 [HACS 安装说明](https://blog.csdn.net/zhuweideng/article/details/128220933)。外链正文未包含在现有资料中，不能据此补写命令。

原帖步骤如下：

1. 在集成 Docker 的固件中通过 iStore 搜索并安装 Home Assistant，之后登录设置。
2. 自行安装时，按 HACS 说明完成安装并重启 Home Assistant。
3. 在 HACS 的集成页面添加并下载存储库，搜索原帖拼写为 `xiaomi mlot` 的项目并安装。原帖没有提供仓库地址或版本号。
4. 在 Home Assistant“配置-设备与服务-添加集成”中搜索该米家项目，登录并选择需要接入的设备。
5. 米家设备接入 Home Assistant 后，再添加 HomeKit 集成，将 Home Assistant 中的设备桥接给 Apple 家庭。按原帖步骤，用“家庭”App 扫描 Home Assistant 通知中的二维码完成配对。这是把 Home Assistant 设备桥接到 Apple 家庭，不是将 Apple 家庭设备导入 Home Assistant。

原帖没有提供组件版本、设备支持范围或登录及配对失败的处理步骤。

补查入口（2026-11-14）：[HACS 下载文档](https://www.hacs.xyz/docs/use/download/download/)、[Xiaomi Miot Auto 项目](https://github.com/al-one/hass-xiaomi-miot)、[HomeKit Bridge 官方说明](https://www.home-assistant.io/integrations/homekit/)。HACS 文档区分 Home Assistant 版本及部署方式，不在所有版本上一概照搬旧安装流程；米家项目链接是按原帖意图找到的候选入口，不能据原帖拼写确认其当时使用的仓库或版本。HomeKit Bridge 是把 HA 实体暴露给 Apple 家庭；HomeKit Device 是反方向接入。

## Docker 部署青龙面板

原帖要求已有 Docker，并给出以下镜像拉取命令。原帖在镜像文字处混入了链接标记；`docker pull whyour/qinglong:latest` 是根据完整 `docker run` 命令中出现的镜像名作的排版修复：

```sh
docker pull whyour/qinglong:latest
```

按原帖部署命令整理如下：为可能包含空格的 `$PWD` 路径补了双引号，其余参数按原帖保留。

```sh
docker run -dit \
  -v "$PWD/ql/config:/ql/config" \
  -v "$PWD/ql/log:/ql/log" \
  -v "$PWD/ql/db:/ql/db" \
  -p 5600:5600 \
  --name qinglong \
  --hostname qinglong \
  --restart always \
  whyour/qinglong:latest
```

命令在执行时的当前目录下使用 `ql/config`、`ql/log` 和 `ql/db` 保存对应数据；主机端口 `5600` 映射到容器端口 `5600`。原帖用 `IP:5600` 访问面板安装界面。镜像标签为 `latest`，原帖没有锁定版本或提供 J1900 架构兼容性信息。

原帖记录使用企业微信机器人推送，并在青龙面板添加依赖、拉取脚本库和填写京东 Token。机器人 Key 和各类 Token 都是凭据，不应公开。作者称重复脚本库会自行禁用，但未说明版本或机制；原帖提到 B 站自动投币目标，没有给出脚本仓库或配置步骤。

### 青龙参考资源

- [青龙面板参考文章](https://www.dujin.org/18884.html?ydreferer=aHR0cHM6Ly9jbi5iaW5nLmNvbS8%3D?ydreferer=aHR0cHM6Ly9jbi5iaW5nLmNvbS8%3D)
- [dailycheckin 项目说明](https://sitoi.gitee.io/dailycheckin/)
- [阿里云盘签到项目](https://github.com/mrabit/aliyundriveDailyCheck)

采集到的 `https://www.dujin.org/tag/qinglong):latest` 含有链接标记和镜像标签拼接残片，不是可用的镜像项目链接；镜像名以原帖完整 Docker 命令为准。

## Tailscale 子网路由：原帖 v1.32.2 旧档案

本节保留原帖记录，不能直接当作当前 OpenWrt 安装指南。原帖使用的归档文件名为 `openwrt-tailscale-enabler-v1.32.2-98e126e-autoupdate.tgz`，但原帖采集到的下载命令只剩“查看链接”。原帖引用的说明文章是 [OpenWrt Tailscale 设置参考](https://pfschina.org/wp/?p=9151)。

按完整文件名补查到的[上游同名发布](<https://github.com/adyanth/openwrt-tailscale-enabler/releases/tag/v1.32.2-98e126e-autoupdate>)含有该资产，发布日期为 2022-11-17，资产清单大小为 `1119` 字节。2026-11-14 仅核对发布元数据，未下载或校验归档；enabler 标签也不作为实际运行客户端版本的证明。

补查得到的真实文件地址：

```sh
wget https://github.com/adyanth/openwrt-tailscale-enabler/releases/download/v1.32.2-98e126e-autoupdate/openwrt-tailscale-enabler-v1.32.2-98e126e-autoupdate.tgz
```

原帖记录的旧档案命令如下：

```sh
tar x -zvC / -f openwrt-tailscale-enabler-v1.32.2-98e126e-autoupdate.tgz
opkg update
opkg install libustream-openssl ca-bundle kmod-tun
/etc/init.d/tailscale enable
ls /etc/rc.d/S*tailscale*
/etc/init.d/tailscale start
tailscale up
tailscale up --advertise-routes=192.168.18.0/24 --accept-dns=false
```

这些命令依赖旧归档及原帖未说明的固件、架构和软件包状态。命令中的 `192.168.18.0/24` 是原帖示例；应公告实际 OpenWrt LAN 子网。原帖还给出主路由 `192.168.123.1` 对应 `192.168.123.0/24` 的示例。该 URL 是按文件名补查到的上游资产，不是从原帖占位命令还原出的隐藏地址；文件内容未校验。原帖中的 `wget 查看链接` 缺少 URL，是采集占位残片，不是可执行下载命令。

原帖登录步骤是执行 `tailscale up`，打开终端显示的登录地址并完成账户验证；该步骤也受旧档案与当前安装方式差异影响。

### 当前子网路由注意事项

官方文档指出，Linux 子网路由器需要启用 IP 转发、公告 LAN 路由，并在管理端批准路由或配置 `autoApprovers`，同时通过访问控制规则允许相应流量。对已存在节点修改设置可用 `tailscale set`。这些是官方 Linux 子网路由要求的概述；不要将宿主 Linux 的 `sysctl` 脚本当作 OpenWrt 指令。具体适用的 OpenWrt 配置方式应依据当前安装包和系统版本确认。

参考：[Tailscale 子网路由器官方文档](https://tailscale.com/docs/features/subnet-routers)。

原帖提到在管理页面启用子网路由、禁用节点密钥过期，并报告 OpenWrt 可以 ping 通其他 Tailscale 节点。公告子网路由不代表路由已获批准或访问控制已允许；能 ping 通另一节点也不证明远端客户端能访问被公告的 LAN 子网。禁用节点密钥过期不是启用子网路由的必要条件。原帖没有提供端到端子网连通性测试步骤。

## 资料缺口

- OpenWrt 固件镜像的具体文件名和版本未给出。
- 原帖未提供网关解释链接、HACS 安装命令及可用的 Tailscale 下载命令；上文另补上游资产地址。
- 防火墙中被要求关闭的具体选项未列出。
- Alist、Home Assistant、HACS、青龙及各集成的版本与兼容性未说明。
- 青龙 B 站脚本及具体签到任务配置未提供。
- 原帖图片、评论及多数旧外链正文未读；补查文档的范围见各节。

## 来源

- [Coolapk 原帖：J1900 安装 OpenWrt 新手向教程](https://www.coolapk.com/feed/45524197)
