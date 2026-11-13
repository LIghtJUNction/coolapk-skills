# 一加 Ace 2 内核编译与修改参考

## 适用范围

帖子描述的是一加 Ace 2 内核，提到内核标签 `android12-5.10`，以及 OKI 内核、KernelSU（KSU）、SUSFS 和 BBG 等配置。它们是帖子发布时的版本记录，不代表当前仓库或所有设备版本仍适用。

## 编译流程

帖子给出的流程是在 Linux 环境中操作；Windows 用户可使用 Linux 虚拟机或 WSL，也可直接安装 Linux。

1. 使用系统包管理器安装 `repo`。示例命令：

   ```sh
   pacman -S repo
   ```

2. 在空目录初始化仓库。帖子原样给出的命令为：

   ```sh
   repo init --partial-clone -u https:/@/github.com/LIghtJUNction/kernel_mainfest
   ```

   **命令疑点：** `https:/@/` 不是正常 URL；仓库路径拼成了 `kernel_mainfest`。此处与后文公开仓库 `kernel_manifest` 拼写不同。不要把这条损坏的 URL 擅自修正后当作已验证可执行命令。帖子称其他分支和文件已移除，未提供可核对的清单内容。

3. 同步源码：

   ```sh
   repo sync -c -j8
   ```

   帖子称同步是最耗时的一步。

4. 运行构建脚本：

   ```sh
   ./kernel_platform/oplus/build/oplus_build_kernel.sh
   ```

   脚本会显示英文交互式选项。帖子称流程完成即为编译完成，但没有提供实机启动或稳定性验证证据。

## 修改版同步方式

帖子称其修改主要放在 `kernel_platform/common`，并对应一个名称含 `common` 的仓库；修改上传到了该仓库的 `dev` 分支。主线仓库新增了清单文件，但帖子中的两个文件名不一致：前文写 `susfs_ksu_bbg.xml`，初始化参数却使用 `ksu_susfs_bbg.xml`。

原帖给出的命令（保留原样）：

```sh
repo init -u https:/@/github.com/LIghtJUNction/kernel_manifest -m ksu_susfs_bbg.xml
```

**命令疑点：** `https:/@/` 不是正常 URL；命令引用的 `ksu_susfs_bbg.xml` 与帖子所称新增的 `susfs_ksu_bbg.xml` 不一致。不能据此确认命令可运行或清单文件实际存在。帖子说明不填写 `-m` 时默认使用 `default.xml`。若要支持其他管理器，作者描述的做法是在 common 仓库新建分支，再修改主线仓库对应分支；具体分支名和文件内容未提供。

公开仓库链接：<https://github.com/LIghtJUNction/kernel_manifest>

## 帖子中的构建与版本记录

- 一篇构建帖称使用 OKI 内核、最新版 SUSFS、最新版 KSU、BBG 关键分区保护和 `lto=full`。作者称第一版编译成功，耗时 17 分钟；快速编译约 7 分钟。该帖表示只提供原始 `image` 镜像，并提醒刷入前备份 `boot.img`。帖子中的“编译成功”不证明设备已成功启动或长期稳定。
- 作者称不打算使用 Action 自动构建，因为其个人经历中未能通过 Action 编译 `lto=full`，并推测可能只能编译 thin LTO。此为个人经验和推测，不是 CI 能力的普遍结论。
- 后续更新记录写明 `android12-5.10`、SUSFS v2、BBG、KSU，并称合并上游后 sublevel 更新到 246，其余不变。
- v2 帖称配置为 SUSFS v2、BBG、官方版 KSU、`lto=full`，适用机型为 Ace 2。作者自述日用发热较低，但没有提供可复核的测量数据。非正式版不提供 AK3 包；帖子要求从压缩包手动提取内核并用 `magiskboot` 的 `unpack` 功能处理，资源有效期标为 1 天。原帖未提供可用下载地址，因此无法据此取得压缩包。

这些记录只适用于对应帖子中的历史构建。不要将历史 SUSFS release 与 SUSFS v2 的兼容描述外推为当前所有版本的结论。

## SUSFS for KernelSU 模块问题记录

一篇帖子针对当时的 `SUSFS-FOR-KERNELSU` 模块 v2.0 提问并记录了作者的答复：当时的最新 release 不支持 SUSFS v2；若内核使用 SUSFS v2，作者建议下载其所指的更新构建 CI。原帖提供的 [特定历史 CI 运行](https://github.com/sidex15/susfs4ksu-module/actions/runs/19565698450) 按实际 href 保留；运行状态及制品是否仍保留未核实

同帖还记录了一个 WebUI 问题：模块状态显示为绿色但无法进入 WebUI。作者提出两种临时处理方法：

- 在 `/data/adb/ksu/susfs4ksu/logs/` 放置名为 `susfs_active` 的文件。帖子称此方法为一次性处理，并怀疑是模块 bug。
- 在 `/data/adb/modules/susfs4ksu/webroot/assets/` 下搜索 JS 文件中的 `susfs_nos_dialog`；未找到时换其他 JS 文件继续查找，并将相关的 `false` 改为 `true`。帖子称可等待模块后续更新修复。

以上是特定版本与场景的历史排障记录，不保证适用于当前模块版本或其他设备环境。
## 本轮回读仓库后可明确的修正

匿名读取原帖链接所指仓库目录和两份 XML 后，确认目录含 `default.xml` 与 `ksu_susfs_bbg.xml`；拼为 `kernel_mainfest` 的仓库入口本次返回 404。因此将旧帖损坏的地址与拼写修正后，初始化示例为：

```sh
# 默认清单；用于新建的空工作目录
repo init --partial-clone -u https://github.com/LIghtJUNction/kernel_manifest
```

若要选修改版，使用另一空目录，或明确这是要切换清单的现有工作区，再用：

```sh
repo init -u https://github.com/LIghtJUNction/kernel_manifest -m ksu_susfs_bbg.xml
```

这两条是整理后的修正版，区别于上面的原文损坏命令；没有在本库执行初始化、同步或编译。仅文件名及清单内容已回读，不能推定所有远端依赖均可拉取。

两份清单将 `kernel_platform/common` 指向 `android_kernel_common_oneplus_sm8475`：默认清单使用 `oneplus/sm8475_v_15.0.0_ace_2`，修改版使用 `dev`。其余设备内核与模块/设备树项目仍使用前一分支，而部分工具来自 CodeLinaro 的固定 revision。不能把“换 common 分支”理解为所有项目同步切换，也不能由分支名称推导出完整 ROM 适配范围。

旧帖的 `android12-5.10` 是内核标签，不证明整机运行 Android 12；当前清单引用中含 `v_15.0.0` 也不等于本库已验证任何 Android 15 固件。

- [本次读取的默认清单](https://github.com/LIghtJUNction/kernel_manifest/blob/oneplus/sm8475/default.xml)
- [本次读取的修改版清单](https://github.com/LIghtJUNction/kernel_manifest/blob/oneplus/sm8475/ksu_susfs_bbg.xml)

来源：[原帖 68841453](https://www.coolapk.com/feed/68841453)、[原帖 68881882](https://www.coolapk.com/feed/68881882)、[原帖 68877414](https://www.coolapk.com/feed/68877414)、[原帖 68894976](https://www.coolapk.com/feed/68894976)、[原帖 68905721](https://www.coolapk.com/feed/68905721)、[原帖 68880661](https://www.coolapk.com/feed/68880661)。所有原帖配图未读，未执行设备操作。
