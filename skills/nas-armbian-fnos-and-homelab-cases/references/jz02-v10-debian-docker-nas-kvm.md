# 低成本设备改造与远程服务部署

## 玩客云：One-KVM 硬件级远控
One-KVM 基于 PiKVM 二次开发，利用视频采集、USB 键鼠模拟和虚拟 USB，使控制不依赖被控主机操作系统，可用于 BIOS 操作、系统安装或系统失去响应后的查看与操作。教程使用玩客云，电源为 12V/1.5A；被控设备需有 HDMI 等视频输出和 USB 接口。HDMI 采集卡负责取画面，USB 公对公线负责 OTG 键鼠控制。文章称二手盒子约几十元、采集卡十几元、总成本不到百元、功耗几瓦，未提供功耗测量条件。

刷写所需镜像名为 `One-KVM_by-SilentWind_Onecloud_250930.burn.img`，工具为 Amlogic USB Burning Tool v2.2.0。原帖列出 123 云盘提取码 `1314`，但未提供实际分享 URL。拆机后取出主板，按图示找到短接触点；用 USB 公对公线连接玩客云靠近 HDMI 的 USB 口和电脑，镊子持续短接触点后接电源，待工具显示连接成功再松开，点击“开始”烧录。刷写完断开 USB 与电源，接网线重启，约 1–2 分钟后从路由器查看 IP。后台默认用户名和密码均为 `admin`。被控端接好采集卡与 USB 控制线后，网页点击 KVM；无画面时可尝试 HTTP MJPEG 视频模式。

cpolar 安装命令（原帖标注为去除“删”字后使用）：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

教程示例 Web UI 地址 `http://192.168.50.136:9200/`，设备后台地址写作 `https://192.168.50.136`。在 cpolar 隧道列表编辑 `website` 隧道，服务本地端口改为 443，命名 `onekvm`，地区选 China Top；在线隧道列表会显示 HTTP 与 HTTPS 地址。免费随机域名约每 24 小时变化；固定二级子域名需付费套餐，先在预留页面保留地区、名称，再将隧道域名类型改为二级子域名。

## 玩客云：HiNas 迷你 NAS
原帖称 HiNas 是轻量 Linux NAS 方案，功能包括 SMB/WebDAV 文件服务、同步、媒体索引、Docker 扩展及 Web 管理；列出的适配设备包括玩客云、N1、香橙派。教程使用玩客云、12V/1.5A 电源、网线和 USB 公对公线，镜像为 `hinas_s805_eMMC.burn.img`，烧录工具 Amlogic USB Burning Tool v2.2.0。拆机短接、刷写与重新联网步骤和前述玩客云刷机流程相同。原帖列出 123 云盘提取码 `1314`，没有给出分享 URL。刷写后约等 1–2 分钟，通过路由器查 IP，访问设备 Web 页面自行注册账户。

终端后台使用 root 登录，原帖所列初始密码为 `ecoo1234`；修改密码命令：

```sh
passwd root
```

cpolar 安装与状态命令：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

示例控制台地址为 `http://192.168.50.179:9200/`。在隧道管理中编辑 `website` 隧道，将其指向 HiNas Web 服务；文章没有明确写出该服务的本地端口数值。随机域名约 24 小时变化；固定二级子域名需升级任意付费套餐，并在隧道设置中选择二级子域名。原帖报告固定域名可访问 HiNas 及其 Alist 服务，并提及 Alist 密码 `admin123`；这是教程示例凭据，实际部署应自行设置。文章称设备功耗约 3W、二手价格 20 多元，未给测量条件。

## jz02_v10（高通 410）：Debian 13 与 SSH
教程针对型号 `jz02_v10`，报告硬件为四核 Cortex-A53、512 MB–1 GB RAM、4 GB eMMC；这是原帖所述规格，不代表全部批次。工具包描述包括 MiKo、QPST、Qualcomm Premium Tool、vivo 驱动及刷机包，123 云盘提取码 `1314`，但没有分享 URL。图中关键位置和工具选项未读取，以下仅记录正文可见操作。

流程概述：安装 vivo 驱动、7-Zip、MiKo、QPST 与 Qualcomm Premium Tool；按住 Reset 接入电脑进入 9008，用 MiKo 的 `Read` → `Partition Backup /Erase` → `Load Partition Structure` → `Read Full Image` 备份全分区。重新进入 9008 后，用 Qualcomm Premium Tool 扫描分区，选择 `aboot` 分区并写入同目录的 `芷荷aboot.img`，用于后续 fastboot 操作。QPST 的 Service Programming 中读取设备，在 `UMTS System` 将 `Perf Service` 从 `PS Only` 改为 `CS and PS`，写回后重新读取确认；若出现 `WARNING:Phone no longer detected on port coM8.`，按原帖说明重新插拔并重做该配置流程。

