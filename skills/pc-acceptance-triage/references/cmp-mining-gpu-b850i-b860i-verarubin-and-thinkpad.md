# CMP 矿卡显存解锁、B860I/B850I 装机避坑、Vera Rubin NVL72 与 ThinkPad 经典机型

> 整理自酷安公开动态正文；图片、评论及外链未逐一核验，实际操作请以对应硬件版本与上游仓库为准。

- #### [酷安原帖](https://www.coolapk.com/feed/74109676)（imgs=0）
  - **DRAM 垄断与反垄断诉讼背景**：2026 年 6 月三大内存原厂（三星、海力士、美光）在美遭遇反垄断集体诉讼，被指控借 HBM 产能转型为由协同压减标准 DRAM 产能，导致传统内存价格 4 年涨幅约 700%。
  - **魔改矿卡固件解锁真实稳定性与避坑**：
    - **NVIDIA CMP 170HX**：GitHub 出现 `CMPUnlocker` 固件破解工具，将锁死的 8GB/10GB HBM2e 显存扩充至最高 80GB。实测验收陷阱：10GB 三星显存颗粒版本在 80GB 档位极其不稳定，实际仅 40GB 档位能长期稳定运行；8GB 海力士版本解锁 64GB 严重依赖核心与晶圆体质，发热剧增故障率高。
    - **NVIDIA CMP 90HX**：解锁单精度 FP32 后 TimeSpy 跑分突破 10000 分，理论游戏与计算性能接近 RTX 3070。
    - **NVIDIA CMP 40HX**：可通过工具破解解锁 PCIe 2.0 带宽限制。

- #### [酷安原帖](https://www.coolapk.com/feed/74105180)（imgs=9 未读）
  - **测试硬件平台**：Intel Ultra 5 230F (10C/10T) + 技嘉 B860I AORUS PRO ICE + 影驰 GTX 1060 HOF + 建兴 CA1 HP OEM 固态。
  - **颗粒属性与超频参数**：金龙惠宇 DDR5 16G 4800MHz（双条 1787.6 元），采用美光早期 1-α 制程 A-Die（D8BNK，Thaiphoon Burner 台风软件误识别为 B-Die）。
  - **稳定性与加压致命踩坑**：
    - 默认状态仅遵循 JEDEC 4800 C40，无 XMP，AIDA64 读写延迟超过 100ns。
    - 加载技嘉主板预设 5600 C38（开启高位宽与低延迟选项），顺利开机且 TM5 压力测试无报错通过。
    - 尝试 6000 C40 时开机即报错；**加压避坑**：手动加压至 DRAM Voltage 1.5V 尝试冲击 6000MHz，导致电脑反复开机崩溃重启，最终直接造成 Windows 系统底层损坏瘫痪。

- #### [酷安原帖](https://www.coolapk.com/feed/73504458)（imgs=9 未读）
  - **机型与硬件结构（ThinkPad X1 Carbon 1st Gen，2012/2013）**：
    - 美版顶配与国行 20 周年纪念版对比：C 面保留独立音量键与实体快捷键；20 周年版保留经典蓝回车键与含闪粉的蓝色 ThinkVantage 键，配备带有“20”标识的专属指点杆帽。
    - 内部专有接口：全板载焊死内存；硬盘与 WLAN 网卡采用联想私有非标 NGFF 接口；WWAN 为半高 mini PCIe，仅出厂适配 3G Gobi 3000 网卡，无原生 LTE 升级路径。
  - **屏幕与外设材料**：
    - 屏幕为定制 14 英寸 1600x900 HD+ TN 屏（72% NTSC，300nit），上下视角衰减明显；触控屏版本采用非全贴合工艺以避免类似 Helix / Tablet 2 的长期漏胶通病。
    - 搭载 ThinkPad 家族史上首款大面积磨砂玻璃触控板。
  - **性能实测基准（i7-3667U Ivy Bridge 17W，基准 2.0GHz / 睿频 3.2GHz）**：CPU-Z 单核 230 分、多核 702 分；7-Zip 压缩解压成绩 11654 分；3DMark06 跑分 5643 分；标配 120GB/256GB 闪迪或 Intel 定制 SSD。

