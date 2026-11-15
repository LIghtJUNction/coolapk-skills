# 免解 BL 临时 Root（GhostLock / CVE-2026-64560）与工程 abl 案例

本参考整理酷安社区中基于内核漏洞（如 GhostLock `CVE-2026-43499`、`CVE-2026-98260`、`CVE-2026-64560`）实现免解锁 Bootloader 临时提权的机制说明、实测踩坑、机型适配汇总，以及利用临时 Root 刷写工程 `abl` 分区的操作边界与高风险警告。所有内容均来自原帖正文；外部网盘、APK 与代码仓库未在本库下载或实机复现。

---

## 1. 一加 15（PLK110）CVE-2026-64560 临时 Root 机制与实测

- **来源**: https://www.coolapk.com/feed/75043417
- **目标环境**: 一加 15（`PLK110`）、`ColorOS 17.0.0.105`、内核 `6.12.69`
- **利用原理简述**:
  - `CVE-2026-64560` 为 `posix_cpu_timer_del` 在非 leader 线程 `exec()` 竞态下产生的 UAF（Use-After-Free）。
  - 利用链：堆喷洒 → 跨缓存回收 → 通过 `uhid + configfs` 搭建内核读写桥 → 修改自身 `cred` 与 SELinux 上下文取得 `uid=0` → 在同一 root 窗口内执行 `ksud late-load --kmi android16-6.12 --allow-shell` 载入 KernelSU。
  - 来源与致谢（原帖提供）：`github.com/feiguang-ai/plz110-6.12.69-cve-2026-64560-ksu`（Apache-2.0）、`github.com/tiann/KernelSU`（GPL-3.0）、`github.com/RikkaApps/Shizuku`（Apache-2.0）。
- **实测特征与撤销**:
  - **概率性与耗时**: 命中具有概率性，快则十几分钟、慢则一小时以上；失败轮会触发内核 panic 自动重启（App 自动续跑），作者实测撞了 3 次 panic 重启才成功，其中一轮曾进入近两万次重试循环。
  - **Per-boot 失效与清理**: 不写任何分区，重启一次即失效（KernelSU LKM 与 `/system/bin/su` 均消失）；磁盘残留 `/data/adb/ksu`、`/data/local/tmp/fgroot/` 可手动删除。

---

## 2. GhostLock（CVE-2026-43499 / CVE-2026-98260）与白鹿工具箱汇总

- **来源**:
  - https://www.coolapk.com/feed/75048655（白鹿提权助手 1.0.3）
  - https://www.coolapk.com/feed/75030894（白鹿工具箱支持机型说明）
  - https://www.coolapk.com/feed/75021684（GhostLock CVE-2026-43499 适配内核汇总）
  - https://www.coolapk.com/feed/75020432（红米 K70 Pro 配合 Shizuku + GhostLock 实测）
  - https://www.coolapk.com/feed/75015258（免解 BL 临时提权支持清单）
- **共同机制与前置条件**:
  - 依赖内核提权路径绕开 Bootloader 解锁，需先确认设备内核版本（如 `5.10` / `5.15` / `6.1` / `6.6` / `6.12`）及安全补丁级别（SPL）；系统升级修补漏洞后即失效。
  - **Shizuku 协同流程**（以 `75020432` 红米 K70 Pro 实测为例）：安装最新版 GhostLock、Shizuku、KernelSU → 开启无线调试配对并启动 Shizuku → 授予 GhostLock 应用列表访问与 Shizuku 权限 → 开启“通过 Shizuku 执行”并运行 → 成功后自动唤起 KernelSU。
  - 保持临时 Root 需尽量使用**软重启**，完整重启后 Root 权限自动失效。
- **原帖声称覆盖范围（未在本库逐机验证）**:
  - `75048655` 提及 `SPL 2026-11-8` 下白鹿提权助手 1.0.3 针对 iQOO/vivo（含 `5.10`、`5.15`、`6.1`、`6.6`、`6.12` 系）、Redmi K80 系列、Xiaomi 15 系列及荣耀（9 月 1 日补丁）等。
  - `75021684` 与 `75015258` 亦列出 iQOO、vivo、小米/红米、一加/真我/OPPO、荣耀、三星等大量机型，但不同子版本与补丁日期的成功率存在差异，实际需以目标设备当前内核构建为准。

---

## 3. 澎湃 OS 临时 Root 刷写工程 `abl` 分区的操作边界与警告

- **来源**: https://www.coolapk.com/feed/75044559
- **核心前提与数据线排查**:
  - 临时 Root 使用本地镜像时，必须严格选择与当前澎湃 OS 完全一致的 `boot` 文件（例如澎湃 `3.0.9` 对应 `OS3.0.9`）。
  - 若电脑未检测到 Fastboot/ADB 设备：台式机应直连主板后置 USB 口（避免机箱前置或供电不足的网吧桌面扩展坞），笔记本需插电源适配器，优先使用原装或满 PIN Type-C 线；必要时关机清理充电口灰尘。
- **工程 `abl` 刷写流程与风险**:
  - 在 KernelSU 超级用户中为 `shell` 授予 Root 权限后，通过紫罗兰工具箱命令行刷入对应机型工程包（原帖强调工程包总大小在 `1GB` 以下）中的 `abl.elf`：
    ```sh
    adb shell su -c dd if=/sdcard/abl.elf of=/dev/block/by-name/abl_a
    adb shell su -c dd if=/sdcard/abl.elf of=/dev/block/by-name/abl_b
    ```
  - 若提示 `No such file or directory`，说明 `/sdcard/abl.elf` 未成功推送。
- **原帖高风险警告**:
  - 免解 BL 临时 Root 状态下**严禁删温控模块或改动 `system`、`boot` 等系统分区**，否则可能触发黑屏或 UFS 闪存保护锁死，只能送小米售后刷回。
  - 重启至 Fastboot 后不可误触非工程包或错误操作，否则可能触发 `The system has been destroyed` 红色感叹号。
  - 9 张配图及紫罗兰工具箱具体界面步骤未读取。
