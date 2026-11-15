# 系统伪装（CocoMask）、ColorOS 调校（横幅/字体/刷新率）与防盗取证案例

本参考整理酷安社区中关于 LSPosed 伪装模块 CocoMask（v1.7–v1.15）、隐身防盗取证模块（v1.30）、ColorOS 16/17 横幅通知修复、ColorOS 17 主题商店字体替换与免 Root 刷新率强制工具（akihz）的实测与边界说明。图片与外部链接均未读取或验证。

---

## 1. CocoMask（v1.7–v1.15）：身份伪装、系统微调与 Shizuku 边界

- **来源**:
  - https://www.coolapk.com/feed/75029602（v1.15）
  - https://www.coolapk.com/feed/75015728（v1.8 / v1.7）
- **核心能力与权限边界**:
  - **Shizuku vs Root 边界**: Shizuku 仅提供 `shell (uid 2000)` 权限，无法修改开机只读的 `ro.product.*` 属性（`setprop` 会被 SELinux 拒绝，`resetprop` 需 Root 打开 `/dev/properties`）。因此：
    - **免 Root + Shizuku**: 可同步 `settings_global_device_name`（影响设置、蓝牙、热点及未 Hook 应用读取的设备名），并在 LSPosed 作用域内做按 App Hook 伪装。
    - **全局伪装（需 Root）**: 通过 `resetprop` 修改 `ro.product.*`（型号、市场名、品牌、认证型号），仅本次开机有效，重启自动恢复；其中 `ro.product.cert` 认证型号修改需完整重启手机才更新，其余多数改动支持软重启（优先 `ksud soft-reboot`，失败回退 `setprop ctl.restart zygote`）。
  - **v1.15 新增特性**:
    - 身份页：运营商伪装（`gsm.sim.operator`、`ro.carrier`、`TelephonyManager`）、地区语言伪装（中/美/日/德预设）、设备标识替换（`android_id`、`Build.getSerial()`、`Build.SERIAL`，强停目标 App 即生效）、敏感路径 `File.exists()` 反检测（`su`/Magisk/KernelSU/LSPosed/Zygisk，仅作用域内生效）。
    - 微调页：动画速度（`1×` / `0.5×` / 关闭）、屏幕 DPI（`240–720`）、刷新率（`60/90/120/144`/自动）。
  - **v1.8 交互改进**: 移除 WebUI 与 Java 间基于 `prompt()` 的原生弹窗，改为静默通道同步返回。

---

## 2. ColorOS 16/17 新消息不弹横幅通知修复

- **来源**: https://www.coolapk.com/feed/75045243
- **适用范围**: 一加 / OPPO / realme（ColorOS 16、ColorOS 17、OxygenOS、realme UI）
- **根因**: 系统将横幅开关拆为两层——设置界面由 `com.oplus.notificationmanager` 控制，但该界面开关不会同步写入 AOSP 的 `heads_up_notifications_enabled`；而 SystemUI 实际判定是否弹出横幅时会读取 `heads_up_notifications_enabled`。两处不同步会导致通知栏有消息但不弹横幅。
- **三种修复入口**:
  - **ADB**: `adb shell settings put global heads_up_notifications_enabled 1`
  - **Root（MT 管理器终端）**: `su -c 'settings put global heads_up_notifications_enabled 1'`
  - **Scene**: `附加功能 -> AOSP 专属 -> 打开悬浮通知`

---

## 3. ColorOS 17 降级主题商店替换字体流程与已知 Bug

- **来源**: https://www.coolapk.com/feed/75028038
- **适用背景**: 升级 ColorOS 17 全量包后出现第三方字体大小不一；新版主题商店（v17）无法直接按旧法替换，需卸载回退至**主题商店 v13.0**。
- **关键前置与注意点**:
  - 卸载主题商店会使当前字体、壁纸、主题恢复默认，操作前需备份；ColorOS 17 测试版在**手机重启后字体会再次错乱**，需重新应用一次。
  - 授权 MT 管理器访问受限目录时，需在开发者选项中开启无线调试与“关闭权限监控”（部分中文界面未显示 `Disabled system optimization`，原帖通过临时将系统语言切换为英语开启该项，配对启动 Shizuku 后再切回中文）。
  - 在主题商店 v13.0 中通过 DIY 字体生成目标字体资源文件夹并切回默认，再由已获 Shizuku shell 权限的 MT 管理器完成字体文件替换（具体目录与替换步骤截图未读取）。

---

## 4. akihz：免 Root 搭配 Shizuku 强制调节刷新率

- **来源**: https://www.coolapk.com/feed/75020073
- **项目入口**: `https://github.com/anlaki-py/akihz/releases/latest`、`https://akihz.anlaki.dev/`（现代设备选 `arm64-v8a`，不确定选 `universal`）
- **使用路径**: 需搭配 Shizuku，无需 Root；界面为英文。依次进入 `Settings -> Refresh rate -> Custom refresh-rate keys` 开启强制刷新率，在 `Profile rates` 选定档位后于下方 `Scan` 搜索可用配置文件，逐个测试生效项（原帖作者测试中失败配置会弹窗提示，成功配置生效后设备直接重启）。

---

## 5. 隐身防盗取证模块（v1.30）功能与限制

- **来源**: https://www.coolapk.com/feed/75043605
- **前置环境**: Android 14+ / 小米澎湃 OS 2/3/4；已 Root（Magisk/KernelSU）+ LSPosed（需勾选模块自身、`com.android.systemui`、`android` 三个作用域，更新后必须重启生效）。无桌面图标，唯一入口在 LSPosed 模块列表，支持最近任务隐藏。
- **主要机制**:
  - **锁屏失败/换卡/拔卡触发**: 连续输错锁屏密码达阈值（1–4 次可设）或检测到陌生 SIM 卡/拔卡时，静默调用前置拍照（最多 6 张）、录像（最长 20 秒）、定位与双卡号码采集（刚开机未首次解锁时亦尝试触发）。
  - **短信远程指令**: 任意号码发送 `<指令> <密码>`（`拍照`/`录像`/`邮箱`/`云盘`/`定位`/`状态`），模块自动拦截并删除指令短信以防在收件箱留痕。
  - **多渠道上报与存储**: 支持短信、SMTP 邮件（465 SSL / 587 STARTTLS，录像超 25MB 改发首帧图）及坚果云（根目录「盗用取证」）；本地默认存私密目录 `files/evidence`，开启公共目录 `/storage/emulated/0/盗用取证` 需授予所有文件访问权限。
