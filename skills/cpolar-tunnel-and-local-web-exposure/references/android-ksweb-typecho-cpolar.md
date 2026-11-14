# Android 上用 KSWEB 部署 Typecho 并通过 Termux cpolar 访问

## 设备与后台运行条件

原帖列出的条件：Android 7.0+；Termux 0.118.x 支持 Android 7+；Root 非必需；建议 2GB+ RAM、2GB+ 可用空间和 ARM64。示例设备为 Android 9、8GB RAM / 256GB 存储的手机，这只是发帖设备记录，不表示其他设备已适配。建议稳定 Wi-Fi、尽量接电，并对 KSWEB 与 Termux 关闭电池优化/后台限制。KSWEB 中启用自动启动和锁定 Wi-Fi。

## KSWEB、Typecho 与数据库

原帖称 KSWEB 默认安装 PHP、Apache 和 MySQL，Apache 默认前端端口为 8000；安装 phpAdmin 后，状态页会出现 8001 端口。示例入口：`http://localhost:8000`、`http://localhost:8001`。原帖提到 phpAdmin 使用用户名 `root`、空密码登录；这是原帖示例环境配置，不应视为所有安装的默认值。

将 Typecho 源码文件复制到 KSWEB 的 `htdocs`。在 phpAdmin 创建数据库；示例数据库名 `xy`，字符集/排序规则 `utf8mb4_bin`。回到 Typecho 安装向导配置 MySQL、站点信息和管理员账号。将 Apache 绑定 IP 从 `localhost` 改为 `0.0.0.0` 后，局域网设备可通过手机局域网 IP 和 8000 端口访问；原帖示例地址为 `http://192.168.50.114:8000`，该 IP 仅是作者当时网络示例。

原帖认为 KSWEB 内置 PHP 8.2.6 与 Joe 主题不兼容，切换到 PHP 7.4.23 后页面错误消失；并称其他推荐 PHP 7.4 的主题也可采用该版本。这是文章所用版本下的报告，不代表当前主题版本兼容性结论。

## Termux 安装 cpolar

在 Termux 执行以下原帖命令；原文明确要求移除插入的“删”字：

```sh
mkdir -p $PREFIX/etc/apt/sources.list.d

echo "deb [trusted=yes] http://termux.cpolar.com termux extras" >> $PREFIX/etc/apt/sources.list.d/cpolar.list

pkg update
pkg install cpolar
pkg install termux-services
```

安装 `termux-services` 后关闭并重新打开 Termux，再执行：

```sh
sv up cpolar
sv-enable cpolar
```

原帖称 Web UI 使用手机浏览器访问本地 9200 端口。原文未给出 cpolar 仓库的下载包校验信息；`[trusted=yes]` 是原帖配置中的字面参数，未独立验证仓库或软件包。

## Typecho 隧道与站点地址

在 cpolar Web UI 创建本地端口 `8000` 的 `typecho-8000` 隧道。原帖报告首次公网页面内容出现但样式缺失，原因是 Typecho 后台站点地址仍为本地地址；将站点地址改成 cpolar 公网地址后样式正常。随机域名约每 24 小时变化，地址变化后需同步更新 Typecho 站点地址。固定二级子域名可在 cpolar 预留后绑定 `typecho-8000` 隧道，并把 Typecho 站点地址改为固定域名。

## 主题安装与资源

将主题放到 `htdocs/usr/themes`。Joe 示例解压后原帖称会出现两层 `Joe` 目录，需把内层主题目录移到上一层，再在 Typecho 后台“更换外观”启用。原帖列出的主题：Butterfly、Cuckoo、GreenGrapes、Joe。

原帖公开资源：<https://www.123865.com/s/3jOKVv-9JNzH?pwd=1314>，分享提取信息为 `1314`。文章称该包包含 Termux、KSWEB、Typecho 源码和主题；未下载或核验包内容。原帖参考文章：<https://www.cpolar.com/blog/an-old-phone-turns-into-a-server-in-an-instant-building-a-typecho-blog-on-android>。cpolar 预留页：<https://dashboard.cpolar.com/reserved>。

## 证据边界

来源为酷安原帖文字及其提供的链接上下文。图片、演示和评论未读取。主题兼容性、端口访问、服务常驻与公网访问均为原帖报告，未独立测试；没有补造各主题仓库或下载链接。

来源：[67711225](https://www.coolapk.com/feed/67711225)（图片与评论未读，未在本库实机复现）。

