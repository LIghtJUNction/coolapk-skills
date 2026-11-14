# 玩客云刷 Armbian 并部署 CasaOS、小雅 Alist

## 适用与证据范围
原帖以玩客云（Onecloud）为例，通过 Amlogic USB Burning Tool 刷入 Armbian，再装 CasaOS、小雅 Alist 和 cpolar。刷机触点图片、安装界面未提供；成功状态均为原帖报告。

## 刷入 Armbian
准备 Windows 电脑、网线和玩客云。原帖资源包通过 123 云盘分享，提取码 **1314**，但没有实际分享 URL。线刷工具版本要求为 **Amlogic USB Burning Tool v2.1.6.8**；原帖称 v2.2 刷 Armbian 会发生“等待命令结果超时”。镜像文件名模式为 `Armbian-xxxx_Onecloud_xxxx.burn.img`。

原帖步骤：拆机找到短接点；USB 公对公线连接主板和电脑；镊子持续短接触点；给主板接电源，等工具显示连接成功后松开短接；导入镜像并点击“开始”。文章报告约 3–5 分钟完成。首次短接后，后续刷机通常可按住 Reset，不必再次拆机短接。

启动 Armbian 后接网线和电源，等待 1–2 分钟，从路由器或局域网扫描工具查询 IP。SSH 示例：

```sh
ssh root@192.168.50.170
```

原帖称首次连接时设置 root 密码；询问是否创建新用户时按 `Ctrl+C` 退出。

## 安装 CasaOS 与基础配置
在 Armbian 终端执行（URL 中原帖明确插入的“删”已移除）：

```sh
curl -fsSL https://get.casaos.io | sudo bash
```

原帖示例 Web 地址为 `http://192.168.50.150`，IP 以安装输出为准。首次访问创建管理员账户。

可选的一键初始化脚本：

```sh
curl -fsSL https://gitee.com/jun-wan/script/raw/master/onecloud/armbian_init.sh | bash
```

原帖称脚本用于查看设备信息、配置时区和添加国内 Docker 镜像源；没有给出镜像源具体内容。

## 安装小雅 Alist
原帖安装命令如下（保留 `--insecure` 和命令替换语法）：

```sh
bash -c "$(curl --insecure -fsSL https://ddsrem.com/xiaoya_install.sh)"
```

原帖报告玩客云是 `armv7` 架构，通用兼容性检查会提示架构不支持。按原帖流程选择两次数字 `1`；配置目录处回车并输入 `Y`。随后通过页面获取 refreshToken，继续输入 `Y` 后获取 Open Token；再输入 `Y` 获取转存目录，将网盘转存目录 ID 粘贴给脚本。原帖所列令牌页面：

- refreshToken 页面：https://aliyuntoken.vercel.app/
- Open Token 页面：https://opentoken.xiaoya.pro/

继续按默认回车，中途设置 `dav` 账号密码。原帖称脚本会拉取镜像并启动，访问玩客云 IP 的 `5678` 端口；使用账号 `dav` 和自行设置的密码登录。令牌页面和账号授权行为未在本文独立核验。

## 安装 cpolar 并建立隧道

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

原帖以 `active (running)` 表示服务运行。Web UI 地址为设备 IP 加 `9200`，示例 `http://192.168.50.150:9200/`。

原帖称默认有 SSH（TCP 22）和 website（HTTP 8080）两条隧道。编辑 `website`，名称示例 `xiaoyalist01`，本地端口设为 `5678`，地区选择 `China Top`；在线列表会显示 HTTP 与 HTTPS 地址。免费随机域名约每 24 小时变化；固定二级子域名需升级任意付费套餐，在预留名称后将隧道域名类型改为二级子域名。示例名称 `xiaoyalist01`，每账号唯一。

## 来源
- 原帖：https://www.coolapk.com/feed/69283016
- CasaOS 初始化脚本：https://gitee.com/jun-wan/script/raw/master/onecloud/armbian_init.sh
- 小雅安装脚本：https://ddsrem.com/xiaoya_install.sh
- refreshToken 页面：https://aliyuntoken.vercel.app/
- Open Token 页面：https://opentoken.xiaoya.pro/
- 资源边界：123 云盘 URL 未提供；图片未提供。
