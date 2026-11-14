# 在线安装脚本：浏览、审计与交互安装

来源：<https://www.coolapk.com/feed/74322348>

原帖介绍了一个在线安装脚本目录，并给出交互式菜单脚本在 macOS/Linux 和 Windows 上的运行方式。它建议先检查脚本内容，再决定是否执行。

## 浏览脚本

脚本目录：<https://api.lmm.best/scripts>

## 交互式安装

macOS/Linux：

```sh
curl -fsSL "https://api.lmm.best/scripts/menu.sh" | bash
```

Windows PowerShell：

```powershell
irm "https://api.lmm.best/scripts/menu.ps1" | iex
```

以上命令从远程地址取得脚本并交给 shell 执行。原帖没有说明所需系统版本、依赖、权限要求或菜单中的具体安装项目。

## 执行前审计

原帖建议先查看脚本内容。可将命令管道 `|` 后的执行程序替换为 `less` 等查看程序，再检查内容；也可以把脚本交给 AI 分析。检查后再决定是否执行。

## 源码仓库

脚本同步自 GitHub 仓库：<https://github.com/TokenNotIncluded/lmm-scripts>

原帖未提供具体提交版本、校验和、安装结果或测试记录，因此无法据此确认脚本版本及实际运行效果。

原文自动识别链接时将 shell 闭引号包含在 href 末尾；上面按原始命令的引号边界还原，没有把闭引号当作URL路径。未下载或执行脚本。原帖1张图片未读。
