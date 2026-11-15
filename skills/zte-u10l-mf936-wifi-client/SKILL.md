---
name: zte-u10l-mf936-wifi-client
description: 按固件条件配置 ZTE U10L 或 MF936 的 2.4GHz WPA2 Wi-Fi 客户端及静态 IP，区分局域网可达与转发互联网。
---

# ZTE U10L / MF936 连接 2.4 GHz Wi-Fi 客户端模式

## 适用范围与限制

原帖针对 ZTE U10L / MF936，使用其固件中的 `wlan0` 接口，通过 ADB shell 配置无线客户端。原帖指出该方式连接 2.4 GHz Wi-Fi；配置示例采用 WPA2-PSK。接口名称和命令行为依赖固件，不能假定其他设备也使用 `wlan0`。

原帖示例静态地址为 `192.168.10.5/24`，这不是所有网络都适用的固定地址。执行前需确认目标网络的地址段、地址冲突情况和设备接口。

原帖前置步骤为关闭 Wi-Fi 并进入设备 ADB shell，未给出具体开关命令。

## 创建配置文件

在原帖指定的工作目录 `/mnt/userdata/etc_rw/wifi` 中执行以下命令；原帖没有给出进入该目录的命令，下面的相对路径 `wpa.conf` 依赖当前工作目录正确。

```sh
touch wpa.conf
cat <<'EOF' > wpa.conf
ctrl_interface=/etc_rw/wifi/sockets
update_config=1
pmf=1
eapol_version=1
ap_scan=1

network={
    ssid="<Wi-Fi SSID>"
    psk="<Wi-Fi 密码>"
    key_mgmt=WPA-PSK
    pairwise=CCMP
    group=CCMP
    proto=RSN
    priority=1
}
EOF
```

此处将原帖的 `EOF` 改为带引号的 heredoc 分隔符，避免配置中的 `$` 等字符被 shell 展开。将 `<Wi-Fi SSID>` 和 `<Wi-Fi 密码>` 按 wpa_supplicant 配置语法替换，含引号、反斜杠的值须正确转义。原帖展示的是 WPA2 示例；这里不包含真实凭据。

## 启动客户端并检查地址

```sh
wpa_supplicant -B -i wlan0 -c ./wpa.conf
ip addr show wlan0
```

如果接口没有取得 IP，原帖示例手动设置静态地址：

```sh
ip addr add 192.168.10.5/24 dev wlan0
```

只有确认该地址适合当前网络且未被占用时才使用示例地址。原帖随后只验证了笔记本能 ping 通 `192.168.10.5`。

## 路由验证边界

客户端接口连上 Wi-Fi、配置静态 IP 或能被笔记本 ping 通，都不能单独证明设备已转发互联网流量。要作为旁路由使用，还需要验证设备上的转发与 NAT（如网络设计需要），并确认上游和客户端的返回路由；原帖没有提供这些配置或验证。因此，仅把客户端网关指向设备 IP 并不能保证互联网路由可用。

## 来源

[酷安技术帖](https://www.coolapk.com/feed/64876343)

## 补充案例

- [LR511C 随身 Wi-Fi（MDM9615M）设备枚举与接口线索](references/lr511c-mdm9615m-rndis-serial.md)
