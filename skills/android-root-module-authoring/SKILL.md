---
name: android-root-module-authoring
description: 从 Root 模块历史模板学习安装脚本、架构判断、管理器分支、启动阶段、systemless 文件映射与 WebUI，并核对旧代码与当前 KernelSU/Magisk 接口的差异。
---

# Android Root 模块编写

原帖提供了仓库初始化、模块目录、安装与启动脚本、系统文件映射和 WebUI 的实例。[完整历史示例](references/legacy-template.md)保留具体代码，下面说明如何理解它的边界。

## 原帖的打包发布流程

[原仓库链接](https://github.com/LIghtJUNction/RootManage-Module-Model)所述流程是 fork → Actions 运行 `init` → 在 `MyModule` 编写 → 更新 `changelog` → 运行 `release`。大于 100MB 的文件通过 Git LFS 上传。实际读取 GitHub API 后确认，原地址现指向[已归档的 RootModuleMaker](https://github.com/MemDeco-WG/RootModuleMaker)；不要把历史步骤当作当前可执行的工作流承诺。

## 安装脚本与环境

- 使用 `customize.sh` 定制安装，原文不建议加入旧 `install.sh`。`ui_print` 输出安装信息，`abort` 中止安装流程。
- 安装期间使用管理器提供的 `$MODPATH`，不要写死安装目录。原示例按 `$KSU`、`$APATCH` 分支写入管理器标记文件；其余分支直接当成 Magisk 的做法需要额外核对环境。
- 架构示例只接受 `arm64`、`x64`，拒绝 `arm`、`x86` 和未列出的值。它不代表管理器本身只支持这两种架构。
- `module.prop` 标识模块，原模板称由 `init` 填入；权限应按目录、数据文件和可执行文件用途设置，而不是机械复制整目录 `0755`。

## 启动阶段与 systemless 内容

原例将 KernelSU 的 `boot-completed.sh` 改名为 Magisk `service.sh`，再通过 `getprop sys.boot_completed` 等待系统启动。文件名差异、运行时目录变量和等待阶段必须先核对，不能只改名就认为两套启动语义完全一致。

把静态编译、与目标架构及系统兼容的工具放入模块 `system/bin/`，是原文用 `yq` 展示的 systemless 映射思路。这不等于覆盖物理系统分区，也不保证任意 Linux 二进制能在 Android 运行。当前 KernelSU 系统目录挂载还需要相应 metamodule；Magisk 后端不同，见[官方模块说明](https://kernelsu.org/guide/module.html)。

## WebUI

`webroot/index.html` 可提供前端。原例仅将页面跳转至 `http://127.0.0.1:1234`，没有提供监听服务；不能把页面能打开等同于后端可用。APatch 或其他管理器的 WebUI 支持仍需核对具体实现。

来源：[酷安模块开发原帖](https://www.coolapk.com/feed/62289808)。这里未安装模块、未运行 GitHub Actions，也未验证手机或完整发布链路。

## 术语

[LKM、GKI、KMI 与目录含义](references/terminology.md)保留可核对的技术术语，排除未证实的命名起源。

## 安装结果与误执行案例

[安装结果验证与脚本执行记录](references/install-verification.md)区分日志提示、模块识别和实际功能；[历史模板迁移](../legacy-magisk-template-migration/SKILL.md)另列旧接口、公开下载和兼容边界。
