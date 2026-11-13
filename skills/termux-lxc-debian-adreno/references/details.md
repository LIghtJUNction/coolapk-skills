# Termux、Debian 12 LXC 与 Adreno 透传笔记

原帖环境为 K40p（原帖写法，未给型号代码）/ 骁龙 888、已刷支持 LXC 的内核。它是内核刷入后的配置笔记，不包含内核下载、刷入或 LXC/Termux 工具安装流程。GPU 配置仅声称对部分 Adreno 设备有效，没有跑分或逐设备成功清单。

## 容器创建

以下在 Android 的 Termux 宿主侧执行，要求已有可用的 `sudo` 与 LXC：

```sh
export CONTAINER_NAME="debian-bookworm"
sudo lxc-create -t download -n ${CONTAINER_NAME} -- --no-validate -d debian -r bookworm -a arm64
sudo vim "$PREFIX/var/lib/lxc/${CONTAINER_NAME}/config"
```

- 容器名 `debian-bookworm`；发行版 `debian`，代号 `bookworm`，架构 `arm64`。
- 原帖下载命令带 `--no-validate`，此处保留该参数作为原帖记录，不把它解释为已验证镜像。
- 原帖先用 `sed` 删除配置中从 `Network configuration` 到文件尾的内容；这可能连带删除后续自定义项，不能理解为只删除一个配置键。
- 原帖另一段临时目录示例使用容器名 `debian`，与这里不同；套用时统一为实际容器名。

## 基础配置与网络含义

写入容器配置文件的行，不是 shell 命令：

```ini
lxc.net.0.type = none
lxc.hook.version = 1
lxc.cgroup.devices.allow = a
lxc.mount.auto = cgroup:mixed sys:mixed proc:mixed
lxc.environment = TERM="xterm-256color"
```

原帖的 `# lxc.cgroup.memory.limit_in_bytes = 2G` 是注释，不能当作已经启用 2G 内存限制。

`lxc.net.0.type = none` 表示共享宿主网络命名空间，**不是关闭网络**。[LXC 官方配置手册](https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html) 区分 `none` 与仅创建回环接口的 `empty`。原帖未给出独立网桥或 veth 配置。

## Termux:X11 与共享临时目录

原帖尝试只共享 `.X11-unix` 后认为不够，改为共享整个 Termux 临时目录。启动前钩子如下，属于 LXC 配置：

```ini
lxc.hook.pre-start = bash -c "mkdir -p '${LXC_ROOTFS_PATH}/tmp' 2>/dev/null; umount '${LXC_ROOTFS_PATH}/tmp' 2>/dev/null; mount --bind '/data/data/com.termux/files/usr/tmp' '${LXC_ROOTFS_PATH}/tmp'; true;"
```

原帖另有一行**注释掉的** `termux-x11 :2 -dpi 96` 启动钩子；并未证明默认自动启动了 X11 服务。容器内 `/etc/environment` 设置 `DISPLAY=:2` 后需重新登录。

## FUSE 与 GPU 设备

FUSE 配置：

```ini
lxc.cgroup.devices.allow = c 10:229 rwm
lxc.mount.entry = /dev/fuse dev/fuse none bind,optional,create=file
```

原帖给出的 GPU、DRM 和 ION 设备号是示例，需要换成实际设备的主次设备号：

| 节点 | 原帖示例 |
|---|---|
| `/dev/kgsl-3d0` | 字符设备 `506:0` |
| `/dev/dri/renderD128` | 字符设备 `226:128` |
| `/dev/dri/card0` | 字符设备 `226:0` |
| `/dev/ion` | 字符设备 `10:127` |

```ini
lxc.cgroup.devices.allow = c 506:0 rwm
lxc.cgroup.devices.allow = c 226:128 rwm
lxc.cgroup.devices.allow = c 226:0 rwm
lxc.cgroup.devices.allow = c 10:127 rwm
lxc.mount.entry = /dev/kgsl-3d0 dev/kgsl-3d0 none bind,optional,create=file
lxc.mount.entry = /dev/dri dev/dri none bind,optional,create=dir
lxc.mount.entry = /dev/ion dev/ion none bind,optional,create=file
```

原帖先设置 `lxc.cgroup.devices.allow = a`；随后追加设备规则并不意味着已经收窄到只允许这些设备。设备节点可见也不等于 Mesa 驱动和硬件加速已验证成功。

## 启动钩子的任务与待核对项

原帖的多条 `pre-start` 钩子负责：

- 首次为 root、debian 设置临时密码 `password`，再注释该密码钩子；这是公开示例密码，不是需要另取的私有凭据。原帖没有给出创建 debian 用户的步骤。
- 挂载 cgroup v1 控制器，包括 blkio、cpu、cpuacct、cpuset、devices、freezer、memory、pids 和 systemd；没有给出 cgroup v2 等价方案。
- 修改 DNS、绑定 rootfs 并重挂载为 suid，包装 `udevadm`，生成 tmpfiles 和启动脚本，处理 binderfs。
- 停止或销毁后用 `umount -Rl` 卸载绑定的 rootfs。

这里不重发布整段混合钩子作为可直接运行脚本。其中有两处需要明确区分：

1. 原帖写 `security.nesting = true`，本文未验证它可作为此 LXC 版本的配置键；不把它加入上面的基础配置。嵌套相关选项应按所用 LXC 的手册核对。
2. 原帖循环把 `/dev/loop${i}` 的次设备号写成 `i*8`。[Linux 内核设备分配表](https://origin.kernel.org/doc/html/v6.13/admin-guide/devices.html) 列出 loop0 为 `7:0`、loop1 为 `7:1`，与该公式不符；不能将原帖循环当作通用设备创建规则。

DNS 示例列表为 `223.5.5.5`、`119.29.29.29`、`8.8.8.8`、`8.8.4.4`、`2400:3200::1`、`2400:3200:baba::1`、`2402:4e00::`、`2001:4860:4860::8888`、`2001:4860:4860::8844`。原帖没有测定它们在各网络中的可达性。

## 容器内初始化

宿主侧启动容器：

```sh
sudo lxc-start -F -n ${CONTAINER_NAME}
```

以下在容器内操作，locale 设置后按原帖重新登录；时区只是原帖示例：

```sh
sudo dpkg-reconfigure locales
sudo timedatectl set-timezone Asia/Shanghai
sudo vim /etc/apt/sources.list
sudo apt update && sudo apt upgrade -y
sudo apt install neofetch wget curl nano dnsutils dbus-x11 task-kde-desktop task-chinese-s-kde-desktop glmark2-x11 fonts-wqy-zenhei fcitx5 fcitx5-pinyin chromium chromium-l10n -y
```

原帖还记录了证书引导的临时绕过：在 `/etc/apt/apt.conf.d/99insecure` 关闭 HTTPS 对端验证，安装 `ca-certificates` 后删除该文件。它降低证书验证保障，不是本文的默认安装步骤。

```sh
echo 'Acquire { https::Verify-Peer "false"; }' | sudo tee /etc/apt/apt.conf.d/99insecure
sudo apt update && sudo apt install ca-certificates -y
sudo rm /etc/apt/apt.conf.d/99insecure
```

原帖未提供 Mesa 安装、GPU 驱动选择、渲染器检查结果或 X11 硬件加速验收记录。不能从设备透传配置推导所有 Adreno 手机都可加速。

