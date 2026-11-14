# Flexus X 上的 Ward 与 Dashdot 部署记录

## 文章记录的云主机环境

两篇文章均记录了相同的示例规格：华为云 Flexus X、北京四、包年/包月、自定义 4 vCPU/12 GiB、Rocky Linux 9.0 64 位、100 GB 通用型 SSD、3 Mbit/s 全动态 BGP 公网带宽。性能模式关闭（可按需开启），VPC 与安全组可自定义。文章环境表还列出内网地址 `192.168.0.130`、主机名 `flexusx-e2c8` 和 Docker `27.1.1`；这些是文章记录的样例值，不代表通用配置或当前云产品保证。

通过 MobaXterm 连接实例的步骤描述为：在云控制台重置登录密码并选择自动重启，然后使用弹性公网 IP 连接，以 root 账号登录。实际账号凭据不在本文复述。

## Ward

Ward 被文章描述为 Linux 服务器监控工具，可查看 CPU、内存、I/O、网络和服务状态；还描述了自适应界面、深色主题与简洁展示。文章适用场景包括数据中心、云环境及开发测试环境。

环境检查命令及文章展示的 Rocky Linux 9.0 信息：

```sh
cat /etc/os-release
```

文章提到应预先安装 Docker 并检查版本，但没有给出 Ward 的镜像、安装命令、容器端口或启动参数，也没有展示 Ward 的运行结果。正文中的 Rocky Linux 首页和问题反馈 URL 在采集文本里被“查看链接”截断，不能据此还原完整地址。

## Dashdot

Dashdot 被文章描述为面向小型 VPS 和私人服务器的开源仪表盘，展示 CPU、内存、磁盘和网络状态，并提供实时更新。文章记录 Docker 版本检查结果为 `Docker version 27.1.1, build 6312585`：

```sh
docker -v
```

文章提及在 Flexus X 上安装 Dashdot，但没有给出镜像、安装命令、端口映射、环境变量或实际运行截图的可读内容，因此不能从正文复现安装过程，也不能据此断定适配结果。

## 证据边界与资源

两篇文章各含图片，图片未读取；文章对 Flexus X 性能、稳定性及工具适配的评价属于作者陈述，未附基准测试或独立验证。共同环境数据来自两篇文章，工具功能分别保留其对应描述。

- Rocky Linux 官网（原帖记录的 URL）：<https://rockylinux.org/>
- Rocky Linux 问题反馈（原帖记录的 URL）：<https://bugs.rockylinux.org/>

来源：[62162617](https://www.coolapk.com/feed/62162617)、[62162521](https://www.coolapk.com/feed/62162521)（图片与评论未读，未在本库实机复现）。

