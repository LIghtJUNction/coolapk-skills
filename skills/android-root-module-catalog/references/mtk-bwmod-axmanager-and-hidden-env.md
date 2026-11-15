# `mtk-bwmod` 路由优先级、AxManager 免 Root 模块、Droidspaces 旧机利用与密钥避坑

本参考整理酷安社区中关于联发科带宽优化模块 `mtk-bwmod`、AxManager 免 Root 模块、旧手机 Droidspaces 改造利用以及一加 Ace 5 Pro 环境隐藏密钥避坑的记录。

---

## 1. `mtk-bwmod v2.0`：天玑设备 Wi-Fi / 移动数据路由权重策略

- **来源**: https://www.coolapk.com/feed/73176021
- **适配**: 全系列天玑处理器（含天玑 8400 Ultra、9000、9300），兼容 Magisk / KernelSU。
- **核心策略**: 连接 Wi-Fi 时将 `wlan0` 路由权重设为 `0`（最高优先级）、移动数据 `rmnet` 权重设为 `200`，强制流量走无线以防止偷跑；无 Wi-Fi 时自动将移动数据权重置 `0` 无缝回退 5G，并支持开机自动重建路由。

---

## 2. AxManager 免 Root 模块、Droidspaces 旧机利用与密钥避坑

- **卫星地球 Pro v6.1.0（AxManager 免 Root）**（https://www.coolapk.com/feed/72432897）：基于 AxManager 框架开发的免 Root 网络优化模块。
- **旧手机 Droidspaces 利用**（https://www.coolapk.com/feed/71862286）：针对性能不足以流畅运行现代社交应用、残值极低的旧手机，通过 Droidspaces 容器化改造成轻量本地服务节点。
- **一加 Ace 5 Pro 环境隐藏与 TrickyStore 密钥避坑**（https://www.coolapk.com/feed/71855009）：白名单模式可排除系统应用仅对微信/QQ 放行；**不要使用公开分享的 TrickyStore 密钥**（如已被大量封禁设备共用的密钥会被拉黑），应使用未公开污染的独立/早期密钥。
