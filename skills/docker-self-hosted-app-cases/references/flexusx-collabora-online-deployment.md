# Collabora Online 容器部署与云盘集成记录

## 功能与集成关系

Collabora Online 是基于开源技术的在线办公套件。原帖列出的能力包括在浏览器中查看、编辑文档、电子表格和演示文稿，以及多人实时协作；文章称可集成 Nextcloud、ownCloud。产品与功能描述来自原帖，未独立验证。

## Docker 部署记录

原帖的操作路径是在宝塔面板安装 Docker，在应用商店搜索 Collabora 容器并安装；初始化时设置端口和密码，然后启动容器。文章给出的容器镜像为 `collabora/code:latest`，容器名显示为 `btC-collabora`，端口映射为主机 `9980` 到容器 `9980/tcp`：

```text
0.0.0.0:9980->9980/tcp
:::9980->9980/tcp
```

相应地，文章要求在云安全组放行 9980。管理页访问形式为：

```text
http://主机IP:9980/browser/dist/admin/admin.html
```

进入页面后勾选允许外部访问。文章记录的示例管理员账号为 `admin`，密码为 `k753cyh6fu`。这是公开文章中的历史示例凭据，不应在实际服务中复用；部署时应设置新的强密码，并避免把管理页暴露给不受信任的网络。

文章给出的 Docker 状态行显示容器 `Up 29 seconds`，镜像 `collabora/code:latest`，但这只是原帖截取的当时状态，不代表当前版本或可用性。

## Nextcloud 与 ownCloud 集成记录

文章描述的 Nextcloud 路径：通过 Docker 手动安装，或使用宝塔一键安装；登录 Nextcloud 后，在应用中安装 Collabora Online 插件，再填写 Collabora Online 地址完成配置。ownCloud 部分也描述通过 Docker 或宝塔安装，然后在其应用中安装 Collabora Online 插件并配置服务地址。原帖将 ownCloud 段落中的参考文档标成“nextcloud安装参考文档”，名称与所述目标不一致，不能据此认定该链接是 ownCloud 专用文档。

原帖另列出 Nextcloud 的 OnlyOffice 应用商店链接，但该链接指向 OnlyOffice 插件，并非 Collabora Online 插件页面，不能当作 Collabora 下载地址：<https://apps.nextcloud.com/apps/onlyoffice>。

## 资源

- Collabora Online 官网：<https://www.collaboraonline.com/>
- Collabora 安装文档：<https://sdk.collaboraonline.com/docs/installation/index.html>
- 原帖标为宝塔安装文档的链接：<https://blog.csdn.net/weixin_62782025/article/details/142531081>（采集文本末尾多出 `**`；该尾缀未纳入 URL）
- 原帖标为 Nextcloud 安装参考文档的链接：<https://blog.csdn.net/weixin_62782025/article/details/142443050>
- Nextcloud OnlyOffice 插件页（原帖给出的链接，插件名称与 Collabora 不同）：<https://apps.nextcloud.com/apps/onlyoffice>

## 证据边界

文章含 4 张图片，图片未读取。正文未提供宝塔容器创建时的完整环境变量、持久化卷、TLS/反向代理配置或正式生产部署设置。示例凭据和 `latest` 标签是原帖记录，不能推断为当前推荐配置。

来源：[62162413](https://www.coolapk.com/feed/62162413)（图片与评论未读，未在本库实机复现）。

