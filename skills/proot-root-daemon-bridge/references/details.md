# 功能、参数与限制

## DSH-Go 在 proot 中无法直接访问完整文件系统

文章称，dsh-go 运行在 proot 中。其挂载表仅包含 /dev、/proc、/sys、/system、/apex、/sdcard，因此没有 /data；同时进程处于 SELinux 的 untrusted_app 域，无法仅凭 root 授权切换 SELinux 域。文章区分了 dsh-go 用于手机操控的 root 能力与 proot 文件系统访问能力。

### 范围与条件

- 文章报告的环境为 realme GT7 Pro、ColorOS 17（112 版本）、KernelSU。
- 文章提及 Android 17 上 seccomp 会使直接启动 dsh 引擎的尝试触发 SIGSYS；未说明其他系统版本表现。

### 现象与排查

- 在 proot 中执行 ls /data 仍看不到目录，访问 /sys 可能出现 Permission denied；文章将其归因于 proot 挂载范围和 SELinux 域限制。
- 不要在 proot 中反复尝试 su；文章称直接以 root 启动 dsh 引擎在 Android 17 上会因 seccomp 触发 SIGSYS。

## 通过开机 root 守护脚本向 proot 提供命令执行通道

文章给出的方案是在设备启动时由 root 守护脚本轮询共享存储目录：读取 /sdcard/root-requests 中的命令并执行，将输出写入 /sdcard/root-results。proot 端通过客户端投递命令、读取结果。作者称脚本放在 /data/adb/service.d，KernelSU、Magisk、APatch 通用；方案另加 token 校验，降低其他进程向共享目录投放脚本即可执行命令的风险。配图中的脚本完整内容未提供，因此无法据此复现守护脚本或客户端实现。

### 操作与检查

- 确保手机已 root，并能取得一次真正的 root shell；文章列举 Termux 中执行 su、MT 管理器或电脑 adb shell su。
- 在 root shell 中将守护脚本写入 /data/adb/service.d/rootd.sh 并启动一次，同时把 token 放到 /sdcard。
- 在 proot 中将 token 收到 ~/.dsh/private/rootd.token，并安装客户端 tools/root-run.py。
- 在 proot 中运行 python3 tools/root-run.py 'id'，检查 uid=0(root)；原帖 KernelSU 环境还显示 context=u:r:ksu:s0，该域名不作为其他 Root 框架的统一要求。

### 参数与路径

- 请求目录：/sdcard/root-requests；守护脚本从中读取命令。
- 结果目录：/sdcard/root-results；守护脚本将命令输出写入此处。
- 守护脚本路径：/data/adb/service.d/rootd.sh。
- token 文件：设备共享存储中的 /sdcard/.rootd-token；proot 客户端侧为 ~/.dsh/private/rootd.token。
- 客户端路径：tools/root-run.py。
- 文章称守护每秒扫描请求目录；没有提供轮询参数或可配置方式。

### 范围与条件

- 作者称 /data/adb/service.d 的开机守护脚本方式适用于 KernelSU、Magisk、APatch。
- 必需条件：手机已 root，且能获取一次真实 root shell；文章称该 root shell 只在安装守护时需要。
- 作者测试设备：realme GT7 Pro，ColorOS 17（112 版本），KernelSU。
- 不需要给 dsh-go 授予 root；文章称授予后对该 proot 场景无用。

### 现象与排查

- 请求文件首行包含明文 token；文章提醒不要把 /sdcard/root-requests 当作公共区，并在安装后删除 /sdcard/.rootd-token。
- 守护脚本处于受限 mount namespace 时，/data/user/0 只能列出两个目录。文章称读取其他 App 数据需经 /proc/1/root/... 或 /proc/<pid>/root/...；没有给出具体命令、权限条件或 pid 获取方式。
- 配图提供了完整命令，但当前输入未包含图片内容，守护脚本和客户端的安装、启动、权限设置及恢复步骤无法核实。

### 已有客户端后的验证

只有原帖所述客户端及守护已经安装时才使用；这不是安装脚本。

```sh
python3 tools/root-run.py 'id'
```

