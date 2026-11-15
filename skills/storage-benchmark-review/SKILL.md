---
name: storage-benchmark-review
description: 整理和评估 SSD、移动硬盘的社区测试，保留接口、负载、缓存和型号批次条件，避免把单次成绩当通用规格。
---

收集测试平台、实际协商接口、线材、盘容量、剩余空间、温度、固件、测试工具及负载；缺项明确写未知。

将顺序与随机读写、短时峰值与长时间持续写入分开。记录测试数据量和缓存耗尽后的表现，不用短时峰值推断大文件全程速度。对写入测试先确认目标和可用空间，避免覆盖业务文件。

同一商品名称不能充分说明内部主控或颗粒。把卖家规格、可核验硬件信息、作者推测与网络转载分开；单个样本的颗粒结论不推广到全部批次。不同接口或占用条件下的数据不能直接排名。

## 依据与验收

- [移动 SSD 测试](https://www.coolapk.com/feed/63365899)：正文区分峰值和持续表现，并给出连接环境；主控颗粒信息注明来自网络，随机成绩依赖图片。具体数值以社区报告保存在下方参考，不作为独立复测规格。
- [同名产品颗粒差异报告](https://www.coolapk.com/feed/62530402)：作者报告不同批次颗粒变化，尚无独立验证。提炼为检查批次与证据的必要性，不采纳价格推荐和绝对性能判断。

输出可复查的条件表与结果差异，清楚区分社区报告、自己的测量及未核实项。2026-11-13 已读文字；7 张图片未读，未进行硬件测试。


## 具体案例与公开资源

[SSD 颗粒与移动盘测试案例参考](references/storage-ssd-cases.md)保留原帖已提供的数值、版本、操作与资源入口；均按社区报告和具体条件理解。


## 补充的来源技术记录

- [消费级 SSD 选购基础：闪存、接口与性能取舍](references/source-74351396.md)

## 补充案例

- [2024年 PCIe 4.0 NVMe SSD 横评记录](references/pcie4-nvme-ssd-comparison-2024.md)
- [希捷酷玩 520 与酷鱼 530 SSD 测试笔记](references/seagate-firecuda520-firecuda530.md)
- [Steam Deck OLED：2230 SSD 扩容与 SteamOS 重装](references/steam-deck-oled-storage-upgrade.md)
- [PS5 Slim 购买与 M.2 SSD 扩容记录（2024）](references/ps5-storage-expansion-guide-2024.md)
- [2024年 PCIe 4.0 SSD 选购指标与型号对比](references/nvme-ssd-buying-guide-2024.md)
- [WD Red Pro NAS 硬盘规格与单机阵列体验记录](references/wd-red-pro-nas.md)
- [六款移动固态硬盘的功能与测试记录](references/portable-ssd-comparison.md)
- [PS5、Xbox 与 Switch 主机扩容及平台差异记录](references/console-platform-comparison.md)
- [致态 SSD、PSSD 与存储卡选购和使用记录](references/zhitai-storage-product-guide.md)
- [铠侠 VD10 1TB PCIe 5.0 SSD：规格与原帖测试记录](references/kioxia-vd10-gen5-ssd.md)
- [DDR5、DDR4 与 SSD 选购：原帖的市场判断与配置建议](references/memory-ssd-buying-2026.md)
- [2024 年 PS5、Xbox Series 与 Switch 选购及型号差异](references/console-buying-guide-2024.md)
- [致态 TiPlus7100 黑神话联名版实测记录与 PS5 M.2 扩容步骤](references/tiplus7100-ps5-expansion.md)
- [金百达 KP270 2TB SSD实测记录与装机配置](references/kingbank-kp270-2tb-ssd-review.md)
- [六款128GB高速 microSD 卡横评记录](references/microsd-128gb-six-card-comparison.md)
- [WD Blue 4TB台式机扩容与性能记录](references/wd-blue-4tb-desktop-expansion.md)
- [移动端混用闪存同设备复测掉速个案](references/mobile-ufs-speed-drop-case.md)
