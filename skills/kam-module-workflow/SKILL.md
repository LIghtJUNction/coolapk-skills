---
name: kam-module-workflow
description: 初始化和维护 Kam 模块项目、模板、构建与发布钩子时使用；记录来源对应版本。
---

# Kam 模块项目初始化与构建

本文汇总帖子记录的 Kam 0.3.x 至 0.5.x 用法。命令和行为可能随版本变化；内容依据帖子整理，未实机验证。

## 安装与模板

Termux 中可按帖子记录安装：

```sh
pkg install rust openssl
cargo install kam
```

旧版帖子也分别记录了 `pkg install rust`、`pkg install openssl`。若 `cargo` 不在 PATH，检查当前 shell 并将 `~/.cargo/bin` 加入对应 shell 的启动配置；帖子给出的示例为：

```sh
echo $0
export PATH=~/.cargo/bin/:$PATH
```

模板仓库可通过 GitHub 的模板功能创建项目，然后运行带 `init` 字样的工作流，帖子建议使用默认模板。已有项目也可先导入模板：

```sh
kam tmpl import templates.zip -f
kam init . --kam
```

`-f` 会强制覆盖本地缓存中的同名模板；初始化时不加覆盖选项则不会覆盖现有文件。单个模板导入示例为 `kam tmpl import xxx.zip` 或 `kam tmpl import yyy.tar.gz`，帖子将 tar.gz 用于单个模板。`kam init -i` 是交互式初始化方式。

模板管理命令：

```sh
kam tmpl pull
kam tmpl path
kam tmpl remove xxx
kam tmpl import 模板文件
```

`kam tmpl pull` 获取默认基础模板；也可附加下载链接指定模板。模板导入的链接与文件格式支持范围以具体版本为准。

## 项目结构和配置

`kam --version` 查看工具版本，`kam version` 查看项目版本，两者用途不同。钩子可使用 `KAM_PROJECT_ROOT` 定位项目根目录。

项目根目录包含 `kam.toml`，模块源文件放在 `src/<module_id>/`。帖子说明构建时会从 `kam.toml` 更新 `module.prop` 与 `update.json`；无需手动编辑这些生成文件。构建会读取根目录 `.env` 中的环境变量，模板含 `.env.example`。帖子建议不要把 `.env` 上传到公共仓库。

如需编译 Rust 子项目，可使用 `crates/` 等自定目录。前端构建产物应由项目自己的逻辑放入模块 `webroot`；帖子指出 Kam 提供 `KAM_WEB_ROOT`，并建议避免硬编码路径。

## 构建与版本

```sh
kam build
kam build -r
kam build -a
kam build -s
kam build -i
kam build -a -s -r
```

帖子对这些参数的解释是：`-r` 启用发布标记，设置 `KAM_RELEASE=1`；`-a` 构建工作区内全部子项目；`-s` 设置签名相关环境，模板钩子使用 `kam sign`；`-i` 为交互式构建。组合示例用于构建全部子项目、签名并发布。实际发布动作由 hooks 中的脚本定义；示例脚本依赖 GitHub CLI `gh`，本地使用需安装并执行 `gh auth login`。这些行为属于帖子对应版本的说明。

版本命令示例：

```sh
kam version
kam version patch
kam version v1.0.0
```

帖子称 `patch` 会递增补丁版本号及 versionCode。另有 `kam version (major, minor, patch)` 的工作流输入示例，原帖未给出可直接复制的精确语法。

## Hooks

Hooks 按名称中的数字顺序执行；帖子早期描述的命名模式为“数字.功能.文件后缀”，后续模板约定为“四位数字.钩子名称.后缀”。这是不同帖子中的命名表述，使用时应查看对应模板的约定。

帖子给出的执行权限命令有两种版本记录，须按来源区分：

```sh
chmod +x hooks/*/*
```

这是 69234886 的原帖命令。另两篇迁移/构建帖子记录：

```sh
chmod +x hooks/**/*
```

它们是不同的原文命令，不应互相替换。shell 对 `**` 的展开取决于 shell 配置；帖子未说明其展开条件。

编写钩子时，若依赖外部命令，可调用模板工具库中的 `require_command`，例如 `require_command cz`；缺少所需命令时构建流程会终止。钩子脚本还可从 `$KAM_HOOKS_ROOT/lib/utils.sh` 导入工具。前端模板带有 post-build 钩子；不需要 WebUI 时，帖子建议移除此钩子及 `src/id/webroot`。

钩子工作流支持预构建、打包和发布等逻辑。模板示例使用 `gh release create` 上传构建产物；真实上传行为由项目钩子决定。

## 其他开发提示

帖子记录可用 `commitizen`（命令 `cz`）生成 changelog；示例安装方式为 `uv tool install commitizen` 或 Arch 系的 `paru -S python-commitizen`，并以 `cz ch` 生成 changelog。Kam 模板还可使用 `cargo-xtask`；帖子提及 `uv run` 与 PEP 723 作为 Python 脚本依赖管理方式；原帖把 `uv run` 与 `uvx` 混称的说法不沿用。

CI 帖子描述首次执行较慢、后续可能受缓存影响；未提供基准数据。

## 资源

- Kam 主项目与版本发布：<https://github.com/MemDeco-WG/Kam>、<https://github.com/MemDeco-WG/Kam/releases/tag/0.3.2>
- Kam 模块模板仓库：<https://github.com/MemDeco-WG/KamModuleX>
- Hooks 仓库：<https://github.com/MemDeco-WG/KamHooks>
- 工作流 action：<https://github.com/MemDeco-WG/setup-kam>
- 示例发布钩子：<https://github.com/MemDeco-WG/Kam/blob/main/hooks%2Fpost-build%2F3.UPLOAD_IF_ENABLED.sh>
- Kam 配置示例：<https://github.com/MemDeco-WG/Kam/blob/main/kam.toml>

来源：<https://www.coolapk.com/feed/69269555>、<https://www.coolapk.com/feed/69099755>、<https://www.coolapk.com/feed/69106370>、<https://www.coolapk.com/feed/69234886>。帖子中的图片未读取；外链页面内容未在本次资料中提供。
