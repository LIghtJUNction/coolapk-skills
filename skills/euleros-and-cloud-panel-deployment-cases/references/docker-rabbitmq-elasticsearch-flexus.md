# Flexus X 上运行 RabbitMQ 与 Elasticsearch

## 安全组与防火墙

原帖提醒：访问应用需在云安全组开放相应端口，示例提到 8888；访问 MySQL 需开放 3306。RabbitMQ 与 Elasticsearch 示例另使用各自端口，云安全组和主机防火墙都需按实际访问路径配置。

原帖给出的 firewalld 命令：

```sh
firewall-cmd --add-port=3306/tcp --permanent
firewall-cmd --reload
```

第一条将 3306/tcp 规则永久加入 firewalld，第二条重新载入规则。原帖是在示例讲解中使用该规则，并未给出 RabbitMQ 或 Elasticsearch 对应的防火墙放行命令。

## RabbitMQ 管理镜像

拉取镜像：

```sh
docker pull rabbitmq:3-management
```

运行容器：

```sh
docker run \
 -e RABBITMQ_DEFAULT_USER=‘mq账号’ \
 -e RABBITMQ_DEFAULT_PASS=‘密码’ \
 --name mq \
 --hostname mq1 \
 -p 15672:15672 \
 -p 5672:5672 \
 -d \
 rabbitmq:3-management
```

原帖使用弯引号 `‘ ’` 包住账号和密码占位文字；按原文保留。执行前应将占位值替换为实际值，并确认终端对弯引号的处理；原帖没有提供实际凭据。映射端口为管理界面 15672 和 AMQP 5672。

## Elasticsearch 单节点

原帖先创建供 Elasticsearch 和 Kibana 共用的 Docker 网络：

```sh
docker network create es-net
```

原帖称使用 Elasticsearch 7.12.1 镜像，并提供 `es.tar` 本地镜像包导入方法：

```sh
docker load -i es.tar
```

同时称 Kibana 镜像 tar 包也需同样导入，但未给出文件名、下载地址或校验值。原帖提示上传后确认文件大小一致；这不能替代内容哈希验证。

运行 Elasticsearch：

```sh
docker run -d \
	--name es \
    -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
    -e "discovery.type=single-node" \
    -v es-data:/usr/share/elasticsearch/data \
    -v es-plugins:/usr/share/elasticsearch/plugins \
    --privileged \
    --network es-net \
    -p 9200:9200 \
    -p 9300:9300 \
	elasticsearch:7.12.1
```

该命令设置单节点模式、JVM 初始和最大堆均为 512 MB，创建数据与插件命名卷，连接 `es-net`，并映射 9200、9300 端口。原帖后文还解释了 `cluster.name=es-docker-cluster`、`http.host=0.0.0.0`、日志卷和插件卷等参数，但这些参数并未出现在上面的实际运行命令中，不应视作已启用配置。`--privileged` 则确实出现在命令中。

原帖给出的访问示例为 `http://你的ip:9200`。该 URL 是占位示例；原帖称浏览器可看到 Elasticsearch 响应，但没有附可读取的响应内容或独立验证。

## 资源与证据边界

原帖提及的 Docker/RabbitMQ 学习文章以文字标题链接出现，未提供可恢复 URL。`es.tar` 与 Kibana tar 包也没有公开下载地址或提取码，因此不能在此提供下载链接。正文中“运行了就删”后接图片占位，无法确认其具体指令或对象；不据此补造删除命令。原帖的硬件性能与高可用性描述属于文章陈述，未附基准测试数据。图片共 7 张未读取，评论未提供。

来源：[61925480](https://www.coolapk.com/feed/61925480)（图片与评论未读，未在本库实机复现）。

