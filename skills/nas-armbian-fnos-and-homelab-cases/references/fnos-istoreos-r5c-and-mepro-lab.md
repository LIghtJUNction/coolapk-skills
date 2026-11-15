# NanoPi R5C / N100 飞牛+iStoreOS 虚拟化、Hyper-V 网卡排错与路由器限速踩坑

本参考整理酷安社区中关于 NanoPi R5C iStoreOS 全栈分流、N100 飞牛 OS 虚拟化 iStoreOS 网口直通、笔记本 Hyper-V 飞牛无线改 Realtek 有线网卡排错、家用路由器限速 Bug 迫使切换爱快软路由，以及零刻 ME Pro 的记录。

---

## 1. NanoPi R5C + iStoreOS 全栈分流与 N100 飞牛虚拟化拓扑

- **NanoPi R5C + iStoreOS**（https://www.coolapk.com/feed/70791158）：记录在友善 `NanoPi R5C` 上同时配置国内直连分流、Tailscale 远程组网、IPv6 DDNS 反向代理与 DNS 去广告的共存踩坑与排错实录。
- **N100 飞牛 OS + 虚拟化 iStoreOS**（https://www.coolapk.com/feed/70660660）：N100（4×2.5G 网口）底层装飞牛 OS，将 `3 个 2.5G` 网口直通给虚拟机 iStoreOS 负责联通千兆拨号（动态 IPv4+IPv6 双公网），保留 `1 个 2.5G` 给飞牛自身，配合 Lucky 与南墙 WAF；家用硬路由仅作纯 AP 发射 5GHz Wi-Fi（关闭 DHCP）。

---

## 2. 笔记本 Hyper-V 飞牛 fnOS 无线转有线排错与路由器限速 Bug

- **Hyper-V 飞牛 fnOS 无线网卡换 Realtek 有线**（https://www.coolapk.com/feed/70804276）：笔记本 Hyper-V 虚拟机跑飞牛通过无线连接路由器时速率仅 `100–300Mbps` 且经常无法获取 IPv6；改用杂牌百兆网卡导致虚拟交换机绑定异常，最终更换带驱动的 **Realtek USB-A 有线扩展坞**并将虚拟网卡绑定至有线网卡，同时在光猫改桥接+路由器 IPv6 `Passthrough` 关防火墙后恢复公网 IPv6。
- **小米 BE5000 / 中兴 BE5100Pro+ 限速 Bug 踩坑**（https://www.coolapk.com/feed/71852937）：实测小米 BE5000 开启设备限速必须开 QoS，导致内外网速度均跌至约 `400Mbps`，且米家 App 最低仅能设 `128KB` 无法远程精细控速；中兴巡天 BE5100Pro+ 同样存在限速降速及断网后无法访问局域网问题，最终改用爱快软路由。
- **零刻 ME Pro x 飞牛 fnOS**（https://www.coolapk.com/feed/69854167）：飞牛硬件联盟继 ME mini 后的进阶机型记录（详情见原帖图文）。
