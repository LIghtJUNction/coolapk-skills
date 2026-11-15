# Android-x86 适配、Play 商店 Systemize 与临时 Root 断网修复

整理自酷安社区开发者实践记录（未读图片与评论，未做实机验证）。

## 1. Android-x86 1105（Android 16）ARM 兼容与桌面/网络修复

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74957485)（`imgs=0`）
- **主要改动**：作者称 1105 版本将 NDK 替换为 `digitalis`、切换为 `userdebug` 并预装 KernelSU 与 Termux，支持 ARM 应用运行。
- **关键配置命令**：
  - 启用 ARM ABI 支持（同时修改 `build.prop` 中对应项或用 `resetprop`）：
    - `resetprop ro.product.cpu.abilist x86_64,x86,arm64-v8a`
    - `resetprop ro.product.cpu.abilist64 x86_64,arm64-v8a`
  - 修复平板屏幕键盘无法弹出：`settings put secure show_ime_with_hard_keyboard 1`
  - 修复桌面模式设备限制：`setprop persist.wm.debug.desktop_mode_enforce_device_restrictions false`
  - 修复网络连接受限探测：`settings put global captive_portal_http_url http://connect.rom.miui.com/generate_204` 与 `settings put global captive_portal_https_url https://connect.rom.miui.com/generate_204`

## 2. microG 自检全过但 Google Play 商店闪退：Systemize 转为系统特权应用

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74919460)（`imgs=1`）
- **处理步骤**：
  1. 刷入 Magisk 模块 `terminal_systemizer` 并重启。
  2. 在终端（aShell / Termux）执行 `su` 后运行 `systemize`。
  3. 选 `1`（Systemize Installed Apps），找到 Google Play Store（`com.android.vending`）对应编号。
  4. 安装位置选择 `2`（`/system/priv-app`），等待输出 `Done!` 或 `Success!` 后重启。
  5. 验证方式：应用信息中不再显示“卸载”而是“卸载更新”即生效。

## 3. 临时 Root 后“已连接但无网络”的 SELinux 策略恢复

- **来源**：[酷安原帖](https://www.coolapk.com/feed/74867203)（`imgs=1`）
- **现象**：iQOO Neo8 Pro 临时 Root 后 Wi-Fi/网络显示已连接但无法联网。
- **排查与命令**：
  - 若手头有 Termux，直接强制重载 SELinux 策略：`su -c 'load_policy /sys/fs/selinux/policy'`
  - 若尚未安装 Termux，可先在 MT 管理器终端模拟器临时切为宽容模式联网：`su -c "setenforce 0"`，下载 Termux 后再执行上述 `load_policy` 恢复状态。
