---
name: zabbix-mysql57-centos7-case
description: 整理原帖中的 Flexus X 产品规格记录、Zabbix 部署顺序和 CentOS 7/MySQL 5.7 安装命令。
---

# Flexus X 上的 Zabbix 监控部署记录

[来源文章](https://www.coolapk.com/feed/61925439)记录了以下历史操作与配置（图片未读，未在本库实机复现）。

## Flexus X 规格与性能说法

原帖描述体验活动提供一台基础模式 Flexus X，规格为 **4 核、12 GB 内存、100 GB 存储、3 M 带宽**。正文还称基础模式 GeekBench 单核及多核成绩可达同规格独享型实例的 1.6 倍，MySQL 性能最高可达同规格独享型实例的 6 倍、长时运行可达 2 倍，并称算力成本可节省最高 30%。这些是宣传/原帖陈述，文章没有给出具体测试分数、对照机型、测试配置或测量过程，不能据此视为独立测评结果。

原帖将 Flexus X 描述为可感知业务负载并动态调整资源配置的云服务器，并提及擎天 QingTian 架构、瑶光云脑、盘古大模型及 X-Turbo 加速。以上属于原帖的产品技术介绍，未提供实现细节或独立验证。

## Zabbix 部署顺序

原帖将 Zabbix 描述为企业级分布式开源监控系统，可监控网络参数和服务器健康状态，配置邮件告警，并基于存储数据提供报表和可视化。其列出的总体步骤为：

1. 准备 Linux 服务器（虚拟机）。
2. 安装 MySQL。
3. 安装 Zabbix Server、Agent 和 Web 组件。
4. 配置 MySQL 并创建 Zabbix 表结构。
5. 配置 Zabbix Server。
6. 启动服务并设置开机自启。

## MySQL 5.7（CentOS 7）准备步骤

原帖指定 MySQL 5.7，提供的仓库 RPM 地址为：

- [http://repo.mysql.com//mysql57-community-release-el7-7.noarch.rpm](http://repo.mysql.com//mysql57-community-release-el7-7.noarch.rpm)

保留原 URL 中双斜杠及 `http` 协议。按原帖步骤安装仓库并安装、启动服务：

```sh
rpm -Uvh http://repo.mysql.com//mysql57-community-release-el7-7.noarch.rpm
yum -y install mysql-community-server
systemctl start mysqld
systemctl enable mysqld
systemctl status mysqld
grep'temporary password' /var/log/mysqld.log
```

最后一行按原文保留为 `grep'temporary password'`，命令与字符串之间没有空格；原帖排版如此，不能保证该命令可按预期工作。

原帖随后给出修改 MySQL root 密码的交互命令：

```text
mysql -u root -p-h localhost
Enter password:
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root!@#$';
```

参数 `-p-h` 连在一起是原帖写法，可能导致参数解析问题；这里不猜测或改写为另一命令。原帖还记录降低密码策略的 SQL：

```sql
set global validate_password_policy=LOW;
set global validate_password_length=4;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
mysql> grant all privileges on *.* to root@'%' identified by 'root';
```

正文明确将简单密码示例标为仅为方便演示，提醒生产环境不要这样设置。向 `root@'%'` 授予 `*.*` 全部权限会扩大远程访问权限；该内容仅作为原帖配置记录，部署时应按实际账号、来源范围和权限需求审查。

## Zabbix 安装资源

原帖指向 Zabbix 4.0、CentOS 7、MySQL 的官方安装选择页面：

- [https://www.zabbix.com/download?zabbix=4.0&os_distribution=centos&os_version=7&db=mysql](https://www.zabbix.com/download?zabbix=4.0&os_distribution=centos&os_version=7&db=mysql)

原帖说明按官网下载页选择版本并使用页面列出的 RPM/YUM 命令，但没有提供实际选定的完整命令，也没有记录后续 Server、Agent、Web 配置或运行结果。页面参数明确指向 Zabbix 4.0、CentOS 7、MySQL；不应将其当成当前版本建议。

## 证据边界

本文包含原帖明确列出的产品规格、性能主张、部署步骤和命令。Zabbix 部署后续内容在输入正文中不完整，图片也未读取；具体建表、服务配置及安装成功状态均缺失。性能数字为原帖主张，没有独立测试数据。
