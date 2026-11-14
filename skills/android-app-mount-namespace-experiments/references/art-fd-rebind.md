# art-fd-rebind：Native Detector 挂载检测实验记录

## 项目与适用范围

App Namespace Experiments（模块 ID：`art_fd_rebind`）是按目标应用启用的实验性 Zygisk 模块，默认不选择任何应用，需按 README 配置目标。项目借鉴 NoHello / AOSP 的已有实现并按 Apache-2.0 开源。记录中的实测设备为一加 PHK110、Android 16，检测器为 Native Detector 7.7.0；结果只适用于这次设备与版本组合，不能外推为全机型兼容。

- 源码仓库：https://github.com/LIghtJUNction/art-fd-rebind
- v0.2.4-experimental Release：https://github.com/LIghtJUNction/art-fd-rebind/releases/tag/v0.2.4-experimental
- 前序动态：https://www.coolapk.com/feed/75049078

## Oplus overlay 隔离

模块在 Zygisk 的 `preAppSpecialize` 阶段，对选中的应用执行 `unshare(CLONE_NEWNS)`，建立该应用自己的挂载命名空间；将其设为 `MS_PRIVATE | MS_REC`，再用 `MNT_DETACH` 移除以下目录中的 overlay 覆盖层：

- `/system_ext/framework/oplusex/`
- `/system_ext/lib64/oplusex/`

操作只修改目标应用的私有挂载视图，不改全局挂载。早期实验观察到目标命名空间中的 9 个覆盖层变为 0，而 init 视图中原有的 9 个仍在。

前序版本的同次开机冷启动对照中，关闭 Oplus 规则时有 2 条 Suspicious Mount，开启时有 1 条；两种情况下都没有 Magic Mount。该对照只支持“这次实测减少 1 条可疑挂载警告”，不能据此认为 Oplus 规则解决了 Magic Mount。剩余 `/system_ext/lib64/oplusex/com.oplus.NetworkAssistSys` 曾仍被报告。

后续排查发现，除了主进程和 AppZ 父进程，Native Detector 使用的普通 isolated 服务也有自己的挂载视图。只检查主进程会漏掉这组进程。v0.2.1 按 Android 用户、包名及私有进程前缀选择目标，不把可能反复分配的 isolated UID 当作永久身份；在选中进程自己的挂载视图处理 Oplus overlay，保持 init 原挂载不变。同次开机的记录显示，关闭规则时报可疑挂载，开启后该项消失。

## ART 与 framework JAR 文件句柄处理

### 早期重新打开方案的局限

v0.1 的思路是在当前 App 的挂载空间重新打开 ART 文件。独立自测通过，但 Native Detector 仍报告 Magic：重新打开后拿到的仍是当前空间中的句柄，未改变相关挂载来源。前序 v0.2 记录提到重新打开继承的旧 FD 能观察到 FD 变化，但当时不能消除 Magic Mount，所以该功能曾关闭。安装成功或单项自测通过不等于检测器通过。

### 检测行为与后续方案

作者对当时 APK 相关分支的观察是：检测器扫描 FD 0—19，读取相关文件的 `fdinfo`；`mnt_id` 数字字符串长度达到 4 位即可命中，`core-oj` 有豁免。此处不是 1024 的门槛；较大的 `mnt_id` 本身也不能证明存在恶意挂载。

后续方案从 init 的视图只读打开同一个公开 JAR，再通过 `SCM_RIGHTS` 将文件句柄交给选中的子进程。目标是使用真实来源的文件句柄，文件内容不变，也不伪造 `fdinfo` 或直接改检测结果。

- v0.2.2 增加 `O_NOFOLLOW` 后，与 Android 原句柄的 flags 不一致，被一致性检查拒绝，替换失败。
- v0.2.3 按原 flags 打开，`core-libart.jar` 处理后，检测器转而报告 `okhttp.jar`，`mnt_id` 为 1280。后续判断这是检测器只展示第一个命中：前一项消失后才显示下一项。
- v0.2.4 将处理范围扩展到该设备继承的 12 个文件：5 个 ART JAR 与 7 个 system framework JAR。使用固定公开路径白名单，不接受任意文件路径；逐个核对设备号和 inode，保留原 FD 编号、flags 与读取位置；同一文件存在多个旧句柄时拒绝处理，校验失败则回滚。

发布前的测试包括 12 个文件分别带与不带 `O_NOFOLLOW` 的情况，以及同时持有 12 个源句柄、混合 flags 的真实 IPC 批量测试。原帖没有列出这 12 个文件的完整路径清单或具体执行命令，因此此处不补写。

## v0.2.4 单机对照结果

同一次开机、Oplus 规则保持开启并冷启动应用：关闭 ART 修复时，`core-libart.jar` 的 `mnt_id` 1277 报 Magic；恢复 ART 修复后，检测环境显示正常。主进程、AppZ 父进程和普通 isolated 服务均记录到 12 个替换。实验最终保持 ART 修复开启，并收起调试日志。

该结果是原帖报告的单机实测，不是独立复现。前序动态报告当时并未全部通过 Native Detector；后续动态报告在上述指定组合中显示环境正常，且明确说明 Experimental / Custom ROM 检测未开启。两次记录的设置与处理范围不同，应按各自版本和实验条件理解。

## 版本与资源边界

前序记录时，Oplus 功能已在 `main` 的 v0.2 源码中，本地构建的 v0.2 ZIP 已装机实测，但尚无 v0.2 Release；当时公开 Release 只有不含新 Oplus 功能的 `v0.1.1-experimental`。后续记录给出了 `v0.2.4-experimental` Release 地址。下载地址是公开 Release 页面；原帖未提供提取码，也未报告下载校验结果。

原帖提到两张同 boot 开关对照图，但本输入未包含图片内容；评论也未提供。未据此补充图片或评论中的信息。

来源：[后续实验记录](https://www.coolapk.com/feed/75051340)、[前序实验记录](https://www.coolapk.com/feed/75049078)。两篇API记录均无内嵌图片；来源提及外链中的2张同boot对照图，未读取；文中结果均为来源报告，本库未实机复现。
