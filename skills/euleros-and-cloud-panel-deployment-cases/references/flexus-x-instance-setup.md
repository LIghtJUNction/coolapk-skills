# 华为云 Flexus X 实例：购买、连接与基础配置

## 购买与控制台操作

原帖描述的购买流程是：进入 Flexus X 实例产品页，注册或登录并完成实名认证，然后选择地域、CPU 与内存规格、镜像、存储/网络/安全组和弹性公网 IP 带宽，确认配置后购买。示例中系统盘为 40G；测试用途选择 1M 带宽，上线服务则按业务需求选择。原帖称免费 HSS 可用 1 个月，密码设置要求为 8–26 个字符并包含大小写等要求。购买后可在控制台的弹性云服务 ECS 中查看实例，通过华为云 Web 远程登录，或使用 Xshell 连接公网地址。

控制台“更多”菜单提供重置镜像环境操作；原帖先后展示安全确认、等待提示和重装后重新连接。安全组规则在控制台配置入站与出站访问；按实际项目开放所需端口和来源 IP。

原帖促销信息为“华为云828”期间 2核2G 云服务器 36元/年，属发帖时的活动报价，不代表当前价格。产品介绍页及活动入口在原文中仅以“查看链接”展示，没有可恢复的 URL。

## 规格与计费（原帖所述）

多篇部署文章使用的测评规格一致：Flexus X 实例基础模式，4核12G、100G、3M。此为原帖记录的配置，不构成当前可购规格或性能承诺。

原帖将计费分为包年/包月和按需计费：前者预付并按订单周期计费，后者按实际使用时长计费，文中称秒级计费、按小时结算。包年/包月关机仍按周期计费；按需关机后 vCPU、内存、镜像等基础资源不计费，但云硬盘、弹性公网 IP、带宽等绑定资源仍计费。两种方式都支持变更规格。原帖称包月 3 个月及以上（含多次续费累计）支持备案，按需方式不支持备案。

性能模式文章记录的范围：关闭时 vCPU 1–16、内存 1–128GiB，处理器为第三代 Intel Xeon 可扩展处理器，基频/睿频 2.8GHz/3.5GHz；开启时 vCPU 2–32、内存 2–256GiB，基频/睿频 2.45GHz/3.5GHz。原帖称开启性能模式使用底层物理绑核以提供 QoS 稳定性。以上为发帖时的文章记录，未独立验证。

另一篇文章对产品定位的描述为：Flexus 应用服务器 L 实例面向负载较低、希望快速部署的中小企业和开发者；Flexus X 面向中等负载且需要灵活选配者；普通 ECS 面向网站、电商、图形渲染、数据分析和高性能计算等广泛负载。

## 登录与安全组

原帖给出三种 Linux 连接方式：控制台 VNC；PuTTY、Xshell 等 SSH 客户端，填写弹性公网 IP、用户名 `root` 和密码；或从 Linux/Mac 主机执行：

```sh
ssh 弹性公网IP
```

文中示例为 `ssh 123.xxx.xxx.456`。MobaXterm 可新建 SSH 连接，填写公网 IP、用户名和默认端口 22。示例使用 root 登录，属于原帖操作记录。

安全组是同一 VPC 内实例的访问策略。原帖称默认安全组出方向全部放行、入方向受限；添加规则时优先级可填 1，按项目需要选择协议和端口，并开放所需入站访问。CodiMD 部署示例需增加 TCP 3000 入站规则。

## Ubuntu 22.04 上安装 Docker（原帖记录）

应用部署文章记录的环境为 Ubuntu 22.04.4 LTS、Docker 27.2.0，内网地址记录为 192.168.0.168。Docker 安装片段如下；原帖的仓库地址在正文中被“查看链接”替代，因此保留占位说明，不能直接作为可执行完整命令：

```sh
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

$ sudo curl -fsSL [Docker GPG URL 在原帖显示为“查看链接”] | sudo apt-key add -

$ sudo add-apt-repository \
      "deb [arch=amd64] [Docker Ubuntu 仓库 URL 在原帖显示为“查看链接”] \
      $(lsb_release -cs) \
      stable"

$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
$ sudo vim /etc/docker/daemon.json
```

原帖记录的镜像加速地址包括：`https://ustc-edu-cn.mirror.aliyuncs.com/`、`https://docker.mirrors.ustc.edu.cn`、`https://docker.nju.edu.cn`、`https://ccr.ccs.tencentyun.com/`、`https://docker.m.daocloud.io/`、`https://dockerproxy.com`。正文中的 JSON 片段排版不完整（引号/逗号不齐），不能据此复原成有效配置文件。

重启 Docker 的原帖命令：

```sh
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

原帖报告的检查结果为 `Docker version 27.2.0, build 3ab4256`，Docker Compose 为 `v2.29.2`，`systemctl status docker` 显示 `active (running)`。这是原帖记录的环境结果。

## 来源与证据边界

来源：酷安文章 [61770434](https://www.coolapk.com/feed/61770434)、[61831594](https://www.coolapk.com/feed/61831594)、[61831689](https://www.coolapk.com/feed/61831689)、[61831472](https://www.coolapk.com/feed/61831472)、[61831305](https://www.coolapk.com/feed/61831305)。61770434 有 5 张图片，其余部署文章也含图片；图片内容未提供，图中可能包含未能整理的配置细节。评论未提供。文中产品宣传、计费与性能参数均按原帖年代记录，未独立验证。
