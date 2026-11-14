# Cudy TR3000（256M）通过 USB 使用中兴 F50 上网

## 适用设备与前提

原帖对象为刷入 Kwrt 的 Cudy TR3000 256M 版本和中兴 F50。Kwrt 默认未集成文中所需 USB 网络驱动；F50 以 USB 连接后，原帖环境未识别出 `eth2`。作者建议使用较短 USB 数据线，避免供电不足；路由器供电为 5V3A。以上是原帖的设备及现象记录，未独立验证其他硬件批次或固件版本。

驱动包括 `kmod-mii`、`kmod-usb-net`、`kmod-usb-net-cdc-ether`、`kmod-usb-net-rndis`。

## 在线安装驱动

先让路由器通过上级网络临时联网，再以 root 登录 Kwrt 终端。原帖给出的终端地址为 `http://10.0.0.1:7681/`（原文插入“删”标记，按其说明移除）。执行：

```sh
opkg update
opkg install kmod-mii kmod-usb-net kmod-usb-net-cdc-ether kmod-usb-net-rndis
```

等待 F50 启动后，在 Kwrt 接口配置中选择 `eth2`。

## 离线安装驱动

将 ipk 上传到路由器，再以 root 在终端执行：

```sh
opkg install /kmod-usb-net-pack_6.6.92~1-r1_aarch64_cortex-a53.ipk

modprobe mii
modprobe usbnet
modprobe cdc_ether
modprobe rndis_host

lsmod | grep -E 'mii|usbnet|cdc_ether|rndis_host'

echo -e "mii\nusbnet\ncdc_ether\nrndis_host" > /etc/modules.d/99-usb-net-pack
```

`lsmod` 无输出表示原帖所列模块未显示为已加载。原帖没有说明这个离线包的公开下载直链；相关材料见下方 123 云盘分享。包名中标注的内核版本和架构为 `6.6.92~1`、`aarch64_cortex-a53`，不能据此推定适用于其他固件构建。

## 配置 F50 为 WAN 设备

在 Kwrt 的“网络 > 接口”中编辑 `wan` 和 `wan6`，将设备都改为 `eth2`，然后确认更改并“保存并应用”。原帖报告其环境联网成功。

## cpolar 安装及隧道

原帖给出的命令按其明确说明删除嵌入的“删”字后恢复如下。命令中的 `$(...)` 是 shell 命令替换，照原文保留：

```sh
wget -O cpolar-public.key http://openwrt.cpolar.com/releases/public.key
opkg install https://downloads.openwrt.org/releases/24.10.0/packages/aarch64_cortex-a53/base/usign_2020.05.23~f1f65026-r1_aarch64_cortex-a53.ipk
opkg-key add cpolar-public.key
echo "src/gz cpolar_packages http://openwrt.cpolar.com/releases/packages/$(. /etc/openwrt_release ; echo $DISTRIB_ARCH)"  >>  /etc/opkg/customfeeds.conf
opkg update
opkg install cpolar
opkg install luci-app-cpolar
opkg install luci-i18n-cpolar-zh-cn
```

恢复后的 cpolar 主机名依据原文逐字符移除“删”标记；未独立核验下载内容。安装顺序包含添加公钥、添加软件源、`opkg update` 更新索引，再安装主程序、LuCI 界面和中文包。原帖称若 LuCI 服务菜单未出现，可断电重启后查看。

将默认 `website` 隧道配置为 Kwrt 管理页（80 端口）。为 UFI-TOOLS 建立另一条隧道，原帖给出的地址为 `http://192.168.0.1:2333`，并要求确认 Kwrt 能访问 F50 管理地址。文章报告两条服务各有 HTTP、HTTPS 在线地址，并称访问成功；具体公网域名未给出。

免费随机域名约 24 小时重置。固定二级子域名需先在 cpolar 预留，再编辑相应隧道选择二级子域名并更新。子域名是账号独有的，原帖示例不能当作通用域名。

## 公开资源

- 123 云盘资源包：原帖提供永久分享链接，提取码 `1314`。原文未给出可见 URL，无法在此还原实际分享地址。
- Kwrt 终端入口：`http://10.0.0.1:7681/`（原文标记恢复）
- Kwrt 管理入口：`http://10.0.0.1`（原文标记恢复）
- UFI-TOOLS 示例管理入口：`http://192.168.0.1:2333/`（原文标记恢复）

## 证据边界

来源为酷安原帖文字。图片和评论未读取；步骤中的界面截图、cpolar 账户入口及实际隧道域名缺失。安装成功、`eth2` 出现和远程访问结果均为原帖报告，未独立复测。

来源：[68290109](https://www.coolapk.com/feed/68290109)（图片与评论未读，未在本库实机复现）。