后续步骤包括在设备后台把 SIM 卡切换到外置卡槽；从 `Debian13_K6.17.0-rc6-20251007.zip` 选择 boot 镜像。`nooc` 后缀为非超频内核，不带 `nooc` 为超频内核。原文在脚本名处写作 `boot-jz02v10-mooc.img`，疑似与前述 `nooc` 拼写不一致，照录且不擅自修正；复制到一键刷机脚本目录并重命名 `boot.img`。从 Android 刷入 Debian 使用 `flash.bat`；Debian 环境下重刷 boot 和 rootfs 使用 `flash-只刷boot和rootfs.bat`。首次刷写原帖报告耗时 250.5 秒。首次可能需在 Windows 设备管理器更新 ADB/复合设备和 RNDIS 驱动。

作者报告刷入 Debian 13 后可通过热点 `192.168.69.1` 或 RNDIS `192.168.68.1` SSH 登录，示例命令为：

```sh
ssh root@192.168.68.1
```

密码在原帖中为初始示例值 `1313144`。关闭默认 Wi-Fi 热点并进入 NetworkManager 文本界面：

```sh
nmcli con down WIFI
nmtui
```

选择“启用连接”后选 Wi-Fi 网络并连接。修复 root shell 配色的原帖步骤是先给 `user` 设置密码，再切换用户复制配置：

```sh
passwd user
su user
sudo cp /home/user/.bashrc /root/
su root
```

cpolar 安装、启用和状态检查：

```sh
curl -L https://www.cpolar.com/static/downloads/install-release-cpolar.sh | sudo bash
sudo systemctl enable cpolar
sudo systemctl start cpolar
sudo systemctl status cpolar
```

Web UI 示例为 `http://192.168.68.1:9200`。默认 SSH 隧道指向 TCP 22。文章示例随机公网端点为 `1.tcp.nas.cpolar.cn:10907`，客户端连接命令：

```sh
ssh root@1.tcp.nas.cpolar.cn -p 10907
```

原帖称免费随机 TCP 地址和端口约 24 小时变化；固定 TCP 地址示例为 `9.tcp.cpolar.top:10417`，需在预留页面保留后将 SSH 隧道改为固定 TCP 端口。以上公网端点是文章示例，不应视作当前可用地址。

## jz02_v10：Docker、青龙与自动任务
该系列第二篇报告测试机约 371 MiB 总内存、75 MiB 空闲、根分区余量约 2.1 GB；并称设备物理内存约 512 MB。先创建 512 MB Swap，原帖报告 `swappiness=10`：

```sh
curl -fsSL https://gitee.com/jun-wan/script/raw/master/swap/auto_setup_swap.sh | sudo bash
```

Docker 安装命令（保留原帖参数）：

```sh
bash <(curl -f -s --connect-timeout 10 --retry 3 https://linuxmirrors.cn/docker.sh) --source mirrors.tencent.com/docker-ce --source-registry docker.1ms.run --protocol https --install-latested true --close-firewall false --ignore-backup-tips
```

脚本交互默认选中公网，按回车继续。用 `docker --version` 查看版本。青龙数据目录和容器：

```sh
sudo mkdir -p /ql/data
sudo chown -R 1001:1001 /ql/data
docker pull whyour/qinglong:latest
docker run -d --name qinglong \
  -p 5700:5700 \
  -v /ql/data:/ql/data \
  --restart unless-stopped \
  whyour/qinglong:latest
docker ps
```

浏览器访问示例 `http://192.168.68.1:5700`，首次初始化可跳过通知配置并创建登录账户。青龙依赖管理中，Linux 依赖填写 `gcc g++ make libffi-dev openssl-dev`，Python3 依赖填写 `dailycheckin`。以百度贴吧为例，在脚本管理创建 `config.json`，按项目示例填入 `TIEBA` Cookie；定时任务命令为：

```sh
task dailycheckin --include TIEBA
```

`--include TIEBA` 限定执行 TIEBA，省略参数则运行全部任务。文章描述通过日志查看执行情况；Cookie 属账户敏感信息，不应分享。

cpolar 安装与服务配置：

```sh
curl -L https://www.cpolar.com/static/downloads/install-release-cpolar.sh | sudo bash
sudo systemctl enable cpolar
sudo systemctl start cpolar
sudo systemctl status cpolar
```

Web UI 示例 `http://192.168.68.1:9200`。在隧道列表编辑 `website`，将本地服务改为青龙端口 5700；原帖将隧道命名为 `qinglong-5700`。随机域名约 24 小时变化；固定子域名示例 `qinglong01`，需预留后改隧道域名类型。

