# Flexus X 上以 Docker Compose 部署 MediaCMS

## 环境与容器工具

原帖记录实例根分区约 99G（`df -h` 的 `/dev/vda1` 行为 `99G  4.2G  90G  5% /`），`ufw status` 显示 `Status: inactive`。作者因此使用华为云安全组管理外部入站访问。

原帖采用 Ubuntu Jammy amd64 的 Docker `.deb` 包，下载目录为 https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/ ，上传后执行：

```sh
dpkg -i *.deb
docker -v
```

记录的 Docker 版本为 `27.1.2, build d01f264`。Docker Compose 从 https://github.com/docker/compose/tags 获取 `docker-compose-linux-x86_64`，上传后执行：

```sh
sudo cp -f docker-compose-linux-x86_64 /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo docker-compose -v
```

原帖记录 Compose `v2.29.2`，属于 2024 年环境记录。

## MediaCMS 项目与启动记录

项目页面： https://github.com/mediacms-io/mediacms/tree/main 。原帖称下载项目代码并放到服务器，配置管理员密码后运行 Compose。具体密码配置文件、编辑内容和下载命令没有出现在所给正文中，无法补全。

作者记录先执行 `docker-compose up`，随后执行：

```sh
docker-compose up -d
docker ps
```

启动时出现 `POSTGRES_DB`、`POSTGRES_USER` 未设置并默认为空的警告，以及 Compose 文件 `version` 属性已废弃的警告。作者的 `docker ps` 输出显示 `mediacms-main-web-1` 映射 `0.0.0.0:80->80/tcp`，worker、celery beat、PostgreSQL `15.2-alpine` 和 Redis 容器也在运行；数据库和 Redis 状态为 healthy。原帖据此报告部署完成，并提示还需在华为云安全组开放外部访问端口。

MediaCMS 在原帖中的功能描述：基于 Django、前端使用 React，可用于视频和媒体分享，包含分类、标签、嵌入、分享、下载、评论、点赞与浏览计数等功能。这些是原帖介绍，未逐项提供配置或验证。

## 来源与边界

来源： https://www.coolapk.com/feed/61721075 。这是 2024 年作者的个人部署记录；没有完整 Compose 配置、管理员密码配置步骤或安全组规则，不能据此重现所有环境细节。