- #### [酷安原帖](https://www.coolapk.com/feed/73206164)（imgs=9 未读）
  - **主板与供电方案**：精粤 B850I SNOW DREAM WIFI（雪梦），Mini-ITX (17x17cm) 8 层 PCB，8+2+1 相 55A DrMOS（主控 RT3678BE）。PCIe 5.0 x16 显卡插槽，3 个 M.2 盘位（正面 1x PCIe 5.0，背面 2x PCIe 4.0），后置自带 OCuLink 接口，RTL8125BG 2.5G 网卡 + MT7902 Wi-Fi 6。
  - **性能与装机避坑关键细节**：
    - 搭配 Ryzen 7 9700X（TDP 65W，短时爆发功耗 160W）；搭配 AMD Wraith Prism 幽灵棱镜散热器（标称 105W TDP），全核高负载渲染温度冲至 85-92℃，噪音极大。
    - **灯效冲突避坑**：散热器提供主板 USB 供电控制（酷冷至尊 App）与 5V 3 针 ARGB（主板 BIOS 控制）两条独立的控制线，**两根线绝不能同时接插**，否则主板信号与 USB 信号冲突导致调色失灵。
    - **双路 12V 电源风险**：大水牛劲强 650 采用双路 +12V（每路 25A，单路限制 250W 左右），9700X (120W) + RTX 2080 FE (225W) 虽总功耗满足，但不可长期高负载烤机，容易单路过载烧毁，建议选用单路 +12V 全模组电源。
    - **主板做工瑕疵**：M.2 金属散热装甲存在金属冲压毛刺黏连无法开启，需人工打磨切除；装甲底部直接贴合硅脂压制南桥芯片。
    - 内存兼容：精粤原厂 DDR5 6000 16G (CL30 1.25V) 插 A2/B2 槽开启 EXPO 一次点亮。

- #### [酷安原帖](https://www.coolapk.com/feed/70858613)（imgs=9 未读）
  - **NVIDIA Vera Rubin NVL72 架构参数**：
    - GPU：TSMC 3nm (N3P) 制程，封装 3360 亿晶体管，搭载 288GB HBM4，显存带宽 22TB/s；配 Vera CPU（88 核 Olympus 架构），通过 NVLink-C2C 实现 1.8TB/s 统一内存寻址。
    - 机架级算力：72 颗 GPU 经 NVLink 6 互联，背板总带宽 260TB/s，提供 3.6 EFLOPS (NVFP4) 推理算力。全机架 45°C 温水全液冷，峰值功耗 230kW，配置前代 6 倍容积的储能电容以平滑峰值功耗。
  - **Kyber NVLink 6.0 铜中板交换刀片**：放弃机架内长铜缆，改用极高密度全被动铜质中板（Passive Copper Midplane）；单 GPU 双向带宽 3.6TB/s；集成两颗 Switch 6 ASIC（单芯片带宽 28.8TB/s）；中板单侧近 10,000 引脚，全机架接触点超 87,000 个。
  - **桌面与边缘 AI 硬件**：
    - NVIDIA GB10 Grace Blackwell Superchip：72 核 Arm Neoverse V3 CPU + Blackwell GPU（NVLink-C2C 900GB/s），128GB HBM4（带宽 1.6TB/s），TDP 控制在 300W-450W，常规风冷即可压制。
    - 富士康 BlueField-4 DPU：双向吞吐 800 Gb/s，板载 128GB LPDDR5 与 64 核 Grace CPU，全液冷接驳设计。
    - 现场发售 Jetson 开发套件：Jetson Thor（$2799，800 TFLOPS FP8，Blackwell 架构人形机器人大脑）；Jetson AGX Orin（$1299，275 TOPS）；Jetson Orin Nano（$249，40 TOPS）。

- #### [酷安原帖](https://www.coolapk.com/feed/70043389)（imgs=9 未读）
  - **设备配置与结构**：ThinkPad X201t（2010 年发布，12.1 英寸 180 度双向旋转翻折触屏 Tablet PC，重量 1.6kg，镁铝合金防滚架）；搭载第一代酷睿低电压版 i7-640LM（双核四线程，2.13-2.93GHz），出厂标配 2GB DDR3 + 500GB 5400 转 HDD。
  - **拆解维修与故障排障流程**：
    - 拆卸 D 面螺丝，依次取下键盘与前掌托，卸下主板螺丝取出背面散热模组；彻底清理 CPU 与铜管上已干结老化的导热硅脂并重新涂抹，清理风扇内灰尘。
    - 故障电池替换：更换主板内部已耗尽的 CR2032 纽扣 BIOS 电池，开机进入 BIOS 重新校准时间；原厂 2011 年笔记本动力电池距今 15 年仍保持可用容量。
    - 存储与内存升级：将原有机械硬盘替换为英睿达 Crucial MX100 256GB SATA SSD，更换滑丝生锈螺丝；拆下独立内存盖板，将原 2G+4G 替换为三星 4G+4G DDR3 组成 8GB 双通道（官方主板上限 8GB，改写 BIOS 可破）。
    - 替换原装拆机键盘并重新装配，安装联想 OEM 专用 Windows 7 Professional 激活。
