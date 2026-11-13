---
name: windows-llvm-mingw-clion
description: 在 Windows 为 CLion 配置 LLVM MinGW UCRT 工具链，识别宿主与目标架构，保留实际发行包链接及完整 clang-format 示例。
---

# 在 CLion 配置 LLVM MinGW 工具链

**来源：**[原文《配置 LLVM 工具链》](https://yumeyuka.moe/notes/llvm/index.md)

## 适用场景与下载

这篇笔记讲的是在 Windows 上用 LLVM MinGW 工具链进行 C/C++ 开发，并以 CLion 为 IDE。原文还列出 [Visual Studio Code](https://code.visualstudio.com/)、[CLion](https://www.jetbrains.com/clion/) 和 [Visual Studio IDE](https://visualstudio.microsoft.com/zh-hans/vs/) 作为开发工具选项。

原文给出的发行包为 `llvm-mingw-20260616-ucrt-x86_64.zip`：

[下载 llvm-mingw-20260616-ucrt-x86_64.zip](https://github.com/mstorsjo/llvm-mingw/releases/download/20260616/llvm-mingw-20260616-ucrt-x86_64.zip)

下载 Release ZIP，解压到固定目录，再把工具链目录加入 `PATH`。这个 ZIP 包运行在 Windows x86_64 宿主，使用 UCRT；包名中的 x86_64 不是只能编译该目标的限制。[上游 README](https://github.com/mstorsjo/llvm-mingw#readme)明确说明 Windows 包可编译多个 Windows 目标架构。原文没有给出具体安装目录、CLion 版本、完整工具链字段或 CMake Profile 配置截图的文字内容，故不能从缺失图片推断逐项填写值。

在 CLion 中添加 LLVM 工具链，然后在 CMake 工具链/配置处选择该工具链。解压目录和 `PATH` 配好后，可创建新项目验证 IDE 能否找到编译器与构建工具；原文展示了运行画面，但没有提供命令行输出或测试记录。

## C++ 示例

```cpp
#include <cstdio>

auto main() -> int {
    printf("Hello, World!");
    return 0;
}
```


## 格式化配置

[完整 clang-format 示例](references/clang-format.md)保留了原文功能性配置，包括 LLVM 基线、106 列、4 空格、指针/引用和 include 分类。原文没有标注格式化器版本；协调者用本地 clang-format 23.1.4 解析并格式化简单 C++ 输入成功，不能据此保证其他版本都识别全部选项。


本技能来自酷安原帖所链接的公开站点文章，单独作为外链知识整理；不算作酷安帖子正文覆盖。正文已读，配图未读。除单独注明的本地验证外，未运行项目、安装扩展或下载发行包。
