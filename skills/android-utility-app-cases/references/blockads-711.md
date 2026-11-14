# BlockAds 7.1.1：Android 域名过滤与内容过滤

## 域名过滤机制
帖文称 BlockAds 7.1.1 在设备本地检查域名请求：命中名单时不解析，未命中则照常放行。无 root 模式使用 Android 提供的本地网络接口；有 root 时使用 iptables 重定向。规则在内存中以 Trie 匹配，未命中的流量不经远端服务器。项目标注 GPL-3.0-only。

帖文列出的功能包括内置 StevenBlack、AdGuard DNS、EasyList 等规则源并按语言地区启用，DoH，自定义规则，按应用绕过，开机重连，设置导入导出，实时查询日志，以及每 6、12、24 或 48 小时自动更新规则。

帖文称默认数据保存在设备本地且无需账户，同时存在可选崩溃报告与主动加入的遥测；因此“默认本地”不应被理解为所有情况下绝不发送诊断数据。

## 可选内容过滤的边界
可选内容过滤使用用户态 TCP/IP 栈，帖文描述其基于 gVisor netstack，经 tun2socks 终止 flow，并用 `ConnectivityManager.getConnectionOwnerUid()` 查询 UID 归属。仅对用户选定的浏览器做本地解密；证书锁定应用与 284 个保留域名绕过。该功能仍处于测试阶段，需要安装用户证书。帖文报告 v7.1.1 修复了白屏和连接不稳定问题，但没有提供复现条件或独立测试结果。

域名过滤不能可靠拦截所有应用中的广告；同域名广告和开屏广告不属于域名名单可解决的范围。内容过滤与域名过滤是不同能力，前者的本地解密会扩大证书信任与隐私边界。

## 系统限制与版本要求
- 最低 Android 7.0；帖文所列 target 为 16.0。
- 官方包按架构提供 `arm64-v8a`、`armeabi-v7a`、`x86`、`x86_64` 和 universal。帖文建议旧 32 位设备选 universal。
- Android 同时只能启用一个本地网络接口；启用 BlockAds 时，VPN、公司内网通道或其他防火墙可能无法并行使用。

## 获取
- 光鸭网盘分享：[https://www.guangyapan.com/s/1966846525042024532_auwOgx1YeiVTPWqW](https://www.guangyapan.com/s/1966846525042024532_auwOgx1YeiVTPWqW)
- 夸克网盘分享：[https://pan.quark.cn/s/96c1ead0494e](https://pan.quark.cn/s/96c1ead0494e)

来源未记录分享提取码。文章含两张图片，未读取；评论未提供且历史评论未完成。实现细节、架构包列表及修复状态均为原帖报告，未独立验证。

来源：[74957036](https://www.coolapk.com/feed/74957036)（图片与评论未读，未在本库实机复现）。