## jz02_v10：OpenList 与夸克网盘
第三篇使用 Docker 部署 OpenList Lite：

```sh
docker run -d \
  --name openlist \
  --user root \
  -p 5244:5244 \
  -v /etc/openlist:/opt/openlist/data \
  --restart unless-stopped \
  openlistteam/openlist:latest-lite

docker ps
docker exec openlist ./openlist admin set 123456
```

最后一条命令中的 `123456` 是教程演示的管理员密码参数，应替换为自行设置的值。Web 页面示例 `http://192.168.68.1:5244`。挂载夸克网盘需在 OpenList 存储设置选夸克驱动，配置挂载路径、账号 Cookie 和根文件夹 ID；Cookie 是账户敏感信息，不应公开。原帖称挂载后状态显示“工作”。

Swap 与 Docker 安装命令同上。本文的 Swap 脚本原文 URL 被插入字样处为 `https://gitee.com/jun-wan/script/raw/master/swap/auto_setup_swap.sh`，Docker 脚本为 `https://linuxmirrors.cn/docker.sh`，安装参数见青龙章节。

cpolar 安装命令为 `sudo curl https://get.cpolar.sh | sh`，检查命令 `systemctl status cpolar`，Web UI 示例 `http://192.168.68.1:9200`。编辑 `website` 隧道指向 OpenList Web 服务；文中未明确给出该隧道具体本地端口设置步骤。文章报告免费随机域名约 24 小时变化，固定子域名例为 `openlist01`，隧道名 `openlist-5244`。作者报告视频经 OpenList 的 302 重定向直接从夸克源播放，观察到下载速度 4.21 MB/s；并转述免费版随机域名带宽为 1M。该现象与数值是单次文章报告，未提供测试网络、计量口径或图片证据。

## 玩客云：EasyVoice TTS
EasyVoice 原项目与文章所用二次开发分支分别为：

- 原项目：<https://github.com/cosin2077/easyVoice>
- 二次开发分支：<https://gitee.com/jun-wan/easyvoice>

原帖称项目基于 Edge TTS，支持流式音频、字幕、多角色配音与 AI 角色解析；二开分支增加多角色 UI、登录和 AMD64、ARM64、ARMv7 支持。作者指出上游不支持 ARMv7，原因是 `/jieba` 的原生模块 `jieba.linux-arm-musleabihf.node` 缺少预编译文件；分支对加载失败作容错，ARMv7 降级到按标点切分的简单分词器，AMD64/ARM64 继续用 Jieba。对长句断句可能有差异。原帖报告近 700 万字文本流式生成期间，播放不足 3 分钟时已生成约 34 分钟音频；这是作者演示数据，测试资源和完整耗时未给出。

HiNas 上通过 `install-portainer.sh` 安装 Docker 管理面板。Portainer 初始化密码至少 12 位。玩客云 ARMv7 部署命令：

```sh
docker run -d -p 9001:9000 --name easyvoice -v ./audio:/app/audio tannic666/easyvoice:v1.1-armv7
```

AMD64 或 ARM64：

```sh
docker run -d -p 9001:9000 --name easyvoice -v ./audio:/app/audio tannic666/easyvoice:v1.1
```

检查容器和日志：

```sh
docker ps
docker logs easyvoice | head -n 10
```

容器内部服务端口为 9000，宿主映射端口为 9001；示例访问 `http://192.168.50.179:9001`。多角色页面可调整音色、语速、音量、音调；AI 解析需配置兼容 OpenAI v1 格式的 API 地址与 Key。文中示例提示词要求除非出现明确引号或冒号发言，否则视为旁白。

cpolar 安装与状态检查：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

本地 Web UI 示例 `http://192.168.50.179:9200/`。编辑 `website` 隧道，将本地地址设为 9001，命名 `easyvoice-9001`，地区 China Top。固定二级域名需先预留，再修改隧道域名类型。

## 证据范围
以上整理依据来源文章正文与随附链接引用。八篇文章均有图片，但图片内容未读取；输入没有评论记录，评论结论未作核对。文中作者报告的安装、访问和性能现象均未独立验证。资源分享 URL 若原文未提供则明确记为缺失，没有推造链接。

来源：[69151237](https://www.coolapk.com/feed/69151237)、[69004639](https://www.coolapk.com/feed/69004639)、[68799380](https://www.coolapk.com/feed/68799380)、[68723512](https://www.coolapk.com/feed/68723512)、[68684456](https://www.coolapk.com/feed/68684456)、[69117839](https://www.coolapk.com/feed/69117839)（图片与评论未读，未在本库实机复现）。

