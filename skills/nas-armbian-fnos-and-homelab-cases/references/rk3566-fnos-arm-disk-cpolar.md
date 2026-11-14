# RK3566 刷入 ARM 版飞牛 NAS、迁移 SATA 启动与 cpolar

## 适用与证据范围
原帖以 RK3566 设备为例，报告 ARM 版飞牛 NAS 镜像刷写和 SATA 系统迁移过程。文章有 9 张图片但图片未提供，无法复核板上短接点图和界面操作。硬件适配、命令效果均为原帖描述，未独立验证。

## 硬件版本与刷写
- 原帖列出设备版本 1.0、1.1、2.0、2.1，并称需拆机查看主板标记。
- 演示设备为 1.1 版：短接 `GND` 与 `1V8` 金属点；原帖称 1.0 版适用相同位置。2.0、2.1 版短接位置不同，具体位置仅在未提供的图片中。
- 刷机时不要接电源。首次刷机拆机短接；后续可按住 Reset 并接数据线。
- Windows 上安装 `RKDevTool.zip` 中 `DriverAssitant_v5.0/DriverInstall.exe` 驱动。打开 `RKDevTool.exe`，首次刷机勾选两个选项；为 `LoaderToDDR` 选择对应文件，再在 `system` 行选择解压后的 `.img` 镜像，不能选择 `.rar` 压缩包。短接后连接电脑，等待工具显示 `MASKROM`，松开镊子并执行刷写。
- 原帖列出的镜像文件名：`fnos_Mainland-PE_arm_1.0.0_rk3566-null_258.img`。资源通过 123 云盘提供，提取码 **1314**，但没有实际分享 URL。
- 刷机后原帖在路由器设备列表看到名为 `debian` 的设备，示例管理地址 `http://192.168.50.211`。

## 克隆系统至 SATA 并切换启动
插入 SATA 硬盘，在飞牛设置中启用 SSH。连接方式：

```sh
ssh 飞牛用户名
sudo -i
```

原帖的一键管理工具命令如下；URL 中插入的“删”按原帖明确指示移除：

```sh
curl -fsSL https://gitee.com/jun-wan/script/raw/master/rk3566-fnos-arm-tools/xxfn-tool.sh -o /usr/local/bin/xxfn-tool && chmod +x /usr/local/bin/xxfn-tool && xxfn-tool
```

工具菜单操作：
1. 输入 `7` 初始化 SATA 分区。默认给分区 1 分配 `32G`，也可手动指定；输入 `y` 确认。
2. 输入 `13`，再输入 `y` 确认克隆系统。
3. 输入 `1`，选择从 `[SATA 硬盘]` 启动，输入 `y` 确认并立即重启。
4. 重启后在存储空间管理中创建其余 SATA 分区。**不要格式化内置硬盘**，原帖称这会导致系统无法启动。

## cpolar
原帖安装命令及服务检查：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

Web UI 使用飞牛 IP 加 `9200` 端口；原帖示例 `http://192.168.50.211:9200`。登录后编辑 `website` 隧道或新建隧道，查看在线隧道中的 HTTP、HTTPS 地址。

免费随机域名据原帖描述约每 24 小时变化；固定二级子域名需升级任意付费套餐。在预留页面设置地区、名称及可选描述，再编辑隧道选择二级子域名。示例地区 `China Top`、子域名 `fnos01`，每账号唯一。

## 来源
- 原帖：https://www.coolapk.com/feed/69681846
- 公开脚本：https://gitee.com/jun-wan/script/raw/master/rk3566-fnos-arm-tools/xxfn-tool.sh
- 资源边界：123 云盘分享 URL 未提供；2.0/2.1 短接点图片未提供。
