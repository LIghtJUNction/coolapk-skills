# Switch 大气层 23.0.1 整合包、M5 Pro Mac mini AI 实测与 Stream 固件更新

本参考整理酷安社区中关于 Switch 大气层整合包 `23.0.1 UltraOC`、新款 M5 Pro Mac mini 与 M4 Pro 本地大模型对比实测，以及 iFi NEO Stream / ZEN Stream MCU 固件更新的官方/社区发布记录。

---

## 1. Switch 大气层整合包 `23.0.1 UltraOC` 稳定版

- **来源**: https://www.coolapk.com/feed/75046062
- **核心组件版本**: `Atmosphère 1.12.2`、`hekate 6.5.5`（支持 `HorizonOS 21.0.0–23.5.0`）、`MissionControl v0.16.1`、`Ultrahand-Overlay 2.5.4`（呼出按键 `ZL+ZR+D-Down`）、`SaltyNX 2.1.4`、`sys-patch 1.6.3.0`、`FPSLocker 3.5.1`、`Horizon-OC 2.6.1`，默认屏蔽虚拟与真实系统序列号和 DNS。
- **注意事项**: 若未升级至 `HOS 21.0.0+` 且无强制需求，不建议升级至 `21.0.0+`；腾讯国行 Switch 用户升级 `19+` 系统前需先切换至全球版本（入口见原帖关联 `Tencent-switcher-GUI_Rebulid`）。

---

## 2. M5 Pro Mac mini vs M4 Pro MacBook Pro 本地 AI 实测

- **来源**: https://www.coolapk.com/feed/75011667
- **测试条件**: LM Studio 加载 `Gemma 4 31B MLX 4-bit` 模型，GPU offload 开到 Max，并发数 `1`，`Temperature=0`，同提示词连续 5 轮（去掉首轮预热）。
- **实测首词元延迟中位数**: M5 Pro Mac mini 为 `0.157 秒`，M4 Pro MacBook Pro 为 `0.384 秒`（首词元快约 `2.45 倍`）。

---

## 3. iFi NEO Stream（MCU `V3.02.1`）与 ZEN Stream（MCU `V3.01.9`）固件更新

- **来源**: https://www.coolapk.com/feed/74868883
- **核心修复**: 适配新版 `Nexis App`（原帖注记写 Neixs/Nexis App），修复 DLNA 下外接 USB DAC 播放 DSD 切换输出偶发杂音与屏显采样错误、AIO 档位经 Jriver 播放时热插拔 USB DAC 导致 DLNA 不可用、U 盘/NAS 曲目收藏识别异常、Qobuz 拖动进度条杂音及 AirPlay 开关异常，并将设备重命名上限优化为 `15` 字符。
