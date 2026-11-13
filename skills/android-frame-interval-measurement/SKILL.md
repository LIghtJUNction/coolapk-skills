---
name: android-frame-interval-measurement
description: 比较Android SurfaceFlinger历史帧记录、libgui提交插桩与Binder dump，核对时间戳、命令横线和C++成员函数ABI。
---

# Android 帧间隔测量方法

Android 上观测帧间隔有三种不同路径：读取 SurfaceFlinger 的历史帧数据、在 `libgui` 的提交路径插桩、读取 SurfaceFlinger 的 Binder dump 接口。它们观测的阶段、开销和实时性不同，所得间隔不能一概解释为实际显示帧率。

## 1. dumpsys：读取历史帧记录

先列出 SurfaceFlinger 当前层名称，再把实际存在的层名赋给 `LAYER`，然后查询该层：

```sh
dumpsys SurfaceFlinger --list
LAYER='在上一步输出中确认的真实层名'
dumpsys SurfaceFlinger --latency "$LAYER"
```

这两个命令的选项使用 ASCII 双连字符 `--`。AOSP Android 13 的 `SurfaceFlinger.cpp` 已核对 `--latency` 和 `--list`；原帖中的排版字符 `–latency` 是错误的。这里只确认 Android 13 该源码版本，不保证更新版本仍支持相同选项。

原帖所引历史实例称返回最近 127 帧；实际窗口长度须以对应系统实现为准。第一行是 vsync 周期，后续记录每行三列，顺序固定为：

1. `desiredPresentTime`：目标呈现时间。
2. `actualPresentTime`：实际呈现时间。
3. `frameReadyTime`：帧就绪时间。

这些字段是时间戳，不应把 `frameReadyTime` 直接解释为 vsync 时间，也不应把 `desiredPresentTime` 简化成应用开始绘制的时刻。相邻 `frameReadyTime` 的差可以作为就绪时间间隔的观测值；相邻 `actualPresentTime` 的差描述实际呈现时间戳间隔。二者回答的问题不同，都不能脱离记录状态直接等同于稳定物理上屏帧率。

计算间隔前，先检查所选测量列的有效性：零值、待处理的 `INT64_MAX`、重复或非递增值不能直接当作正常间隔进行差分。不要仅凭另一列的特殊值判断当前列是否有效，具体含义须看对应版本实现。历史记录可能被复用；计算差值前须检查顺序和有效性，不要盲目逐行相减。

第一行的 vsync 周期不是应用或 layer 的目标 FPS。`16666666 ns`（约 `16.67 ms`）只是旧示例中的周期值，不能据此推定目标帧率或当前显示模式。

此方式简单、能按 layer 查看历史记录，但读取的是有限历史窗口，轮询还会受到采样时机影响；相对直接插桩，其实时性和效率较弱。vsync 节奏与应用目标帧间隔不同，也会使观察到的间隔呈现不均匀分布。

## 2. libgui 插桩：观测提交调用

另一种方法是在应用侧 `libgui` 的 `Surface::queueBuffer` 路径记录调用时刻，并计算同一 Surface 上相邻调用的时间差。这观测的是 buffer 提交调用间隔，不是 GPU 完成时间，也不是屏幕实际呈现间隔。

Surface.cpp 的相关路径接收 fence，再将 buffer 提交到队列。因此，插桩数据应归因于提交路径的调用观测。传入 fence 描述待满足的依赖，不能仅凭调用发生判断 GPU 已完成，更不能保证该帧已呈现。

实现时应按 Surface 或目标线程隔离计时状态，首个样本只建立基准、不产生间隔；并发访问须同步或采用合适的线程局部状态，避免不同 Surface 或线程的样本互相污染。入口也可能对应失败的提交，应结合原函数返回值区分提交尝试与有效帧。若需把间隔传给控制程序，还要另行实现 IPC，例如 Binder 或 domain socket；仅记录时间差并不包含传输实现。

原帖给出的代码有 `#inculde` 拼写错误、未完成的省略内容，并把非静态成员函数写成仅有 buffer 与 fenceFd 两个形参的形式。`Surface::queueBuffer` 是非静态成员函数，调用约定中还存在隐式 `this` 对象指针。匹配目标平台 ABI 的 hook 必须正确保留对象指针、buffer 和 fenceFd，并遵守对应返回值及调用约定；不能把原帖两个显式形参版本当作可编译、通用即用的 hook。

