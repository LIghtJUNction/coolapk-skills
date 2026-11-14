# MaxKB 离线部署与运维（Flexus X 示例）

## 系统与网络要求

原帖列出的部署要求：Ubuntu 22.04 或 CentOS 7.6，64 位；至少 2 核 CPU、4 GB 内存；磁盘空间 100 GB。文中技术栈列为 Vue.js、Python/Django、LangChain、PostgreSQL/pgvector。离线部署需开放 SSH 的 22 端口，以及默认 Web 服务端口 8080（可按实际配置更改）。

原帖快速安装建议选择 Linux 环境，并提到 CentOS。该建议与上面的系统要求一并作为原帖记录；没有说明具体发行版、依赖版本与兼容矩阵。

## 1Panel 快速安装路径

原帖给出的 1Panel 安装脚本命令如下，执行环境描述为服务器命令行：

```sh
curl -sSL https://resource.fit2cloud.com/1panel/package/quick_start.sh -o quick_start.sh && sudo bash quick_start.sh
```

安装 1Panel 后登录面板，在 AI 大模型分类中选择 MaxKB，按默认选项安装。Web 访问端口为 8080；需在云控制台安全组放行该端口。原帖展示的访问形式为 `http://您的服务器ip+8080端口号`，这是地址占位写法，不是可直接使用的 URL。

快速安装脚本：<https://resource.fit2cloud.com/1panel/package/quick_start.sh>

## 离线安装配置

原帖记录以 root 用户通过 SSH 登录服务器，将离线包解压：

```sh
tar -zxvf maxkb-v1.2.0-offline.tar.gz
```

解压后的 `install.conf` 可在首次安装前设置安装目录、服务端口、Docker 网段和数据库参数。原帖给出的默认值为：

```ini
MAXKB_BASE=/opt
MAXKB_PORT=8080
MAXKB_DOCKER_SUBNET=<服务器IP>/16
MAXKB_EXTERNAL_PGSQL=false
MAXKB_PGSQL_HOST=pgsql
MAXKB_PGSQL_PORT=5432
MAXKB_PGSQL_DB=maxkb
MAXKB_PGSQL_USER=root
MAXKB_PGSQL_PASSWORD=Password123
```

原帖说明：首次安装前修改 `install.conf`；安装完成后如需调整参数，修改 `${MAXKB_BASE}/maxkb/.env`，然后运行 `mkctl reload` 重新加载配置。示例数据库密码是原帖中的配置示例，不应直接用于实际部署。

在解压目录中运行安装脚本：

```sh
cd maxkb-v1.2.0-offline
bash install.sh
```

原帖给出的登录地址形式是 `http://您的域名:8080`，并称使用默认管理员账号和密码登录；没有提供默认凭据的具体值。安装包下载地址、默认凭据和完整版本兼容信息未提供。

## 离线升级与服务管理

原帖所述升级方式是下载新版本安装包、上传并解压，再进入新版本目录重新执行安装脚本：

```sh
cd maxkb-v1.x.y-offline
/bin/bash install.sh
mkctl status
```

原帖列出的 `mkctl` 运维命令如下：

```text
mkctl [COMMAND] [ARGS…]
mkctl --help
status    查看 MaxKB 服务运行状态
start     启动 MaxKB 服务
stop      停止 MaxKB 服务
restart   重启 MaxKB 服务
reload    重新加载 MaxKB 服务
uninstall 卸载 MaxKB 服务
upgrade   升级 MaxKB 服务
version   查看 MaxKB 版本信息
```

原帖写明 1.5.0 之前使用 `kbctl`，1.5.0 起使用 `mkctl`。这只是原帖所述版本差异，未提供各版本发行说明。原帖中卸载命令的排版为 `uninstall`，这里按其命令列表语义整理；实际可用子命令应以对应安装包内的 `mkctl help` 输出为准。

## 证据边界

本文整理自 2025 年发布的部署文章。文章包含 4 张图片，图片内容未读取；正文未提供 MaxKB 离线包下载链接，也未给出实际安装日志或独立验证结果。Flexus X 的规格性能及缓存优化效果属于文章中的宣传性描述，不作为本部署步骤的实测结论。

## 公开资源

- 华为云 Flexus X 产品页：<https://www.huaweicloud.com/product/flexus-x.html>
- 1Panel 快速安装脚本：<https://resource.fit2cloud.com/1panel/package/quick_start.sh>

来源：[62161920](https://www.coolapk.com/feed/62161920)（图片与评论未读，未在本库实机复现）。

