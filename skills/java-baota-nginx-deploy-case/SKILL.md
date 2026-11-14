---
name: java-baota-nginx-deploy-case
description: 按主题整理原帖中的端口、安全组、JDK、Tomcat、宝塔、Nginx 与 MySQL 操作及遇到的问题，并标注原文缺失的步骤。
---

# Flexus X 实例部署瑞吉外卖：原帖命令与故障记录

## 网络端口与防火墙

原帖指出，访问应用需开通 `8888`，访问 MySQL 需开通 `3306`；其后针对 Tomcat `8080/tcp` 列出两种选择，并称实际采用放行端口的方式：

```sh
# 以下操作2选一即可
# 方式1：关闭防火墙
systemctl stop firewalld # 关闭防火墙
systemctl disable firewalld # 停止防火墙开机自启
# 方式2：放行8080端口的外部访问
firewall-cmd --add-port=8080/tcp --permanent # --add-port=8080/tcp表示放行8080端口的tcp访问，--permanent表示永久生效
firewall-cmd --reload # 重新载入防火墙规则使其生效
firewall-cmd --zone=public --list-ports #查看防火墙开放的端口
```

原帖称放行后仍无法访问 Tomcat，后来安装宝塔提供的 Tomcat 7 后才可访问；没有给出排查过程或最终网络规则。安装 Nginx 时，作者报告关闭防火墙后可访问、开启后不可访问；使用宝塔后又报告防火墙开启时可以连接，但不清楚原因。此记录不能证明单纯放行某端口即可解决问题。

## 安装 JDK 及环境变量

原帖使用 yum 安装 OpenJDK 8：

```sh
yum install java-1.8.0-openjdk* -y
```

随后编辑 `/etc/profile`，写入：

```sh
JAVA_HOME=/usr/local/btjdk/jdk8
PATH=$JAVA_HOME/bin:$PATH
```

并执行：

```sh
source /etc/profile
java -version
```

原帖解释，选择 yum 是因为其提到的黑马安装包传输很慢。没有给出 JDK 安装后的版本输出。

## 宝塔安装与 Nginx

原帖说明使用宝塔面板安装后续软件，并记下安装回显中的 `Bt-Panel`、`username` 和 `password` 用于登录。安装命令在原文排版中含有被“查看链接”文字替代的 URL，且命令不完整：

```sh
yum install -y wget && wget -O install.sh 查看链接 && sh install.sh
```

该命令的下载 URL 缺失，不能据此补造完整命令。输入宝塔面板地址登录后，按面板提示开放所需端口。原帖提供的独立安装脚本链接为：<http://download.bt.cn/install/install_6.0.sh>。这是脚本资源链接，不是原帖缺失命令中 URL 已被核实的证明。

作者提到宝塔安装了 `nginx1.16`，并用 `whereis nginx` 查到两个 Nginx 路径；其此前通过 `./nginx` 启动的仍是原安装路径中的 Nginx。原帖没有给出路径、配置或服务管理命令。

## MySQL 启动与连接报错

原帖提到安装 MySQL 5.7 后还需启动，并执行：

```sh
systemctl enable mysqld
```

回显称 `mysqld.service is not a native service`，并将操作重定向到 `/sbin/chkconfig`。原帖随后执行：

```sh
/sbin/chkconfig mysqld on
```

它报告 MySQL 连接错误：

```text
(1130, "<服务器IP>' is not allowed to connect to this MySQL server")
```

作者怀疑是防火墙端口，执行了以下规则，但称 `3306` 实际已经开放；该错误记录本身不能证明问题由防火墙导致或解决：

```sh
firewall-cmd --add-port=3306/tcp --permanent # --add-port=3306/tcp表示放行3306端口的tcp访问，--permanent表示永久生效
firewall-cmd --reload # 重新载入防火墙规则使其生效
```

宝塔安装的数据库初始密码位置原帖以图片呈现；原帖给出的登录命令是：

```sh
mysql -uroot -p
```

## 来源与证据边界

来源：[酷安原帖](https://www.coolapk.com/feed/61924142)。原文还引用瑞吉外卖部署博客、Tomcat 教程和华为云宝塔教程，但仅有标题，没有可恢复的完整链接；文章含 3 张图片，图片内容未提供。故障与安装结果均为作者报告，系统发行版、完整应用部署步骤及最终连通性验证信息不足。
