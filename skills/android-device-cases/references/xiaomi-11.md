# 小米 11 Neko-CL（FinaSU）自编译内核

来源：<https://www.coolapk.com/feed/74906254>

## 改动内容

- 内置 FinaSU v1.4.2 和 SUSFS，协议对齐 v2.3.0。
- 增加 5.4 兼容层，修复 5.10+ 源码在 vendor 5.4 内核树上的编译与链接问题。
- 将 SELinux 隐藏功能从较新内核回移到 5.4，分为两层：type 名称层和 access 规则位层。
- 默认 governor 从 `performance` 改为 `schedutil`。原先配置会使 8 个核心恒定满频；另修复 `schedutil` 早期启动挂死问题，方法是强制提供 PELT 信号。关闭 input boost 和 `io_is_busy`。
- 自带 AK3 刷机包，支持通过 TWRP 卡刷。

原帖没有提供具体补丁、构建命令、内核配置项名称或 PELT 信号实现细节；上述机制之外的实现细节无法从文字来源确认。

## 刷机准备与适用范围

1. 刷入前备份原始 boot，以便设备卡开机时恢复。
2. 确认管理器版本与内核内嵌的 FinaSU v1.4.2 匹配。
3. 原帖自测设备为小米 11：`venus` / `M2011K2C`，SoC 为 `SM8350`，运行澎湃 3 移植系统。

作者报告该设备上自测通过；其他 ROM 未验证。原帖提示刷机有风险，并建议反馈问题时附日志。未提供复现步骤或测试日志，因此无法补充测试覆盖范围。

## 项目与下载

- 源码仓库：<https://github.com/cvbyisme/venus-kernel-finaisu>
- 发布页（原帖建议优先从此处下载）：<https://github.com/cvbyisme/venus-kernel-finaisu/releases/tag/ak3-susfs-2>
- 网盘：<https://pan.huang1111.cn/s/alqLnTG>

原帖没有给出网盘提取码。发布页包含 AK3 包；原帖未列出具体文件名、校验值或版本对应关系。

## 许可

原帖说明内核部分遵循 GPL-2.0，构建脚本与文档采用 MIT 许可。
