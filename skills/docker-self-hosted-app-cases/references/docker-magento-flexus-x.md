# 在华为云 Flexus X 上用 Docker 部署 Magento

## 应用与部署方式

原帖将 Magento 描述为以 PHP 和 MySQL 为基础的开源商城系统，并概述通过宝塔面板安装 Docker、拉取镜像、配置端口和环境变量、启动容器，再将宝塔网站或应用指向容器的流程。该文的概述部分提到 Magento；实际展示的容器清单使用 `alexcheng/magento2:latest` 和 `mysql:5.6`。

宝塔界面中从 Docker 应用列表选择应用并初始化；原帖提到界面流程中出现 Prometheus，但实际运行容器和 compose 配置是 Magento 与 MySQL。命令行部署参考项目链接见下方。

## Compose 配置

原帖记录以 root 身份在 `dk_magento` 目录中创建 `docker-compose.yml`，内容如下：

```yaml
version: '3.7'
services:
  magento:
    restart: always
    image: alexcheng/magento2:latest
    ports:
      - '${magento_port:-8680}:80'
    volumes:
      - '${magento_data:-./data}:/data'
    depends_on:
      - mysql
  mysql:
    restart: always
    image: mysql:5.6
    environment:
      - MYSQL_ROOT_PASSWORD=${mysql_root_pwd:-root}
      - MYSQL_DATABASE=magento
      - MYSQL_USER=${mysql_user:-admin}
      - MYSQL_PASSWORD=${mysql_pwd:-admin123}
    volumes:
      - '${mysql_data:-./mysql}:/var/lib/mysql'
```

同目录 `.env` 示例：

```dotenv
magento_data=/www/dk_project/dk_app/dk_magento/data
mysql_data=/www/dk_project/dk_app/dk_magento/mysql
magento_port=8680
mysql_user=admin
mysql_pwd=admin123
mysql_root_pwd=root
```

原帖命令及顺序：

```sh
docker pull prom/prometheus:v2.48.0
docker-compose up -d
docker-compose ps
```

原帖在创建 compose 文件前记录了 `docker pull prom/prometheus:v2.48.0`，但配置和示例容器列表中没有 Prometheus 服务；不要将该拉取命令误当作 Magento 镜像拉取命令。

示例运行状态中，Magento 容器使用 `alexcheng/magento2:latest`，容器 80/tcp 映射到主机 8680；MySQL 使用 `mysql:5.6`，容器端口为 3306/tcp。原帖建议在云安全组放行 8680 以测试访问。

## 资源

- Magento 官网（原帖给出的 URL）：https://www.magento.cn/
- Docker Magento 命令行部署参考项目：https://gitee.com/leoncycle/docker-magento

## 证据边界

以上配置、镜像标签、端口和容器状态是原帖记录；未提供独立部署复现或生产环境验证。原帖“宝塔安装文档”和“命令行部署参考文档”的文字链接分别有一处未解析，命令行参考项目由原帖外链上下文确认；未补造宝塔文档地址。

来源：[62161497](https://www.coolapk.com/feed/62161497)（图片与评论未读，未在本库实机复现）。

