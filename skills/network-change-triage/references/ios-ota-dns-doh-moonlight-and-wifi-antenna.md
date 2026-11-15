# iOS 屏蔽更新 DNS、ColorOS DoH、低延迟串流与随身 WiFi 天线检测

整理自酷安社区短帖技术经验（未读配图，参数按原帖记录）。

- **iOS 系统更新屏蔽应急 DNS 方案**：[酷安原帖](https://www.coolapk.com/feed/71582657)（`imgs=2`）— 当旧屏蔽描述文件失效时，可通过 Safari 下载 DNS 版屏蔽配置（原帖提及 `app.pgbug.com`）阻止系统更新联网（会保留红点与设置顶部提示，但不再弹窗催更）。
- **ColorOS 16 / Find X8 DoH DNS 配置列表**：[酷安原帖](https://www.coolapk.com/feed/71348268)（`imgs=3`）— 添加 4 项 DoH 地址：`https://doh.pub/dns-query`、`https://dns.alidns.com/dns-query`、`https://1.1.1.1/dns-query`、`https://223.5.5.5/dns-query`。
- **黑月光（Moonlight）低延迟串流参考配置**：[酷安原帖](https://www.coolapk.com/feed/70950857)（`imgs=0`）— 300M 电信宽带 + 桥接路由 + IPv6 + 5G Wi-Fi 下，1080p 60fps 20Mbps 实测网络延迟 ~1ms、解码延迟 ~3ms（开启低延迟队列与紧凑型 VSync，并改包名优化调度）。
- **随身 WiFi DIY 天线焊接防短路检测**：[酷安原帖](https://www.coolapk.com/feed/59950874)（`imgs=6`）— 嘉立创打板焊接天线后，务必用万用表通断档检查两极是否连焊短路，否则无增强效果。
