# 小米与一加系统隐藏 Activity 与 `am start` 快捷入口记录

本记录整理自社区分享的系统隐藏界面类名与 `am start` 快捷调用参数，仅供参考，实际可用性取决于系统与目标应用版本，未在本库实机验证。

## 1. 小米隐藏界面（免 Root）[72721623](https://www.coolapk.com/feed/72721623)

- 高级色彩管理：`com.xiaomi.misettings.display.ScreenExpertActivity`
- 锁定频段：`com.android.settings.MiuiBandMode`

## 2. 一加 / ColorOS 快捷键 Activity 与 `am start` 示例 [68545296](https://www.coolapk.com/feed/68545296)

原帖注明面向一加快捷键拓展（标注 `#一加15#`、`#一加Ace5至尊版#`、`#一加Ace6#`）：

- 全局搜索：`com.heytap.quicksearchbox/com.heytap.quicksearchbox.ui.activity.SearchHomeActivity`
- 全局搜索-搜应用：`com.heytap.quicksearchbox/com.heytap.quicksearchbox.ui.activity.AppCategoryActivity`
- 游戏中心：
  ```sh
  am start -n com.oplus.games/business.module.desktop.JumpSpaceActivity
  ```
- 调用识屏：
  ```sh
  am start-foreground-service -a oplus.intent.action.DIRECT_SIDEBAR_SERVICE --es extra_entrance_function full_screen_ocr --es triggered_app com.coloros.smartsidebar
  ```
- 相机：
  ```sh
  am start -n com.oplus.camera/com.oplus.camera.Camera
  ```
- 微信支付 / 扫一扫：
  ```sh
  am start -n com.tencent.mm/com.tencent.mm.ui.LauncherUI -a com.tencent.mm.ui.ShortCutDispatchAction --es LauncherUI.Shortcut.LaunchType launch_type_offline_wallet
  am start -n com.tencent.mm/com.tencent.mm.ui.LauncherUI -a com.tencent.mm.ui.ShortCutDispatchAction --es LauncherUI.Shortcut.LaunchType launch_type_scan_qrcode
  ```
- 支付宝付款 / 扫码：
  ```sh
  am start -n com.eg.android.AlipayGphone/com.alipay.android.phone.wallet.shortcuts.bridge.ShortcutsLauncherActivity -a android.intent.action.VIEW --es KEY_APP_ID 20000056 --es KEY_SCHEME "alipays://platformapi/startapp?appId=20000056"
  am start -n com.eg.android.AlipayGphone/com.alipay.android.phone.wallet.shortcuts.bridge.ShortcutsLauncherActivity -a android.intent.action.VIEW --es KEY_APP_ID 10000007 --es KEY_SCHEME "alipays://platformapi/startapp?appId=10000007&sourceId=scan3dtouch"
  ```
