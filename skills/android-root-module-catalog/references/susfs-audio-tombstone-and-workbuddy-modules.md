# SUSFS UI、高保真音频 XML、SIGSTOP 墓碑与 WorkBuddy Manager 模块

整理自酷安社区开发者发布记录（未读配图与评论，未做实机刷入验证）。

## 1. `susfs_ui`：KernelSU-Next SUSFS 内核模块前端控制台

- **来源**：[酷安原帖](https://www.coolapk.com/feed/75026368)（`imgs=3`，包名 `susfs_ui`，体积 17.9KB 纯界面模块）
- **背景**：KernelSU-Next 支持 `ksud susfs load` 以 LKM 方式加载 SUSFS，但无模块列表入口、无 WebUI、无配置文档，且 `/proc/susfs_*` 写入错误不报错或返回值截断。
- **核心功能边界**：
  - 仅作遥控器/可视化前端（不自带隐藏能力，不修改系统文件），覆盖状态总览（LKM 状态、隐藏路径数、挂载前缀占用、隐藏模块数）、普通 App UID 隐身实测（返回 `ENOENT` 视为真隐藏）、路径管理与内置 7 条 `/data/adb` 常用规则一键写入、10 项诊断检查及自定义路径增删。

## 2. 红魔 11S Pro+：KernelSU Systemless 高保真音频策略模块

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74995177)（`imgs=3`）
- **问题与方案**：原机 `/system/vendor/etc/audio_policy_configuration.xml` 最高仅配置 16bit 48kHz；作者通过 KernelSU + 元模块（`meta-overlayfs` 或 magic mount 类）systemless 替换该文件（依赖参考贴 `https://zhuanlan.zhihu.com/p/511094531`），支持高达 32bit 192kHz PCM（USB 连接下 `deep_buffer`/`primary`/`direct_pcm` 支持至 384kHz）、扩展压缩格式（FLAC/ALAC/APE/Vorbis/WMA）及扩展 A2DP 编解码，实测 Poweramp 可直连绕过系统重采样。
- **故障排查**：
  - 开机无声或音频服务崩溃：在 KernelSU Manager 禁用模块并重启。
  - 挂载与日志检查：`adb shell "grep direct_pcm /system/vendor/etc/audio_policy_configuration.xml"`、`adb shell "logcat -d | grep AudioPolicyEnh"`、`adb shell "logcat -d | grep -i audiopolicymanager"`。

## 3. `SIGSTOP`/`SIGCONT` 墓碑冻结模块

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74990611)（`imgs=0`）
- **机制**：每 2 秒轮询前台应用；应用退至后台超过 `FREEZE_DELAY`（15/30/60/120 秒四档，默认 30 秒）后发送 `SIGSTOP` 挂起全部进程，切回前台时发送 `SIGCONT` 恢复；支持总开关、熄屏连带冻结前台、冻结系统应用开关（默认关）。
- **防误杀双层保护**：包名白名单（支持前缀通配，默认包含微信、QQ、支付宝、网易云/QQ 音乐、高德/百度地图、讯飞/搜狗输入法、下载器）+ 挂载前台服务的进程自动豁免。

## 4. `WorkBuddy Manager` Magisk/KernelSU 模块

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74811564)（`imgs=5`）
- **用途**：将开源项目 `ithtelab/WorkBuddy Manager`（腾讯 CodeBuddy 账号池管理控制台 + OpenAI 兼容反向代理网关，MIT 许可）做 Android 运行时适配打包为 Magisk/KernelSU 模块，使旧手机替代常开 Docker/VPS。
- **使用方式**：安装重启后访问 `127.0.0.1:7864`，用户名 `admin`，随机初始密码位于 `/data/adb/workbuddy-manager/webui.txt`；支持在面板内直接更新上游。
