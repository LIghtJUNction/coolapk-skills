# iPad Pro M4 本地模拟运行 macOS (Henge) 配置与权限排障

> 整理自酷安公开动态正文；图片、评论及外链未逐一核验，实际操作请以对应硬件版本与上游仓库为准。

- **iPad Pro M4 本地模拟运行 macOS (Henge) 配置与内存权限排障**
  - [酷安原帖](https://www.coolapk.com/feed/74922367)（`imgs=2` 未读）
  - 项目架构：开源项目 Henge（开发者 Leviidev，GPL-2.0-or-later），在 iOS/iPadOS 本地模拟运行完整 macOS（测试版本 Sequoia 15.6.1）。采用独立 ARM64 模拟器、JIT 执行引擎、MMU 与虚拟设备，基于 Metal 处理虚拟 GPU 图形渲染，不依赖 QEMU 或 Apple Virtualization.framework。
  - 运行环境与要求：iPad Pro M4（iPadOS 27.0.1）；虚拟机分配内存 4GB/6GB/8GB；macOS Sequoia 15.6.1 下载大小 16.81GB，安装后占用约 26GB，建议预留 43GB+ 空间；JIT 环境采用内置 StikJIT Helper。
  - 核心权限避坑（内存分配失败报错）：
    - 现象：报错 `Cannot build the machine (not enough memory for the guest's RAM?)`，Settings 显示 `This install isn't signed with the increased memory limit entitlement.`；
    - 根因：侧载签名缺少 `increased-memory-limit` 和 `extended-virtual-addressing` 内存上限权限；
    - 解决：通过电脑端工具 iLoader 2.4.1 连接 iPad，将 Henge IPA 重新签名并写入完整内存权限后安装，确认 Settings 中内存上限识别达到 8.59GB 后方可成功创建分配。
  - 局限性：模拟性能弱于原生，首次进系统后台初始化需数分钟，macOS 内 Apple 账户登录验证存在问题，单次仅允许运行一个虚拟机。附 GitHub 仓库与 Release 直链。
