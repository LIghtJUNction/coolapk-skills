---
name: legacy-magisk-template-migration
description: 阅读历史 Magisk 模块模板、公开下载与提取码，并迁移 module.prop、安装日志和路径映射知识到当前接口。
---

# Magisk 模块模板结构与下载

Magisk 模块 ZIP 的目录布局决定文件如何安装和映射。旧教程中的模板含 `common/`、`META-INF/`、`system/`、`config.sh` 和 `module.prop`；这是历史模板结构，不应直接视为当前必需布局。当前模块通常以模块根目录中的 `module.prop`、可选脚本和 `system/` 内容组成。

## 获取旧版示例模板

第一期原帖列出的百度网盘模板地址：<https://pan.baidu.com/s/1OLy3uaTbwrz2YJqvGMxBQg>，提取码：`m3of`。此提取码只对应这份百度网盘模板。

整理更新帖列出三条蓝奏云工具接口地址：

- config 接口：<https://www.lanzoux.com/b0be98ibc>
- install 接口：<https://www.lanzoux.com/b0be98ikb>
- customize 接口：<https://www.lanzoux.com/b0be98igh>

原帖自称 config 兼容 12–20.0，install 写为 20.2+（原帖另提 27.0+），customize 写为 20.4+（原帖另提 27.0+）；这些是作者当时给出的范围，未对下载包进行核验，不能当作当前版本兼容保证。蓝奏页面当前无法核读，缺少的访问密码不作猜测。

评论中作者另提供了爱玩机工具箱下载：<https://www.123912.com/s/DuvbVv-gliA3>。这是工具箱资源，不是模块模板包。

## 迁移到当前接口

旧模板的目录不能直接套到当前版本。先读[当前接口核对](references/current-interface.md)，再按需要查阅[module.prop](references/module-prop.md)、[安装排错](references/install-errors.md)和[路径与属性](references/paths-properties.md)。

来源：[15831897](https://www.coolapk.com/feed/15831897)、[9507020](https://www.coolapk.com/feed/9507020)。工具箱下载链接来自原帖作者回复，未读取下载包。百度、蓝奏和123包均未验内容或有效性。
