# 5G CPE（通讯壳改造 / F50）、OpenWrt 编译/TTL 刷机与 PPPoE 断流排查

本参考整理酷安社区中关于 5G 随身 Wi-Fi/CPE 改造、高通 410 云编译 ImmortalWrt、小米 4A 千兆版 / Spectrum SBE1V1K 刷机、`SimAdmin` 管理后台、二手定制路由锁网风险与家宽 PPPoE LCP 保活重拨排查的记录。

---

## 1. 华为 5G 通讯壳 + Cudy TR3000 / RAX3000M 低成本 5G CPE 改造

- **来源**: https://www.coolapk.com/feed/72630740
- **硬件组合**: 华为 5G 通讯壳（改插卡，使用 U 型 C-to-C 母转接头直连路由器 USB 3.0 内口以保证速率与供电稳定性）+ `Cudy TR3000 128M` 大分区版或 `CMCC RAX3000M NAND` 大分区/64G eMMC 版（刷支持拨号的 ImmortalWrt）。
- **刷写提示**: 通讯壳刷机包中的 `install_RM5x500U.bat` 若出现乱码，可按原批处理逻辑改写为 PowerShell 执行，刷完等待设备管理器出现 4 个串口后断电。

---

## 2. 5G 随身 Wi-Fi 实测、高通 410 云编译与 `SimAdmin`

- **中兴 F50 / 老飞猫 U20（展锐 T760）**（https://www.coolapk.com/feed/73410196）：支持 USB 3.0 直连路由器与 TF 卡扩展（可作简易 NAS），但高负载下发热明显，高温地区插本地卡易严重断流甚至热关机，更适合固定直连路由器使用。
- **高通 410 随身 Wi-Fi 云编译 ImmortalWrt**（https://www.coolapk.com/feed/71420536）：Fork 仓库后通过 GitHub Actions `Build_imm_高通410系列` 选择机型与插件包（约 `1.5–2` 小时）即可生成刷机包，支持 `ufi003`、`ufi001c`/`b`、`ufi103s`、`qrzl903`、`w001`、`uz801`、`mf32`、`mf601`、`wf2`、`jz02v10`、`sp970v10`/`v11`。
- **`SimAdmin` Debian CPE/软路由管理系统**（https://www.coolapk.com/feed/71677513）：开源地址 `https://github.com/3899/SimAdmin`，Rust（Axum + zbus / ModemManager / mmcli / qmicli / AT）后端 + React 前端，默认安装至 `/opt/simadmin` 并通过 `systemd` 托管。

---

## 3. 路由器刷机、硬路由脚本、二手锁网与断流排查

- **硬路由免刷机网速显示脚本**（https://www.coolapk.com/feed/73398177）：不刷机、不解锁 SSH，针对家用硬路由（中兴、华为、小米、H3C、TP-Link、华硕，中兴/小米基本覆盖全系 Wi-Fi 7 与 Wi-Fi 6）改进网速显示。
- **小米路由器 4A 千兆版（`2.30.x`）刷 Breed + OpenWrt**（https://www.coolapk.com/feed/73331748）：LAN 口连电脑并断开外网，使用新版 Telnet 破解程序后刷入 `breed.bin` 与 `openwrt.bin`（实测 2025 年产设备亦适用）。
- **Spectrum `SBE1V1K`（`IPQ9574`）TTL 刷机准备**（https://www.coolapk.com/feed/72940241）：需 T10 + 十字 2.0 螺丝刀拆机、金属镊子短接、`CH340G` 转 TTL 与 `2.00mm-4p` 单排烧录夹配合 TFTP（`192.168.255.1`）传输固件。
- **二手定制路由锁网与断流对比**（https://www.coolapk.com/feed/73173972、https://www.coolapk.com/feed/70357938）：移动定制路由存在锁网风险，同价位无线体验联发科方案普遍优于高通外置 FEM 旧款；电信定制版中兴巡天 AX3000 在 Steam 高并发下载下出现闪红灯断流死机（仅限速 100M 缓解），更换 `TP-Link 7DR6430`（BE6400 + 2.5G）后恢复稳定。

---

## 4. 家宽 PPPoE 频繁重拨排查（LCP Echo 保活机制）

- **来源**: https://www.coolapk.com/feed/70277236
- **根因区分**: 家宽除运营商定时强制下线外，若每隔几小时频繁重拨，多与 **LCP Echo 保活失败**相关——运营商向路由器发送 `LCP Echo Request`，若未及时收到 `LCP Echo Reply`，服务端会主动下发 `LCP terminated by peer` 断开连接并触发重拨。
