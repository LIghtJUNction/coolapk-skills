---
name: magisk-module-skill-discovery
description: "当需要为 AI 辅助开发 Magisk、APatch 或 KernelSU 模块寻找现成技能或实战案例线索时使用；对不完整链接保持待补状态。"
---

# 查找 AI 开发模块的技能与案例

## 适用场景
需要了解 AI 辅助开发 Magisk、APatch 或 KernelSU 模块的技能和案例入口时，使用本技能整理线索并核实链接。

## 安装技能与查看案例

Kam 仓库提供模块开发技能。原帖写出的地址省略了协议冒号，下面已按原帖说明补全：

- [Kam 技能仓库](https://github.com/MemDeco-WG/Kam)
- [MagicNet 实战项目](https://github.com/LIghtJUNction/MagicNet)

安装命令：

```sh
npx skills add https://github.com/MemDeco-WG/Kam
```

从仓库的 `.agents/skills` 目录查看技能内容，按安装器提示选择需要的技能和目标 Agent。然后将具体模块需求、Root 管理器、目标系统、已有项目路径和验收条件交给 Agent；可参考 MagicNet 的项目实现。安装命令来自原帖，仓库目录已核对；未在本次整理中执行安装或验证具体设备上的模块。

模块的构建、安装与设备验收分别记录。技能安装成功不等于生成的模块已适配设备；需要按具体项目补充构建和设备测试结果。

## 来源
- [Coolapk 帖子 73871864](https://www.coolapk.com/feed/73871864)


详见：

- [MagicNet 功能演进、网络冲突与开发反馈经验](references/magicnet.md)
- [MagicNet 网络过滤、TUN 路径与构建安装记录](references/magicnet-operations.md)
