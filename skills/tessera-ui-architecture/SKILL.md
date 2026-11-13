---
name: tessera-ui-architecture
description: 理解 Tessera UI 的 Rust/wgpu 即时模式、可插拔渲染与计算管线、声明式组件和约束布局；区分 v1.0.0 测试状态、部分完成项与后续计划。
---

# Tessera UI：架构、布局与 v1.0.0 路线图

## 项目状态与定位

Tessera UI 是一个使用 Rust 和 wgpu 的即时模式 UI 框架。发布说明将 v1.0.0 明确称为测试版，并指出当时不建议用于生产环境。文中的功能状态、路线图和生产建议对应 2025 年发布时的情况，不代表当前版本保证。

## 渲染与组件架构

Tessera 把着色器作为一等公民：核心不提供类似“画刷”的内置绘制图元，而提供可插拔的 WGPU 渲染/计算管线。开发者可以编写自定义着色器实现光照、阴影、粒子等效果，也可以使用计算着色器完成图像处理或物理模拟。

核心不内置 UI 组件。`tessera_basic_components` 是可选的通用组件库；项目也可以组合它、替换它或自行创建组件库。自定义渲染管线的文档见 [高级用户：自定义渲染管线](https://docs.rs/tessera-ui/latest/tessera_ui/#-advanced-users---custom-rendering-pipelines)。

## 声明式组件与布局

使用 `#[tessera]` 宏，可以通过函数定义和组合组件。布局引擎使用 `Fixed`、`Wrap`、`Fill` 等约束，并提供 `row`、`column` 等布局组件，可用于构造从简单到复杂的响应式布局。

## 即时模式模型

每帧 UI 根据当前应用状态重新描述，可概括为 `UI = f(State)`。开发者通过下一帧是否绘制某个控件来反映状态变化，而不是维护控件的创建、更新和销毁生命周期。

动画可通过把时间作为 UI 输入来表达。发布说明还提出，即时模式便于并行化 UI 渲染和状态更新；这是设计意图，并非该帖给出的性能测试结论。

## v1.0.0 发布说明中的功能状态

发布帖将下列基础组件列入已划线的 v1.0.0 路线图：`row`、`column`、`boxed`、`text`、`spacer`、`text_editor`（部分完成）、`button`、`surface`、`fluid_glass`、`scrollable`、`image`、`checkbox`、`switch`、`slider`、`progress`、`dialog`。

框架层面列出的路线图项包括 Windows、Linux、macOS 输入法事件（部分完成）、窗口最小化处理和回调 API、窗口关闭回调 API。该帖明确说路线图事项可能已经完成，也可能推迟到 v2.0.0；不能仅凭划线把每一项当作完整交付。

## v2.0.0 计划（发布帖所述）

当时计划优化基础组件库文本框、增加 Android 和 iOS 输入法支持、增加基础组件并调整基础组件库样式。这些是计划，不是已交付状态。

## 获取示例、文档与参与项目

- 示例 crate：[Tessera example 目录](https://github.com/shadow3aaa/tessera/tree/main/example)
- API 文档：[docs.rs/tessera-ui](https://docs.rs/tessera-ui/latest/tessera_ui)
- 项目仓库：[shadow3aaa/tessera](https://github.com/shadow3aaa/tessera)
- 贡献指南：[CONTRIBUTING.md](https://github.com/shadow3aaa/tessera/blob/main/CONTRIBUTING.md)
- 发布帖提到的博客阅读版：[Tessera UI v1.0.0 release](https://shadow3aaa.github.io/blog/tessera-ui-v1.0.0-release/)

## 来源与证据边界

依据酷安发布帖 [66229520](https://www.coolapk.com/feed/66229520)。帖内 `counter.rs` 代码片段未能在采集文本中展示；图片共 6 张但未提供图片内容。本说明不补写代码，也不把发布帖中的设计主张当作独立性能验证。
链接保留原帖实际地址，其中 `latest`、`main` 会随上游变化，不是固定 v1.0.0 的 API 快照。本库没有编译示例或验证当前 API；读取的是酷安正文，外链文档与博客未读取。
