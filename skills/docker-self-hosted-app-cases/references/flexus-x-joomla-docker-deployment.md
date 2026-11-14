# Flexus X 上部署 Joomla 的 Docker Compose 配置

## Joomla 项目与实验环境

Joomla! 是开源内容管理系统，原帖说明其基于 MVC Web 应用框架，可用于企业网站、门户、出版物、电商、预订及组织网站等。

- Joomla 官网：<https://www.joomla.org/>
- Joomla CMS 仓库：<https://github.com/joomla/joomla-cms>

原帖实验环境为 Flexus X 实例、Ubuntu 22.04.4 LTS、Docker `27.2.0`、Docker Compose `v2.29.2`，规格报告为 `4核12G-100G-3M` 基础模式。内网 IP `192.168.0.168` 是原帖环境记录。作者报告使用体验流畅，同时表示 Joomla UI 未达到其预期；这些是个人体验，不是性能基准。

## Docker 安装记录

原帖通过 apt 安装 Docker。依赖安装命令：

```sh
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

添加 GPG 密钥：

```sh
sudo curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

仓库配置原文包含被替换的 URL。其结构如下，`查看链接` 无法据现有材料恢复，因此不能直接作为完整可执行命令：

```sh
sudo add-apt-repository \
      "deb [arch=amd64] 查看链接 \
      $(lsb_release -cs) \
      stable"

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

更新 apt 包索引的 `sudo apt-get update` 与安装 Docker 的 `sudo apt-get install ...` 是两个独立步骤。原帖提供的文档链接为 <https://docs.docker.com>。

原帖列出以下 Docker 镜像加速地址：

- <https://ustc-edu-cn.mirror.aliyuncs.com/>
- <https://docker.mirrors.ustc.edu.cn>
- <https://docker.nju.edu.cn>
- <https://ccr.ccs.tencentyun.com/>
- <https://docker.m.daocloud.io/>
- <https://dockerproxy.com>

原帖中的 JSON 由于链接脱敏而缺少各值后的引号/逗号细节，不能原样构成有效配置；上列地址按链接清单还原为单独 URL，不代表都应同时配置。编辑路径及重启命令原文为：

```sh
sudo vim /etc/docker/daemon.json
sudo systemctl daemon-reload
sudo systemctl restart docker
```

原帖报告安装结果为 Docker `27.2.0, build 3ab4256`、Compose `v2.29.2`，并称 `systemctl status docker` 显示 `active (running)`。

## Joomla、MySQL 与 phpMyAdmin Compose 示例

原帖给出的配置包含三个服务：Joomla、MySQL 和 phpMyAdmin。保留原始镜像、端口、环境变量及挂载路径：

```yaml
version: '2'

services:
  joomla:
    #image: joomla:3.6.2-apache-php7
    image: joomla:3.6.2-apache
    links:
     - mysql
    ports:
     - "2370:80"
    environment:
     - JOOMLA_DB_HOST=mysql:3306
     - JOOMLA_DB_PASSWORD=12345678
    volumes:
     - ./code:/var/www/html

  mysql:
    image: mysql:5.7.14
    environment:
     - MYSQL_ROOT_PASSWORD=12345678
     - MYSQL_DATABASE=joomla
    volumes:
     - ./database:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:4.6.4-1
    links:
     - mysql
    ports:
     - "2371:80"
    environment:
     - PMA_HOST=mysql
```

Compose 把宿主机当前目录下的 `./code` 和 `./database` 分别挂载到容器内 Joomla 目录及 MySQL 数据目录。Joomla 暴露宿主机 `2370` 到容器 `80`；phpMyAdmin 暴露宿主机 `2371` 到容器 `80`。`mysql:5.7.14` 为原帖明确列出的镜像版本。原帖没有给出启动 Compose 的命令、目录准备步骤或安全组端口配置。

配置中的数据库口令是原帖示例值，不应作为真实部署凭据使用；本记录保留其技术上下文以准确呈现示例。

来源：[61922863](https://www.coolapk.com/feed/61922863)（图片与评论未读，未在本库实机复现）。

