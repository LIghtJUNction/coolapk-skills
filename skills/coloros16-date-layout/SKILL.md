---
name: coloros16-date-layout
description: "记录针对 ColorOS 16 经典主题资源文件的布局调整方法及前置条件。"
---

# ColorOS 16 主题日期与星期布局修改

此方法针对 ColorOS 16 的经典主题 APK 资源布局。原帖说明的前置条件是设备已 Root 并启用 Corepatch；使用 MT 管理器提取主题 APK 后修改 XML。资料没有说明适用于其他系统版本、主题或机型。

## 居中样式

编辑 `res/Cf.xml`：将 `clock_time` 的 `app:row` 从 `1` 改为 `0`；将 `date_text`、`week_text` 与 `tips_text` 的 `app:row` 从 `0` 改为 `1`。若日期区域与时钟间距过小，可给日期、星期和提示元素增加 `android:layout_marginTop="8dp"`。

## 左上角样式

编辑 `res/PD.xml`（文件名中的 `PD` 为大写），在根布局的第三行加入 `week_text`，放在 `date_text` 与 `tips_text` 之间，并补齐对应行列属性。原帖图片未读取，行列属性的具体值无法从现有文字资料确认，不补写数值。

原帖要求保存时不签名，安装修改后的主题后无需重启。没有说明安装失败时的恢复方式；操作前应自行保留原主题文件。该方法未经实机验证。原帖没有明确说这是锁屏页面布局，不应据此标注为锁屏教程。

来源：[原帖](https://www.coolapk.com/feed/72914953)。

整理日期：2026-11-13。
