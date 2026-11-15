# 本地 AdGuardHome / SmartDNS 模块、私人 DoH 描述文件与系统广告拦截

本参考整理酷安社区中通过私人 DoH 描述文件（iOS/macOS）、88DNS、本地 SmartDNS / AdGuardHome Root 模块、四层拦截模块及 Shizuku + Canta 免 Root 方案实现 DNS 过滤与系统广告/遥测阻断的实操与边界。

---

## 1. 苹果描述文件一键 DoH 与 88DNS 公测服务

- **来源**:
  - https://www.coolapk.com/feed/75039521（SaaS 控制台 Apple 描述文件生成）
  - https://www.coolapk.com/feed/75018500（88DNS 全链路自研 DoH/DoT 公测）
- **要点**:
  - `75039521` 通过生成 iPhone/iPad 描述文件让 iOS/macOS 全局走 DoH（安装提示“未签名”属正常，Token 重建后需重装），并修复广告拦截缺 SOA 记录导致客户端高频重试及复杂 CNAME 返回 `SERVFAIL` 的问题。
  - `75018500` 提供 DoH/DoT 加密解析与可视化过滤，免费版限单档案每月 `30 万`次查询、不限设备数，误杀可通过 AdGuard 语法白名单放行。

---

## 2. Android Root 本地 DNS 模块（SmartDNS 1.1 / AdGuardHome）与四层拦截

- **来源**:
  - https://www.coolapk.com/feed/75006429（SmartDNS 1.1 模块）
  - https://www.coolapk.com/feed/74884697（本地 AdGuardHome 模块）
  - https://www.coolapk.com/feed/74872599（四层广告与隐私回传拦截模块）
  - https://www.coolapk.com/feed/74858202（HyperOS 3 遥测组件拦截记录）
- **SmartDNS 模块（`75006429`）**:
  - 兼容临时 Root，不改系统分区、不依赖元模块，配置位于 `/data/adb/smartdns/etc/smartdns.conf`，Web 仪表盘 `127.0.0.1:6080`（`admin`/`password`）。
  - 修复 KernelSU 热重启死机 Bug：热重启前需先点模块“执行”停掉 SmartDNS。
- **AdGuardHome 模块（`74884697`）**: 以系统服务运行于后台实现 DNS 层全局过滤（含热点共享设备），不占用应用层过滤通道。
- **四层拦截与 HyperOS 3 阻断（`74872599` / `74858202`）**: 对仅改 hosts 无法拦住 IP 直连或系统级上报的场景，需结合组件/网络层多维度阻断（具体模块包未在本库验证）。

---

## 3. 免 Root 方案：Shizuku + Canta 卸载澎湃广告组件与私人 DNS

- **来源**: https://www.coolapk.com/feed/74828089
- **操作组合**:
  1. 用澎湃工具箱关闭系统广告开关。
  2. 通过 Shizuku 授权 **Canta** 卸载或替换广告/埋点组件（`analytics`、`msa`、`AdSolution`，以及系统浏览器、应用商店、软件包安装程序）。
  3. 系统设置 → 网络和互联网 → 私人 DNS → 指定主机名填写 `dns.23-4.cn`（可能影响部分网络访问速度，按需取舍）。
