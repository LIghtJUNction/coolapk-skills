---
name: thanox-google-framework-rule
description: 理解和核对 Thanox 按应用事件启停 Google 框架的历史情景规则，保留原始功能性 JSON，并明确事件、短路和多应用并发边界。
---

# 自动开关 Google 框架 V1.0

本文介绍一个 Thanox 情景模式：当切换到全局变量 `Google` 中列出的应用时，启动 Google Play 服务和 Google 服务框架；当其中一个应用被杀时，结束这两个进程。

## 配置情景模式

1. 搜索包名关键词 `d.g`，找到以下应用：
   - `com.google.android.gms`
   - `com.google.android.gsf`
2. 原文明确提到关闭后台自启，但后半句“运行后台运行”有语病；不能据此补成已确认的后台运行开关或具体系统菜单。
3. 在 Thanox 情景模式页面右上角打开“全局变量”，将变量标题设为 `Google`，并在其中添加需要 Google 框架才能运行的应用。
4. 如果不需要提示弹幕，可删除配置中的 `ui.showDanmu(...)` 调用。

## 情景模式配置

```json
[
  {
    "name": "自动开关谷歌框架",
    "description": "打开需要软件时开启框架，杀软件后关闭，全局变量Google",
    "priority": 2,
    "condition": "if (frontPkgChanged == true && globalVarOf$Google.contains(to)) {ui.showDanmu(\"框架开\");activity.launchProcessForPackage(\"com.google.android.gms\")&&activity.launchProcessForPackage(\"com.google.android.gsf\");} else if (pkgKilled == true && globalVarOf$Google.contains(pkgName)) { ui.showDanmu(\"框架关\");killer.killPackage(\"com.google.android.gms\")&&killer.killPackage(\"com.google.android.gsf\")}",
    "actions": [
      ""
    ]
  }
]
```

配置的条件表达式使用 Thanox 提供的事件变量和动作：`frontPkgChanged` 与 `to` 用于判断前台应用切换；`pkgKilled` 与 `pkgName` 用于判断应用被杀；`globalVarOf$Google` 是应用包名列表。启动和结束操作分别针对 `com.google.android.gms` 与 `com.google.android.gsf`。匹配时会显示“框架开”或“框架关”弹幕。

原帖给出的配置片段存在排版/格式疑点：`actions` 数组中只有空字符串，实际动作写在 `condition` 字符串里；该字符串中的 `&&` 连接动作调用，且末尾结束分支后没有分号。这里保留原配置，不推断或静默修复其语法。使用前应按所用 Thanox 版本确认情景模式表达式格式。

## 适用范围与资料缺口

此配置仅针对全局变量 `Google` 中列出的应用及上述两个 Google 框架包名。原帖没有提供 Thanox 下载链接、项目链接、版本兼容信息或公开分享码。
## 验证范围与规则边界

本库已解析 JSON，并确认与原帖配置的字段及字符串内容一致；没有在 Thanox 或手机上执行，不能把 JSON 解析通过当作情景表达式验证。

原规则没有统计其他 Google 应用是否仍在运行：列表中任意一个应用被杀就会尝试结束 GMS/GSF。`&&` 还存在短路行为，第二项是否执行取决于第一项返回值与实际表达式引擎。副作用位于 `condition`、`actions` 只有空字符串的结构按原文保留，不能默默改写成已验证的新版本规则。结束框架可能影响仍依赖它的功能，本文不将其当作通用省电保证。

来源：[酷安原帖](https://www.coolapk.com/feed/56714973)。
