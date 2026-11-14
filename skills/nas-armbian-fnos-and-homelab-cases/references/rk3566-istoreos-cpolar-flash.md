# RK3566 刷入 iStoreOS 并配置 cpolar

## 适用与证据范围
原帖以 RK3566 设备为例，记录在 Windows 电脑上使用 RKDevTool 将 iStoreOS 镜像写入内置存储，并通过 cpolar 暴露管理界面的流程。文章有 9 张图片，但图片内容未提供；短接点图示和刷机工具勾选项等无法从文字确认。步骤与效果属于原帖报告，未独立验证。

## 硬件与准备
- RK3566 设备；12V/2A 或更高功率电源；Windows 电脑；Type-C 数据线、网线、HDMI 线；拆机工具和镊子。
- 原帖称资源包通过 123 云盘提供，提取码为 **1314**，但正文未给出实际分享 URL，因此无法从本文独立取得资源。
- 驱动包中的 `DriverAssitant_v5.0` 文件夹内运行 `DriverInstall.exe`，点击驱动安装。

## 刷机
1. 首次刷机需拆机短接进入刷机模式；后续刷机可用卡针按住 Reset，连接 Type-C 线，无需再次拆机。原帖未提供所引用拆机教程的 URL。
2. 打开 `RKDevTool.exe`。原帖描述勾选第二项（第一项不用勾），在 `system` 行末尾空格处选择解压后的镜像文件；正文提到“固件镜像压缩包”，但随后要求选择解压后的文件，且明确提示选镜像而非压缩包。
3. 按住 Reset 并连接设备与电脑；工具识别为 `LOADER` 设备后点击执行。工具提示“下载完成”后，原帖视为写入成功。
4. 拔掉 Type-C，接电源和网线；从路由器设备列表查找 iStoreOS 的 IP。原帖示例地址为 `http://192.168.50.247`（原文插入“删”字，按原帖明确指示移除；这是示例局域网地址）。登录默认无密码，随后按提示设置密码。

## 在 iStoreOS 安装 cpolar
登录 iStoreOS 终端，使用 root 用户和已设置的 root 密码。原帖命令中的“删”是明确插入的屏蔽标记，按原帖说明逐字移除后如下：

```sh
wget -O cpolar-public.key http://openwrt.cpolar.com/releases/public.key
opkg-key add cpolar-public.key
echo "src/gz cpolar_packages http://openwrt.cpolar.com/releases/packages/$(. /etc/openwrt_release ; echo $DISTRIB_ARCH)" >> /etc/opkg/customfeeds.conf
```

之后原帖要求更新仓库索引，但没有提供对应命令。随后安装三个包：

```sh
opkg install cpolar
opkg install luci-app-cpolar
opkg install luci-i18n-cpolar-zh-cn
```

安装后刷新 iStoreOS 首页，在“服务”菜单打开 cpolar Web UI。原帖称默认隧道列表有两条，编辑 `website` 隧道并按需更新；在线隧道列表会显示 HTTP 与 HTTPS 地址。

## 固定二级子域名
原帖称免费随机域名约每 24 小时变化；升级任意付费套餐后，可在 cpolar 预留页面填写地区、名称和可选描述，保留二级子域名，再编辑隧道将域名类型改为“二级子域名”并填写已保留名称。示例地区为 `China Top`、名称 `istoreos01`；名称按账号唯一，需以自己的预留结果为准。

## 来源
- 原帖：https://www.coolapk.com/feed/70421510
- 资源边界：正文提到 123 云盘永久链接但未包含 URL；未提供评论或外链内容。