可将插桩逻辑理解为以下伪代码，不是可直接编译或部署的 hook：

```text
在匹配目标 ABI 的 queueBuffer 入口：
  取得当前 Surface 对象、buffer、fenceFd
  用单调时钟读取当前时刻
  按 Surface（必要时再按线程）查找上次有效时刻
  若不存在上次时刻：保存当前时刻，不输出间隔
  否则：计算当前时刻与上次时刻的差，并更新状态
  按原调用约定转发对象指针、buffer、fenceFd 到原函数
  将结果交给独立实现的记录或 IPC 通道
```

原帖符号字符串 `_ZN7android7Surface11queueBufferEP19ANativeWindowBufferi` 可作为历史版本的定位线索。符号解析和成员函数 ABI 会随平台、构建和 Android 版本变化；该字符串不保证在所有设备或 ABI 上存在或匹配。Itanium C++ ABI 文档可用于理解相关名称编码约定，但不能替代对目标二进制的核验。

原帖提及 Zygisk、ptrace 和 patchelf，作为向目标进程注入或加载插桩代码的入口方向。其提到的注入器链接在现有资料中缺失，因此不补造 URL；也没有提供完整、经过验证的注入步骤。修改系统库依赖的兼容性和风险取决于设备、系统版本及 linker 配置。此处不提供系统修改脚本。

## 3. Binder dump：客户端直接请求 dump 接口

SurfaceFlinger 通过 Binder 服务提供 dump 接口；理论上可由客户端直接请求相关 dump 信息，以避免每次启动外部 `dumpsys` 程序的额外开销。此路径仍有 Binder IPC 成本，也不构成实时性保证。它读取的仍是 dump 所暴露的帧信息，不能因此消除历史记录、采样时机或 vsync 与目标间隔不一致带来的限制。

原帖作者明确表示当时没有研究完整实现，因此现有资料不包含客户端调用细节、代码、接口选择或可运行步骤。不能据此补写一套声称已经验证的 Binder 实现。

## 方法选择与解释边界

- 需要快速查看某个 layer 的历史呈现记录：可尝试 `dumpsys`，并按列含义和有效性检查时间戳。
- 需要高频观测提交路径调用间隔：可研究 `libgui` 插桩，但必须针对目标 ABI 实现和验证 hook，并将指标标记为提交观测。
- 需要避免外部 `dumpsys` 进程开销：可进一步研究 Binder dump；现有资料未给出完整实现，且 IPC 并非零成本或实时保证。

这些测量提供的是不同阶段的时间观测。调度或性能控制若要把它们用于决策，还需定义目标指标、异常样本处理和跨阶段验证；单一间隔值不足以证明用户实际看到的画面流畅度。

## 来源

- 原帖：[在 Android 中获取帧渲染时间（间隔）的通用方法记录](https://www.coolapk.com/feed/53879704)。原帖提供三种方法的概述、示例命令和作者对实现完整度的说明。
- [AOSP Android 13 `SurfaceFlinger.cpp`](https://android.googlesource.com/platform/frameworks/native/+/refs/heads/android13-release/services/surfaceflinger/SurfaceFlinger.cpp)：用于核对 Android 13 的 `--latency`、`--list` 选项及层查询相关代码。
- [AOSP 旧版 `FrameTracker.cpp`](https://android.googlesource.com/platform/frameworks/native/+/cb31cabec0411d3647be742d9be0605a4dcf6622/services/surfaceflinger/FrameTracker.cpp)：用于核对历史记录输出的三列顺序和有效值处理；未独立核对该版头文件中的窗口容量。
- [AOSP `Surface.cpp`](https://android.googlesource.com/platform/frameworks/native/+/a34de5209546e74a5ad6e4c348016d70271a6492/libs/gui/Surface.cpp)：用于理解提交路径对 fence 的处理。
- [Itanium C++ ABI 文档](https://itanium-cxx-abi.github.io/cxx-abi/abi.html)：用于了解 C++ 符号名称和 ABI 约定；实际 hook 仍须匹配目标二进制。
- 原帖提供的[金山文档入口](https://www.kdocs.cn/l/ctevMeObWUGE)：此入口未读取，不作为本文技术结论的独立依据。
