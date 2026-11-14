# Windows 上用 Nginx 部署 tang-love 并通过 cpolar 暴露

## 安装与本机验证

原帖以 Windows 11 为例，并称步骤适用于 Windows 10/8/7。安装 cpolar 后可在 CMD 运行：

```bat
cpolar version
```

安装包页面：<https://www.cpolar.com/download>；官网：<https://www.cpolar.com/>。原帖称 Web UI 可在 `http://127.0.0.1:9200` 打开。默认隧道包括 `remoteDesktop`（TCP 3389）和 `website`（HTTP 8080）；原帖描述 HTTP 隧道会生成 HTTP 与 HTTPS 两个公网地址。

Nginx 下载页面：<https://nginx.org/en/download.html>。示例版本为 1.28.0，下载 `nginx-1.28.0.zip`，解压后运行 `nginx.exe`，允许防火墙访问，再访问 `http://127.0.0.1`；原帖以出现 `Welcome to nginx!` 作为启动检查。

## 部署 tang-love

项目仓库：<https://github.com/37tt/tang-love.git>。可下载 ZIP，或使用 Git 克隆。将项目解压到 Nginx 的 `html` 目录，再把 `tang-love-main` 内文件和目录移至上一层；出现重名时按原帖操作替换。之后通过 `http://127.0.0.1` 本地检查。原帖未给出实际公网 URL。

## cpolar 隧道

登录 cpolar Web UI，编辑 `website` 隧道，将本地端口从 `8080` 改为 `80` 并更新。原帖报告在线列表中有 HTTP、HTTPS 地址，并称 HTTPS 访问成功。随机域名约 24 小时变化；固定域名需先在 <https://dashboard.cpolar.com/reserved> 预留二级子域名，再编辑 `website` 隧道选择二级子域名并更新。原帖示例名 `love01` 仅是示例，子域名按账号预留且唯一。

## 证据边界

原帖截图与评论未读取。Nginx 启动、项目呈现和隧道访问结果均为原帖报告，未独立验证；文中未提供公网地址、Nginx 配置文件内容或 cpolar 隧道完整参数。

来源：[67565212](https://www.coolapk.com/feed/67565212)（图片与评论未读，未在本库实机复现）。

