# APatch 安卓 14 报告：不要混用两个问题的环境

[酷安 59302933](https://www.coolapk.com/feed/59302933)提到 Pixel 安卓 14 的 XDA 移植系统，内核为 `4.14.348-openela-perf+`，构建串为 `#1 Mon Aug 19 07:03:36 CST 2024`，并链接 [APatch #297](https://github.com/bmax121/APatch/issues/297)。正文没有描述图片中的具体错误，配图尚未读到；“已有同类 issue”不能自动等同根因相同。

本轮另行读取 #297 正文，里面记录的是不同环境：

| 项目 | Issue #297 的环境 |
| --- | --- |
| 设备 | OnePlus 6T |
| 系统 | Android 14 / crDroid |
| 内核 | 4.9.337 |
| APatch | 10568，PR #289 |
| KernelPatch | 0.10.3 |

该 issue 描述模块安装空白、su 权限异常以及导出日志失败，安装日志只有 `Error: No such file or directory (os error 2)`。它还链接 [libsu #179](https://github.com/topjohnwu/libsu/issues/179)、[log.txt](https://github.com/bmax121/APatch/files/14623376/log.txt) 和 [config.txt](https://github.com/bmax121/APatch/files/14623383/config.txt)；这些关联页面和附件本轮未读取。

读取时 #297 状态为 closed，但本轮没有读取关闭原因及评论。因此不能把其设备、版本、错误或关闭状态移植到酷安那条报告，也不能由此断言某个修复已适用于该 Pixel 移植系统。
