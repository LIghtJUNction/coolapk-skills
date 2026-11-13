# ASL：基于 Rurima 的安卓子系统

## 项目与资源

- 项目仓库：https://github.com/RuriOSS/asl
- 下载分享：https://www.123865.com/s/FWYIjv-j3WJ3（原帖标注无提取码）
- 原帖所述压缩包大小：8 MB
- 原帖所述 SHA256：`e46e402b56e24528a14a479d0d12e874b3e8e1d7b9c2fbcb74fa8e207ae56fa9`
- 这是基于 Rurima 的非官方版本；原帖表示后续可能继续修改更新。

## 功能与环境边界

ASL 为 Rurima 虚拟环境提供终端配置与工具：默认集成 zsh、zim 框架和 p10k 主题，包含补全、高亮和历史记录；集成 nvim，并提供 `action.sh`、`.rurimarc` 等配置入口。原帖还列出 `magiskboot` 等实用工具。

Rurima 环境不会自动安装子系统。原帖明确说明它不是 GNU/Linux：其中运行的软件仍依赖 Android 的非 GNU 库，生态复用 Termux。若要使用 GNU/Linux，需要自行部署相应子系统。Termux 是扩展软件来源的推荐搭配，并非 ASL 自动附带的完整 GNU/Linux 发行版。

## 进入环境

原帖给出的基本步骤：

```sh
su
rurima
```

先通过 `su` 提权，再运行 `rurima` 进入虚拟环境。原帖称虚拟环境位于 `/data/adb` 下，普通应用不可见；并报告其场景中的 `/system/xbin/rurima` 权限为 `700`。这些是该帖描述的具体安装与权限情况，不构成所有 Root 环境的通用保证。

模块不自动下载或安装子系统。进入环境后，用户需自行安装所需内容，安装位置可自行选择。原帖支持 `rurima pull` 拉取镜像，但这不表示镜像已预先安装。

原帖推荐在 Termux 中安装所需软件，并通过 Root 授权后运行 `su`、再执行 `rurima`。其说明称 Termux 下载的软件可在该模块环境中使用；实际可用性取决于目标软件及运行环境。

## 终端配置

原帖列出的别名如下：

```sh
alias cls="clear"
alias pull="rurima pull"  # 等价于 rurima docker pull
alias ota="rurima ota"
alias rdocker="rurima docker"
alias bak="rurima backup"
alias unpack="rurima unpack"
alias dep="rurima dep"
```

可按需自定义别名。`.rurimarc` 可用于自定义启动时显示内容；原帖称默认显示 Termux 已安装时的 fastfetch 信息及 `rurima -v` 版本信息。原帖建议不要修改 `.zshrc`，并称其开头包含提示信息。可运行 `action.sh` 快速禁用或启用提示。

默认 Shell 为 zsh。原帖称也可以自行安装 fish；由于 zim 框架用于 zsh，切换 Shell 时还需替换相应框架。p10k 样式和字体可以更换。

## 原帖报告的问题与修复

测试版曾在未安装 Termux，或 Termux 未安装 zsh 时发生报错。后续更新称将 `libcab` 库内置到模块自己的 `lib` 目录后修复此问题。更新仍建议安装 Termux 以扩展功能，并不表示 Termux 已不再有用途。

测试版还新增了 `magiskboot` 等工具。原帖不建议使用 MT 终端打开该测试版，但没有提供更详细的原因或适用版本范围。

## `su` 文件检查说明

原帖建议通过以下命令查找并查看 `su`：

```sh
which su
cat $(which su)
```

也可用 MT 管理器打开对应文件。原帖提议按需修改 `su` 文件，但没有给出具体修改内容或安全、兼容性说明。`cat $(which su)` 只是原帖提供的查看方式，不代表所有环境里的 `su` 都是文本脚本。

## 使用场景与资料边界

原帖举例：在 MT 管理器所处目录运行 `rurima`，进入环境时默认不切换目录，以访问 Termux 下载的软件；也举例通过 ADB 连接手机后进入环境执行命令。原帖提及 npm、uv、cargo、git 及 Termux `pkg` 可获取的软件。这些是作者描述的使用场景，不是对所有软件兼容性的保证。

原帖将“Rurima 安装子系统教程”列为后续计划；提供的资料没有该教程内容。原帖中的图片未提供可读内容，不能据此补充功能或操作步骤。
来源：[68855900](https://www.coolapk.com/feed/68855900)、[68913646](https://www.coolapk.com/feed/68913646)。资源、哈希和修复为原帖所述，本次没有下载 ZIP、校验文件或在手机运行。它们与主技能中较晚的 1.2.12 资源分开记录，不借用版本/哈希。
