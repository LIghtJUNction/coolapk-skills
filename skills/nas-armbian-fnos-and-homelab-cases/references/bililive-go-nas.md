# 在飞牛 NAS 上部署 Bililive-go 自动录制直播

## 适用范围与证据边界
原帖以飞牛 NAS、Docker 和一台 RK3566 ARM64 设备为例，称设备内存约 4 GB、eMMC 约 8 GB，录像存储另接 SATA 硬盘。正文报告约 4 小时直播生成超过 20 GB 视频文件；这是原帖单次使用报告，不代表固定码率或通用容量估算。图片未读取，评论未提供。

## 工具机制与项目
Bililive-go 使用 Go 编写。添加直播间地址后，服务后台轮询/监控开播状态，开播时自动获取直播流并录制，下播后停止并保存到指定目录，再继续等待下一次开播。它直接保存直播流，不要求一直打开直播网页。原帖列举哔哩哔哩、抖音、斗鱼和虎牙等平台；具体支持范围应以项目文档为准。

项目仓库：<https://github.com/bililive-go/bililive-go/>

## Docker 部署配置
在飞牛 NAS Docker 镜像仓库搜索 `Bililive-go`，下载 `chigusa/bililive-go`，标签使用 `latest`。创建容器时可勾选开机自动启动；端口映射左侧为 NAS 主机端口、右侧为容器端口：容器端口保持 `8080`，主机端口若冲突可改为 `8081` 或 `8082`。将 NAS 录像目录挂载到容器路径 `/srv/bililive`，权限为读写，并勾选创建后启动。浏览器访问映射后的主机端口，进入 Web 管理页面添加直播间地址。

原帖说明可同时监控多个直播间，录制文件可从 Web 页面查看，也可在 NAS 文件管理器中访问挂载目录。长时间、多路或高画质录制会快速占用磁盘空间；原帖记录的一段近 4 小时录制超过 20 GB。

## cpolar 远程访问
启用飞牛 NAS 的 SSH，从终端连接后切换 root。原帖命令为：

```sh
ssh rk3566@192.168.50.212
sudo -i
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

`https://get.cpolar.sh` 是按原帖明确要求删除插入的“删”字恢复所得。cpolar Web UI 示例：`http://192.168.50.212:9200`。在“隧道管理 > 隧道列表”编辑或新建 HTTP 隧道，原帖建议本地端口 `8080`，地区选择 `China VIP`；按在线隧道列表提供的 HTTP/HTTPS 地址访问。

免费随机域名会周期性变化。固定二级子域名流程是先在 cpolar 后台预留名称，再编辑 `bililive` 隧道，将域名类型改为二级子域名并填入名称。公网管理页面可在隧道高级设置中配置 HTTP Auth，格式为 `用户名:密码`。它是公网隧道的访问认证，不是 Bililive-go 内建用户系统；原帖建议使用独立密码并优先通过 HTTPS 访问。

## 来源
- 原帖：<https://www.coolapk.com/feed/73023624>
- Bililive-go 项目：<https://github.com/bililive-go/bililive-go/>
- cpolar 安装脚本：<https://get.cpolar.sh>（按原帖明确规则移除插入字后恢复）
