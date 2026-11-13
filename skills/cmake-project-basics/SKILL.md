---
name: cmake-project-basics
description: 创建最小 CMake C++ 项目，区分源码和构建目录、修正 cmake --build 拼写，并理解目标源码路径与 CMakeLists.txt 的对应关系。
---

# CMake 入门笔记

**来源：**[原文《Cmake 笔记》](https://yumeyuka.moe/notes/cmake/index.md)

## CMake 做什么

CMake 是“元”构建系统：它根据环境与工具链生成平台对应的构建文件，本身不负责直接编译整个项目。原文举例：Linux 常用 GCC、Windows 常用 MSVC、macOS 常用 Clang；CMake 用生成器连接项目配置与具体构建系统。

## 安装

- 官网下载页：[https://cmake.org/download](https://cmake.org/download)。Windows 可用 MSI 安装包，或解压 ZIP 并将目录加入 `PATH`。
- Arch Linux 示例使用 yay：

```shell
yay -S cmake
```

安装后可用以下命令检查版本。原文示例输出为 CMake 4.4.3（这是原文记录的版本，不是当前版本承诺）：

```shell
cmake --version
```

```text
cmake version 4.4.3

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

## 指定源码与构建目录

原文列出 `-S` 指定源码目录、`-B` 指定构建目录，并在构建目录中运行构建。其命令块把 `cmake -- build` 写成了带空格的形式；正确的常用子命令拼写是 `cmake --build`：

```shell
# 源码目录下必须有 CMakeLists.txt
cmake -S <dir> -B <build-dir>

# 在指定构建目录中构建
cmake --build <build-dir>
```

将 `<dir>` 与 `<build-dir>` 换成实际路径。把构建产物放在独立目录可减少源码目录中的生成文件；若只执行 `cmake -S <dir>`，CMake 会采用默认构建目录行为，后续仍应明确配置和构建目录以免混淆。

## 最小项目

CMake 配置文件通常命名为 `CMakeLists.txt`，顶层和子模块都可以有自己的列表文件。原文给出的最小示例要求 CMake 3.23 或更新版本，并定义一个由 `main.cpp` 构建的可执行目标：

```cmake
cmake_minimum_required(VERSION 3.23)

project(MyProject)

add_executable(MyProject main.cpp)
```

示意目录包括 `src/main.cpp`、`include/myproject/`、顶层 `CMakeLists.txt`、`README.md`、`.gitignore` 和 `build/`。若 `main.cpp` 实际放在 `src/`，目标参数也应相应改成 `src/main.cpp`，或调整源码目录结构；原文示例本身使用顶层 `main.cpp`。

原文后续“配置文件介绍”标为未完待续，没有进一步说明生成器选择、工具链文件、依赖管理或跨平台配置，不应把这些内容当作该笔记已覆盖的教程。
## 本地验证与可运行示例

协调者在 Linux 的 CMake 3.31.6 上验证了上述最小 CMakeLists.txt，配合以下 `main.cpp` 完成配置、构建并输出 `Hello, World!`。此文件用 LLVM 笔记中的简单示例补全，不声称验证过 Windows/CLion。

```cpp
#include <cstdio>
auto main() -> int {
    printf("Hello, World!");
    return 0;
}
```

```shell
cmake -S . -B build
cmake --build build
```


本技能来自酷安原帖所链接的公开站点文章，单独作为外链知识整理；不算作酷安帖子正文覆盖。正文已读，配图未读。除单独注明的本地验证外，未运行项目、安装扩展或下载发行包。
