---
name: ace2-susfs-kernelsu-adaptation
description: "记录帖子提及的机型、功能组合和 GitHub 工作流资源；没有固件版本或构建细节可供确认。"
---

# ACE2 的 SUSFS 与 KernelSU 适配资源记录

帖子提到 ACE2 的 SUSFS 与 KernelSU 适配项目，并称可提供 GitHub 工作流。帖子同时表示适配需求需提供机型、是否需要 SUSFS、Root 管理器和开源偏好等信息。

## 可确认范围

- 帖子明确提到的机型标识：ACE2。
- 提到的组件组合：SUSFS、KernelSU。
- 资源是 GitHub 项目链接；输入未说明其中是否包含可直接刷入的构建产物。
- 固件完整版本、地区、升级或降级类型、清数据要求、构建工具、恢复材料和实际适配结果均未知。

项目仓库未读取，因此不能据此给出安装、刷写或恢复步骤。来源：[帖子 74362813](https://www.coolapk.com/feed/74362813)。

## 编译与历史排障

[内核编译、清单选择与 SUSFS WebUI](references/build-and-history.md)保留 repo 同步、构建脚本、common 分支差异、历史构建配置和具体路径。损坏的原命令与经过仓库回读的修正版分开列出；未编译、刷入或验证设备。


## 源码版本声明的核对

[版本号、上游提交与构建证据](references/source-version-provenance.md)说明修改Makefile、引入功能、启用配置和实际运行是不同证据；该通用区别不扩大本技能的机型适配清单。


## 来源补充

- [ACE2公开项目与适配输入字段](references/project-entry.md)
