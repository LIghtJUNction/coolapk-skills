---
name: asl-rurima-containers
description: 使用 ASL 和 Rurima 运行 Alpine 或 Arch 根文件系统、排查容器 DNS 并配置编辑器时使用。
---

# ASL 与 Rurima 容器使用参考

以下步骤来自帖子，未实机验证。ASL ZIP 是帖子给出的 1.2.12 版本直链；帖子未提供独立校验和，因此下载内容未在此核验。帖子说明安装前需安装 Termux。

## ASL 模块资源

- 1.2.12 ZIP：<https://github.com/LIghtJUNction/asl/releases/download/1.2.12/asl.zip>
- 原帖附的第三方代理链接：<https://hk.gh-proxy.org/https://github.com/LIghtJUNction/asl/releases/download/1.2.12/asl.zip>
- 相关帖子提供的模块下载页：<https://www.123865.com/s/FWYIjv-koWJ3?pwd=rZW6>（公开分享提取码：`rZW6`；文件版本及页面当前访问状态未知）

HTTP 响应不等于文件内容已校验。

## Termux 与权限

帖子列出的依赖：

```sh
pkg install jq proot
```

Rurima 需要以 root 权限运行的说明在后续更正中确认。旧帖中的内核命名空间检测、`unshare` 检查与重新编译内核推测已被撤回，不作为安装前提。进入容器之后才使用下文的发行版确认命令；它不是宿主机权限检测。

帖子中的管理命令包括：

```sh
su
rurima
setup
dep
```

`setup` 用于将帖子提到的工具部署到 Termux 的 bin 目录；`dep` 用于检查依赖。若需从 Termux 安装依赖，帖子说明应先从 root 会话退出两次再运行 `pkg install`，例如：

```sh
exit && exit
pkg install jq proot
```

`pkg install fastfetch`、`pkg install fakeroot`、`pkg install util-linux` 是原帖列为可选项的命令，不是核心必需依赖。

## Alpine 容器路径

原帖分步骤写法是在普通 Termux 会话拉取：

```sh
rurima pull alpine:edge ./test
```

运行该根文件系统时需 root，命令为 `rurima r ./test`。跨会话要确认相对路径仍指向刚解压的目录。

原帖另外给出一段独立的 root/工具会话示例：

```sh
su
rurima
cd /data
mkdir asl && cd asl
pull alpine:edge ./apline
rurima r ./apline
```

`apline` 是此段原文的目录拼写，镜像标签仍是 `alpine:edge`。两段采用不同调用上下文，不拼接为一条流程；第二段的 `mkdir asl && cd asl` 假设目录尚不存在，已有目录时应进入已有目录。不要把 `./test` 和 `./apline` 交叉使用。

进入容器后查看发行版：

```sh
cat /etc/os-release
```

## Arch Linux 容器路径

Arch 示例来自另一帖，路径与容器名独立：

```sh
su
rurima
dep
cd /data
mkdir -p asl/archlinux && cd asl
pull archlinux:current ./archlinux
run ./archlinux
```

`dep` 查看依赖，使用者按输出补齐；不据此保证该命令自动下载所有依赖。随后通过 `pull` 获取 `archlinux:current` 根文件系统，再运行该目录。此步骤属于帖子示例，未验证其当前镜像可用性。

## 容器 DNS

另一帖给出的网络排查操作是修改容器内的 `/etc/resolv.conf`；每个容器都要分别修改，之后再尝试下载软件。帖子没有给出 DNS 服务器地址，因此这里不指定地址。

## 编辑器配置

原帖使用 AstroNvim 模板。以下只适用于 `~/.config/nvim` 尚不存在的新配置目录；命令中的模板地址来自原帖链接：

```sh
git clone --depth 1 https://github.com/AstroNvim/template ~/.config/nvim
rm -rf ~/.config/nvim/.git
nvim
```

删除 `.git` 仅去掉新模板的版本历史，不是启动编辑器的必要条件。`.rurima.env` 中的 `EDITOR` 可改为实际安装的编辑器；不需要固定使用 Neovim。

## 其他资源

- Rurima 使用文档：<https://github.com/RuriOSS/rurima/blob/main/doc%2FUSAGE.md>
- Linux Containers 镜像目录：<https://images.linuxcontainers.org/>（镜像索引页，不是 ASL 固件或 ZIP 直链）
- AstroNvim 文档：<https://docs.astronvim.com/>
- AstroNvim 模板：<https://github.com/AstroNvim/template>

来源：<https://www.coolapk.com/feed/69054461>、<https://www.coolapk.com/feed/69083921>、<https://www.coolapk.com/feed/69104127>、<https://www.coolapk.com/feed/69105650>。帖子中的图片未读取。
