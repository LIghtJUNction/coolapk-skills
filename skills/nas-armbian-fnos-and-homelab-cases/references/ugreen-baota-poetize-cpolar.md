# POETIZE 部署记录

## 项目结构与功能
原帖将 POETIZE 描述为 Spring Boot 后端、Vue 前端、MySQL 数据库的前后端分离博客项目。功能包括文章管理、表白墙、图片墙、收藏夹、音乐和视频、留言、友链及后台管理；另有 Vue3 聊天室模块，聊天室依赖博客系统，需从博客“联系我”进入，因为登录模块在博客系统中。项目仓库地址在原帖正文中留空，不能据此补写。

## 宝塔容器
原帖选择 Docker 部署宝塔，以隔离系统环境。SSH 登录绿联 NAS 后切换 root：

```sh
ssh susu@192.168.50.99
sudo -i
```

原帖容器命令（保留端口、卷和镜像）：

```sh
docker run -d \
--name baota \
-p 888:888 -p 8888:8888 -p 3306:3306 -p 7777:7777 -p 7778:7778 -p 8081:8081 \
-v /volume1/docker/baota/www:/www \
-v /volume1/docker/baota/www_server_panel:/www/server/panel \
-v /volume1/docker/baota/www_server_data:/www/server/data \
-v /volume1/docker/baota/www_server_logs:/www/server/logs \
-v /volume1/docker/baota/poetize_logs:/home/poetize/logs \
-v /volume1/docker/baota/www_backup:/www/backup \
--restart=always \
btpanel/baota:latest
```

检查运行状态并进入容器：

```sh
docker ps
docker exec -it baota /bin/bash
```

原帖称在容器内运行 `bt` 后输入数字 `14` 可查看默认信息。示例面板地址为 `192.168.50.99:8888/btpanel`；IP 和面板路径应以实际部署输出为准。

## 部署文件与运行环境
原帖称部署包可从 123 云盘获取，但正文没有实际分享 URL；明确给出的提取码是 `1314`。其描述的包包含前端、后端和数据库文件。宝塔环境版本示例：Nginx、JDK 1.8、MySQL 5.7。

在宝塔创建数据库 `poetize`，设置用户名和密码后导入数据库文件；编辑后端 `application.yml` 的 `username` 与 `password`，与新建数据库凭据一致。

## 前端站点与反向代理
原帖将 Vue2 博客目录配置为 PHP 站点根目录，示例访问端口为 `7777`。Nginx 配置片段如下；原文在 `http://` 与端口数字中插入“删”字，并明确要求移除该字：

```nginx
location / { 
    try_files $uri $uri/ /index.html; 
}

location ^~ /api/ { 
    proxy_pass http://127.0.0.1:8081/; 
    proxy_set_header Host $host; 
    proxy_set_header X-Real-IP $remote_addr; 
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
}
```

原帖操作后重启 Nginx。初次打开前端出现 502，被解释为后端尚未启动。

## Java 后端
在宝塔 Java 项目中选择 `poetize-server.jar`，原帖给出的启动命令是：

```sh
/www/server/java/jdk1.8.0_371/bin/java -Xmx1024M -Xms256M -jar /www/wwwroot/后端/poetize-server.jar --spring.config.location=/www/wwwroot/后端/application.yml
```

原帖要求在项目设置中将启动用户改为 `root`。查看项目日志，原帖以运行端口显示为 `8081` 作为后端启动完成的信号，随后报告前端可以显示数据。目录名“后端”是原帖命令中的路径文字，部署时需与实际目录一致。

## cpolar 与域名
原帖在 NAS 宿主机终端安装 cpolar（若仍在容器中先执行 `exit`）：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

Web UI 示例地址为 `http://192.168.50.99:9200/`。在隧道管理中编辑 `website` 隧道或新建隧道，协议选 HTTP，本地地址填 `7777`，地区示例为 China Top。原帖报告在线隧道列表同时显示 HTTP 与 HTTPS 地址。

固定自定义域名案例中，原帖给出域名 `771566.xyz`，示例 CNAME 值 `32e0a637.cname.cpolar.top`；将 `blog` 作为 DNS 主机记录，记录类型 CNAME，记录值填 cpolar 提供的 CNAME，TTL 默认。cpolar 隧道域名类型改为自定义域名，域名填 `blog.771566.xyz`。原文示例访问地址明确为：

- `https://blog.771566.xyz`
- `http://blog.771566.xyz`

原帖称有 SSL 证书时可上传证书文件；未申请时可用 HTTP。域名、CNAME 和配置结果属于原帖案例，不能视为读者账户可直接复用的配置。

## 来源与边界
来源：酷安帖子 [3步搞定家用NAS网站！绿联NAS + 宝塔 + cpolar，在家跑一个能用自己域名访问的网站！（完整详细流程）](https://www.coolapk.com/feed/70493561)。配图未读取；项目仓库和云盘分享 URL 在输入正文中为空，虽有提取码 `1314`，但无法据此恢复下载链接。文中命令和版本是原帖所述环境，未独立验证。
