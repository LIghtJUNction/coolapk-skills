# SUSFS 内核模块（.ko）、PowerStats 调度功耗统计与 Root 变砖排查

本参考整理酷安社区中关于 Lenovo Legion Y700 五代免解 BL Root 后使用 SUSFS 可加载内核模块（`.ko`）与 HMA 隐藏环境、PowerStats 调度/整机耗电评估模块，以及 Root 变砖按软砖/硬砖分层的应急排查指南。图片与评论未读，未在本库实机验证。

---

## 1. Y700 五代免解 BL Root + SUSFS（`.ko`）与 HMA 环境隐藏

- **来源**: https://www.coolapk.com/feed/75036431
- **问题定位（两个暴露面）**:
  1. **内核痕迹**: 实测 Y700 五代上 `/proc/modules` 全局可读（普通应用无需 Root 即可 `grep` 出 `kernelsu`），`kallsyms` 中亦存在 3000 余行 `ksu` 符号；反作弊/风控可同时扫描内核痕迹与已安装应用列表。
  2. **用户态包名列表**: KSU 管理器、HMA、检测器与模块本身若被枚举，同样触发环境异常。
- **两层封堵路线（原帖结构）**:
  - **内核层（SUSFS `.ko` 模块）**: 相比传统重编译内核刷 `boot`（易因版本不一致变砖或影响引导锁状态），原帖采用可加载内核模块（`.ko`）形式的 `susfs4ksu-for-lkm`（通过 `insmod` 加载）配合 `susfs4ksu-module` 进行内核级隐藏。
  - **用户态（HMA-OSS / Zygisk）**: 结合 `KernelSU` / `KernelSU-Next`、`ZygiskNext` / `ReZygisk`、`HMA-OSS`（隐藏应用列表）、`LSPosed`、`TrickyStore`、`PlayIntegrityFork` 等开源组件进行应用列表隔离与环境管理。
- **证据边界**: 原帖正文前半部分为教程结构与开源仓库索引（含 `EHoRY`、`LTBox`、`gkd` 等），具体 `.ko` 针对目标内核 KMI 的编译匹配、配置清单与“一键游戏态”脚本详情及配图未在正文完整展开或未读取。

---

## 2. PowerStats：调度轮询频率与整机耗电评估模块

- **来源**: https://www.coolapk.com/feed/75046233
- **定位与注意事项**:
  - 用于评估第三方调度进程的活跃频率、CPU 占用与整机功耗趋势。模块本身仅读取 `/proc` 与电池节点，**不修改任何调度参数或系统文件**；模块自身也有采样开销，可通过调大采样间隔降低开销。
- **轮询判定与 CPU 口径**:
  - **判定阈值**: 仅当“活跃占比 + 连续性 + CPU 开销”同时达标才判为“每秒轮巡”；若进程每秒唤醒但单次仅耗时数微秒、CPU 占用低于门槛，则最高判为**低频轮询（活跃占比 ≥ 15%）**，低于 15% 为**事件驱动**。
  - **双口径 CPU 占比**: 同时输出**占整机**（进程 CPU 时间 ÷ 所有核心总时间）与**单核等效**（占整机 × 核心数），避免 8 核设备上 `0.21%` 误导判断（单核等效约 `1.7%`）。
- **整机功耗估算方式**:
  - 读取 `/sys/class/power_supply` 下电池节点的 `current_now` 与 `voltage_now` 计算瞬时功率（mW），按采样间隔积分得到 `mWh`（充电时自动跳过采样并记录跳过次数）。
  - 明确局限：该读数为**整机电流计功耗**，无法直接拆分单个调度进程的独立耗电。
- **配置与输出路径**:
  - 配置文件：`/data/adb/modules/powerstats/config.conf`（修改 `BINARY=` 为目标进程名，注意 `comm` 长度截断为 15 字符；保存后 1 秒内热生效）。
  - 实时状态与报告：`/data/adb/modules/powerstats/status.txt` 与 `/data/adb/powerstats/reports/report_年月日_时分秒.txt`；`log` 与 `status` 超 `256KB` 自动清空，重装/升级保留已有报告与配置。

---

## 3. Root 变砖分层排查与应急恢复流程

- **来源**: https://www.coolapk.com/feed/75038430
- **状态分级与优先动作**:
  1. **仍能进系统 / ADB 可连**:
     - Magisk：开机按住音量下（部分机型）跳过模块，或通过 ADB 删除异常模块。
     - KernelSU：执行 `rm /data/adb/ksud` 阻止加载模块后重启。
  2. **能进 Recovery（官方 / TWRP）**:
     - 优先清除缓存；模块问题可在 TWRP 挂载 `/data` 删除对应模块（若 `/data` 已加密且 TWRP 无法解密，强行格式化会丢失数据）。
     - 官方 Recovery 可尝试清除数据、本地升级或联网修复（如华为 eRecovery）。
  3. **能进 Fastboot / Download 模式**:
     - 仅 `boot` 镜像错误：Fastboot 重刷**同版本**官方 `boot.img`。
     - 系统分区异常：使用官方线刷工具（Mi Flash、Odin 等）刷入完整官方包，优先选“除 data 外刷写”保数据；严禁刷入低于防回滚（ARB）版本的固件，切勿混用不同机型包。
  4. **黑屏且仅识别 9008（高通）/ MTK 深度端口**:
     - 需通过 EDL / 深度下载模式刷写官方售后包（部分机型需售后授权）。
  5. **完全无任何反应**: 排除充电问题后多为硬件或底层损坏，建议送修。
