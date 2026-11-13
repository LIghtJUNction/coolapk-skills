# 红米 K80 Pro：区分 AVF 测试路径

## 社区证据

[社区测试记录](https://www.coolapk.com/feed/66587066)使用 LineageOS 22.2 源码构建的 Microdroid demo。作者报告：在红米 K80 Pro 上，demo 未能进入终端，而脚本测试可运行。这只能支持两条测试路径表现不同，不能单凭 demo 失败认定设备不支持 pKVM。

## 通用排查顺序

1. 记录设备完整系统、AVF 组件、demo 构建来源以及使用的测试路径。
2. 在已连通并获授权的 ADB shell 中，先查询当前组件帮助：

   ```sh
   /apex/com.android.virt/bin/vm --help
   /apex/com.android.virt/bin/vm run-microdroid --help
   ```

3. 将“组件存在”“命令可解析”“创建虚拟机”“连接终端”“运行载荷”分别验收。保留私有日志，排除应用权限或终端连接失败后再判断虚拟化能力。
4. 若准备主动启动虚拟机，先核对对应系统和源码分支的项目说明，不从其他手机复制解锁、刷系统或启用 pKVM 的命令。

[AOSP 入门说明](https://android.googlesource.com/platform/packages/modules/Virtualization/+/refs/heads/main/docs/getting_started.md)提供通过 `vm_shell.sh` 测试 Microdroid 的路径；其列举的支持设备与准备条件不能当作所有安卓手机的保证。

2026-11-13 已读社区文字并核对上述官方说明；图片未读，本库未在 K80 Pro 执行这些检查。社区关于其他 Android 15 出厂设备的建议属于待验证范围。
