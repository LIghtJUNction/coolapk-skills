# W1701K 的 TTL、Chainloader 与 UBI 安装

仅针对原帖的 Quantum Fiber W1701K 流程。原帖没有说明硬件修订、原厂 U-Boot 版本或固件具体版本；不能把闪存地址套到其他路由器。

## 资源和文件角色

- [原帖文件分享](https://www.123865.com/s/sELVVv-bxw7d?pwd=6666#)，公开提取码 `6666`。
- [UBI installer 项目](https://github.com/ZqinKing/w1700k-ubi-installer)。仓库路径按原链接保留，不据仓库名推断其他机型适配。
- [升级固件发布页](https://github.com/ZqinKing/wrt_release/releases)。

原帖提供链接，尚未下载或核对文件校验值。

| 文件 | 使用阶段 |
|---|---|
| `ubi-chainload-uboot.itb` | 原厂 U-Boot 加载并写入的 Chainloader |
| `immortalwrt-airoha-an7581-gemtek_w1701k-ubi-initramfs-installer.itb` | Chainloader 菜单经 TFTP 获取的 installer |
| `ubi-initramfs-installer.itb` | 备用 TFTP push 流程使用的重命名 installer |
| `ubi-squashfs-sysupgrade.itb` | 初次安装完成后在 OpenWrt 内升级 |

## 串口和第一阶段网络

准备 USB 转 TTL、杜邦线、串口终端及 TFTP 工具。原帖使用 CH343；串口波特率通常为 `115200`。

| TTL 模块 | 路由器 |
|---|---|
| GND | GND |
| RX | TX |
| TX | RX |

原帖要求电平跳线为 `3.3V`，通常不接 VCC，避免供电冲突。具体主板针脚位置未在正文标明；原帖提醒背部有交流电插脚，并建议将 TTL 引出后恢复后盖。

电脑有线网卡设为 `192.168.1.10`、掩码 `255.255.255.0`，网线接路由器的黄色 1G 口。固件放入电脑 TFTP 服务目录。这里电脑向路由器提供文件；不要将此方向与后面的 TFTP push 混淆。

路由器上电，启动信息出现时按空格或回车打断启动，进入原厂 U-Boot。

## 写入 Chainloader

以下命令运行在路由器 **U-Boot 串口终端**，不是电脑 shell。确认对应文件传输完成后，才进入擦写步骤。`flash erase/write` 会改写设备闪存。

```text
setenv serverip 192.168.1.10 ; setenv ipaddr 192.168.1.1 ; tftpboot 0x89000000 ubi-chainload-uboot.itb
setenv one flash read 0x600000 0x100000 \$loadaddr
setenv two "; bootm"
setenv bootcmd "$one$two"
saveenv
flash erase 0x600000 0x100000
flash write 0x600000 0x100000 0x89000000
reset
```

| 参数 | 原帖取值 |
|---|---|
| TFTP 服务端 | `192.168.1.10` |
| 路由器地址 | `192.168.1.1` |
| 内存加载地址 | `0x89000000` |
| 闪存偏移 | `0x600000` |
| 擦写长度 | `0x100000` |

`setenv one` 中 `$loadaddr` 前的反斜杠按原帖保留。若 TFTP 显示连续 `T T T...`，原帖判作超时，要求重启设备重新开始。重启后应进入 Chainloader。

## Chainloader 菜单安装

1. 电脑 IP **改为 `192.168.1.2`**，掩码仍为 `255.255.255.0`。
2. TFTP 服务目录内保留完整文件名 `immortalwrt-airoha-an7581-gemtek_w1701k-ubi-initramfs-installer.itb`。
3. 串口菜单选择第 4 项 `Boot installer via TFTP.`，由 Chainloader 从 `192.168.1.2` 获取 installer。
4. 等待安装完成。已有 UBI 布局时可能显示 `Existing UBI layout detected. Proceed and overwrite? (yes/no)`；输入 `yes` 会继续格式化，不是普通重启确认。

两个阶段的电脑地址不同：写 Chainloader 用 `.10`，菜单安装用 `.2`。

## 备用：原厂 U-Boot 的 TFTP push

菜单流程不能使用时，原帖另给下列路径：将 installer 重命名为 `ubi-initramfs-installer.itb`，重新上电进入原厂 U-Boot。

U-Boot 先设置加载地址：

```text
setenv loadaddr 0x89000000
```

电脑端使用原帖的 Windows 风格 TFTP 客户端命令；`-i` 不是这里已验证的 Linux/macOS 通用参数：

```bat
tftp -i 192.168.1.1 put ubi-initramfs-installer.itb
```

传完后回到 U-Boot：

```text
bootm 0x89000000
```

原帖称 installer 会迁移并安装初始 OpenWrt；之后通过系统更新使用 `ubi-squashfs-sysupgrade.itb`。迁移内部过程、失败后的分区恢复方法及具体版本未提供。

