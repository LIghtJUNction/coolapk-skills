# OpenClaw Pocket 离线模块、Droidspaces LKM（`.ko`）与内核提权分析

本参考整理酷安社区中关于手机本地离线运行 OpenClaw（`openclaw-pocket`）、通过可加载内核模块（`.ko`）为原厂 GKI 内核补齐 Docker/Droidspaces 命名空间与网络能力、真我 GT8 Pro（`sm8850`）编译刷机验证，以及 vivo Y500（`6.1.145`）POSIX CPU timer 竞态提权分析。

---

## 1. `openclaw-pocket`：离线 Ubuntu 24.04 + OpenClaw Magisk/KSU 模块

- **来源**: https://www.coolapk.com/feed/74806217
- **结构与边界**:
  - 仅支持 `arm64`，兼容 Magisk / KernelSU / APatch；`openclaw-pocket-offline-1.3.0.zip` 约 `169MB`，内置 Ubuntu 24.04 `arm64` chroot、Node `26` 与 OpenClaw `2026.10.9`（安装后占约 `1GB`，Gateway 常驻内存约 `0.9–1.2GB`）。
  - 针对 Android 内核缺少 `/proc/self/ns/pid` 自动打兼容补丁；刷入重启后访问 `127.0.0.1:18789`（token 在 `/data/openclaw/token`），用 `openclawctl` 管理；卸载自动清理 `/data/openclaw`。

---

## 2. 一加 Ace 6 至尊版（`PMB110`，`6.12.69`）挂 `.ko` 免刷内核跑 Docker

- **来源**: https://www.coolapk.com/feed/74676851
- **开源仓库**: `https://github.com/ai-luo2006/droidspaces-6.12-pmb110/`
- **补充的 6 个内核模块（手动加载，默认不开机自启）**:
  - `droid_lkm.ko`（PID/IPC/mqueue 命名空间）
  - `droid_lkm_misc.ko`（`devtmpfs` 设备节点）
  - `droid_lkm_pids.ko`（使 `--pids-limit` 生效）
  - `macvlan.ko` / `ip6table_nat.ko`（容器独立 IP 与 IPv6 NAT）
- **已知踩坑**: 若触发「转换容器属主」将 rootfs 文件属主改为 `200000`，因内核无 user namespace、进程仍为 `0`，容器内会报 `su: cannot set groups`。

---

## 3. 真我 GT8 Pro（`sm8850`，`6.12.23`）Droidspaces 33/33 与 USB-Arsenal 12/12

- **来源**: https://www.coolapk.com/feed/74659206
- **要点**: 基于 `https://github.com/cctv18/oppo_oplus_realme_sm8850` Fork 后通过 GitHub Actions 插入构建步骤产出 `AnyKernel3_none__6.12_mizzhuo.zip`（`6.12.23` 内核），在真我 GT8 Pro 上实测通过 Droidspaces `33/33` 与 USB-Arsenal `12/12`（具体改法与补丁详见原帖后续部分，图片未读）。

---

## 4. KernelSU LKM vs GKI 模式与 SUSFS / BBRv3 / Droidspaces 区别

- **来源**: https://www.coolapk.com/feed/74665047
- **模式差异**:
  - **LKM 模式**: 刷写 `init_boot.img`（原文写 `ini_boot.img`），不改官方内核，稳定性高；其中 SukiSU-Ultra 内置 SUSFS 与 `susfs4ksu` 模块，Bakasu / KernelSU-Next 需按对应配置搭配。
  - **GKI 模式（BUILT-IN）**: 刷写 `boot.img`，将 SUSFS、BBR/BBRv3、BBG、TTL、`IP_SET & IPv6 NAT`、Unicode Fix、Droidspaces、NTSync 等编译进内核。

---

## 5. vivo Y500（`PD2506` / `6.1.145`）POSIX CPU timer 竞态提权分析

- **来源**: https://www.coolapk.com/feed/74774656
- **目标环境**: vivo `PD2506` / `V2506A`（Y500），`PD2506B/D_A_16.1.15.0.W10`，内核 `6.1.145-android14-11-gf506252fa70d`（SELinux Enforcing）。
- **利用链概要**: 非 leader 线程 `execve` 释放 `k_itimer` 时 `posix_cputimers base` 保留陈旧 `rb_node`；通过 `order-1` 抢占 `posix_timers_cache` slab 页并伪造 `k_itimer`/`cpu_timer`/`cred`，前置包括零权限 KASLR slide、`vr.ko` 去探针（清 `__tracepoint_sys_exit->funcs`）及清 `selinux_state-7` enforcing，最终 `ksud late-load` 进入越狱模式。
