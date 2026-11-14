# 使用 Docker Compose 部署 Blossom 与 MySQL 8

## 项目与适用方式

Blossom 被原帖介绍为开源、可私有部署的云端双链笔记平台，也可本地部署；原帖称笔记和图片能保存在部署设备，不依赖图床或对象存储。文章列出的官方部署方式包括 Docker Hub 公共镜像、Docker Compose、JAR 包和 IDEA 插件。没有提供项目主页或仓库的可恢复链接。

原帖建议服务器准备 MySQL（最好 8.0）和 Docker；Compose 示例使用 `mysql:8.0.31` 镜像，并由 MySQL 初始化 `blossom` 数据库。作者称其 Compose 版本为 `2.29.1`，该版本信息来自原帖的 `docker-compose --version` 输出。

## Compose 示例

原帖文件名为 `blossom-mysql8.yaml`。以下保留其服务、镜像、环境变量、端口、卷、网络和健康检查配置；健康检查 URL 在输入中被替换为“查看链接”，无法恢复，故以显式占位说明，不应直接运行该检查行：

```yaml
version: "3.8"

networks:
  blossomnet:
    driver:
      bridge

services:
  blossom:
    image: jasminexzzz/blossom:latest
    container_name: blossom-backend
    volumes:
      # 将冒号前改成宿主机上实际存在的目录；保留容器侧路径。
      - /home/blossom/bl/:/home/bl/
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://blmysql:3306/blossom?useUnicode=true&characterEncoding=utf-8&allowPublicKeyRetrieval=true&allowMultiQueries=true&useSSL=false&&serverTimezone=GMT%2B8
      SPRING_DATASOURCE_USERNAME: root
      SPRING_DATASOURCE_PASSWORD: jasmine888
    ports:
      - "9999:9999"
    networks:
      - blossomnet
    healthcheck:
      # 原帖 URL 被替换，不能复原：test: ["CMD", "curl", "-f", "<原文未提供的URL>"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 5s
    restart: always
    depends_on:
      blmysql:
        condition: service_healthy

  blmysql:
    image: mysql:8.0.31
    container_name: blossom-mysql
    restart: on-failure:3
    volumes:
      - /home/blossom/mysql/data:/var/lib/mysql
      - /home/blossom/mysql/log:/var/log/mysql
      - /home/blossom/mysql/mysql-files:/var/lib/mysql-files
    environment:
      MYSQL_DATABASE: blossom
      MYSQL_ROOT_PASSWORD: jasmine888
      LANG: C.UTF-8
      TZ: Asia/Shanghai
    ports:
      - "3306:3306"
    networks:
      - blossomnet
    healthcheck:
      test: ["CMD", "mysqladmin", "-uroot", "-pjasmine888", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 3s
      retries: 12
```

原帖说明：所有挂载项冒号前是宿主机路径，应替换为运行 Docker 的设备上实际存在的目录；冒号后是容器路径，不应改动。Blossom 数据挂载到 `/home/bl/`；MySQL 数据、日志和 `mysql-files` 分别挂到三个宿主机目录。应用容器通过 Compose 网络中的 `blmysql:3306` 连接数据库，应用对外端口为 `9999`，MySQL 映射 `3306`。

应用与数据库密码需保持一致：`SPRING_DATASOURCE_PASSWORD`、`MYSQL_ROOT_PASSWORD` 与 MySQL 健康检查 `-p` 后的密码必须同步修改。原帖示例明文密码 `jasmine888` 仅是示例配置，且数据库端口映射到宿主机；实际部署时应替换凭据并按需限制外部网络访问。

原帖健康检查还引用 `http://localhost:9999/sys/alive`，但输入中对应 URL 被截断为占位符，不能确认该路径是否就是 Compose 检查所用端点。文章的完整 Compose 文件中，Blossom `curl` 检查行缺少可恢复 URL，因此上方未伪造为可运行配置。

## 来源边界

原帖称上传该 YAML 后可登录使用，但未提供启动命令、实际服务器系统/规格、Blossom 镜像版本标签以外的应用版本信息、公开项目链接或可读取的截图。示例包含 MySQL 3306 端口映射；不应据此推断作者实际将其开放给公网。

来源：[61923627](https://www.coolapk.com/feed/61923627)（图片与评论未读，未在本库实机复现）。

