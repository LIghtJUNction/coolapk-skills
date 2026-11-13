---
name: termux-hexo-blog-setup
description: 在 Termux 安装 Hexo、初始化博客、本地预览和配置 Git 部署，区分示例参数与实际发布目标。
---

# 在 Termux 中搭建 Hexo 本地博客

Hexo 官方还列出 Git 为前置依赖，原帖没有给出 Git 安装步骤；开始前先确认环境已有 Git。

## 安装 Node.js 与 Hexo

更新 Termux 包索引：

```sh
pkg update
```

升级已安装的包：

```sh
pkg upgrade
```

两条命令作用不同：`pkg update` 刷新包索引，`pkg upgrade` 升级已安装包。原帖称二选一不严谨；是否执行升级由维护者按需决定。

安装 Node.js 并安装 Hexo CLI：

```sh
pkg install nodejs
npm install -g hexo-cli
```

初始化名为 `myblog` 的项目并进入目录：

```sh
hexo init myblog
cd myblog
npm install
```

原帖中的“(名字)”和“(刚刚那个名字)”是说明文字，不是 shell 命令内容。若使用其他项目名，应在初始化和 `cd` 命令中使用同一个目录名。

## 启动本地预览

```sh
hexo server
```

默认可在本机开发地址 http://localhost:4000 访问。此地址只用于本地预览，不代表博客已经公开托管。

常用命令：

```sh
hexo new "文章标题"
hexo generate
hexo deploy
```

`hexo-deployer-git` 是 Git 部署插件。安装插件本身不表示部署已完成：

```sh
npm install hexo-deployer-git --save
```

## 配置 Git 部署

在项目的 `_config.yml` 中配置目标仓库和分支：

```yaml
deploy:
  type: git
  repo: <目标仓库 URL>
  branch: <目标分支>
```

`repo` 和 `branch` 都是需要替换的占位内容；原帖中的 `[branch]` 不是实际分支名。执行 `hexo deploy` 前应核对仓库 URL、分支及访问权限，避免推送到错误目标。

## 来源与官方文档

原帖：[酷安技术帖](https://www.coolapk.com/feed/63495981)

Hexo 官方文档：https://hexo.io/docs/

Hexo 命令参考：https://hexo.io/docs/commands

原帖视频编号：`BV11kXqYWE9M`。视频正文未读取。
