---
name: fas-rs-frame-aware-scheduling
description: 配置 Android fas-rs 的游戏列表、四种调度模式与 Lua 插件，区分 v4.6/4.7/4.9 发布变化和后续帧线程设计；含实际镜像分享页及提取码。
---

# fas-rs：Android 用户态帧感知调度

## 项目定位与适用条件

fas-rs 是运行在 Android 用户态的帧感知调度项目，采用 GPLv3。项目介绍帖给出的运行条件是：SoC 平台为 arm64、Android 12 及以上、目标应用为 arm64 应用；具体版本可能还有其他要求。发帖文本未提供设备实测结果或成功率。

## 游戏配置与模式

配置文件路径为：

`/sdcard/Android/fas-rs/games.toml`

该文件是标准 TOML。可在 `game_list` 表中按默认配置填写需要生效的游戏包名，不要添加重复包名。自 v2.2.3 起，也可通过 scene 的“我的游戏”功能添加生效游戏。

fas-rs 提供省电、均衡、性能、极速四种模式。项目介绍帖推荐使用 scene 动态切换模式；也可配合 [perapp-rs](https://github.com/shadow3aaa/perapp-rs) 模块动态切换。不使用动态模式时，介绍帖称默认始终为均衡模式。

当前安装版本的完整说明文件位于：

`/sdcard/Android/fas-rs/doc_cn.md`

## Lua 插件

用户侧插件通常以独立模块形式存在。项目介绍帖链接了 [插件开发模板](https://github.com/shadow3aaa/fas-rs-extension-module-template)。帖子称插件系统致力于向后兼容：fas-rs 更新插件 API 版本后，旧插件仍可使用旧版本 API。该表述是项目介绍中的兼容性说明。

## 发布记录

以下条目是各发布帖列出的变更，不应据此推断未列出的行为或当前最新版本。

### v4.6.0（2025 年发布帖）

- 更新 `margin_fps` 参数。
- 修复关闭 `feas` 等冲突功能时失效的问题。
- 改进错误处理，记录更详细的错误原因和回溯信息。
- 将 Rust edition 降至 2021，以优化兼容性；同时加入 nightly Rust toolchain 配置。
- 移动构建环境变量设置到 `cargo_ndk` 函数中。
- 删除有 bug 的 `check-license` 工作流文件。
- 修复 `cargo lint` 未覆盖 `xtask` 的问题。
- 撤回生成构建元信息并改变构建输出格式的改动。

### v4.7.0（2025 年发布帖）

- 通过适当采样验证频率控制是否生效；帖子没有给出采样周期，不改写成“调整采样频率”。
- 调整内核版本要求，使之更符合实际情况。
- 修复迁移到 Rust edition 2024 时的编译错误，并使用新的 rustfmt 规则格式化。
- 修复可能无法达到目标帧率的问题。
- Rust edition 从 2021 升至 2024（帖内称该 edition 已稳定）。
- 更新多项依赖，包括 anyhow、clap、libc、log、serde、serde_json 和 zip。

### v4.9.0（2025 年发布帖）

- WebUI 增加应用选择；游戏列表中点击编辑会弹出编辑界面；已在应用列表中的应用不可重复添加。
- 修复游戏配置修改不会自动保存、应用选择菜单空白、删除游戏对话框未显示包名、亮暗主题卡片适配等问题；调整弹窗透明度并重新设计 WebUI。
- API/WebUI 实现与依赖有多项调整：API 改为只监听 localhost；API 实现曾更换并有撤回 Rust API 的提交记录；WebUI API 改用 KernelSU package。发布日志也记录了 CORS 测试。
- 构建脚本增加 npm 安装步骤，并更新相关锁文件；清理临时 NDK，使用 `mkdir -p` 避免目录已存在时报错。
- 多项依赖更新，包括 anyhow、clap、flexi_logger、libc、log、mimalloc、next、nix、quick-xml、toml、zip。

## 项目与下载

- 项目仓库：[fas-rs](https://github.com/shadow3aaa/fas-rs)
- GitHub Releases：[版本发布与下载](https://github.com/shadow3aaa/fas-rs/releases)
- 蓝奏云镜像分享页：[fas-rs 下载](https://wwr.lanzoui.com/b019jb8mb)，公开提取码：`22bw`
- 项目介绍帖：[酷安项目主页](https://www.coolapk.com/feed/54082861)
- perapp-rs：[GitHub 仓库](https://github.com/shadow3aaa/perapp-rs)
- Lua 插件模板：[GitHub 仓库](https://github.com/shadow3aaa/fas-rs-extension-module-template)

链接类型说明：GitHub Releases 是版本发布页；蓝奏云地址是分享页而非已验证的文件直链。原始输入未提供下载校验结果。

## 来源与证据边界

使用说明和适用条件来自 [54082861](https://www.coolapk.com/feed/54082861)；v4.6.0 来自 [62814840](https://www.coolapk.com/feed/62814840)；v4.7.0 来自 [63051527](https://www.coolapk.com/feed/63051527)；v4.9.0 来自 [64618435](https://www.coolapk.com/feed/64618435)。后三帖的更新日志是项目发布信息，未独立验证其修复效果。输入未包含评论或可读图片内容。
## 后续帧线程设计说明

[后续原帖 70459297](https://www.coolapk.com/feed/70459297)称，可以识别决定帧生成时间的线程，只提升这些线程所在 CPU 的频率，不再手动设置 offset。原帖称其为“fas2.0”，这是该条设计说明的用语，不能与上文软件发行号混为一谈；没有算法源码片段、性能数据或完整设备要求，不补写实现与收益。

v4.9.0 的“只监听 localhost”、更换 API 实现与撤回 Rust API 都是提交日志记录，不足以证明当前发行包仍运行特定 HTTP 监听器，也不能据此生成跨机访问配置。“用户态”描述执行位置，不等于无需 Root；安装方式、管理器与内核条件应以实际版本文档为准。


## 较早版本与关键路径研究

- [v3.1 / v3.3 / v3.8 发布记录](references/v3-release-history.md)：最小频率集群、offset、温控、P 控制和历史修复，不能把不同提交视为同时生效的当前规则。
- [CPCS 关键路径预算分配](references/cpcs-critical-path.md)：已读取原理文章，区分关键链查找、执行时间权重与未公开实现，并纠正其帧耗时反馈方向的文字解释。


## 补充来源案例

[v1.0.0历史配置与Termux构建](references/v1-config-build.md)补齐games.toml、模式节点、配置合并和可恢复的clone地址；[v3.7与v4.5变动](references/v3-7-v4-5-changes.md)区分旧字段、CPU利用率分析变化及需重下的早期构建。
