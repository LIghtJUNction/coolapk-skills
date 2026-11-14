# fas-rs v1.0.0：历史配置与 Termux 构建

> 本文记录 v1.0.0 帖子中的历史配置与构建说明，不应据此推断最新版仍使用相同字段或默认值。

## 项目与配置

fas-rs 是运行于 Android 用户态的帧感知调度（FAS）实现。原帖称它通过 inline hook 获取渲染时间；这种注入方式可能被反作弊系统误判。项目仓库：[shadow3aaa/fas-rs](https://github.com/shadow3aaa/fas-rs)，版本下载：[GitHub Releases](https://github.com/shadow3aaa/fas-rs/releases)。

配置文件路径为 `/sdcard/Android/fas-rs/games.toml`。

### `[config]` 字段

- `keep_std`（布尔值，原帖示例默认值为 `true`）：`true` 时，合并配置会保留标准配置的 profile，并保留本地配置的应用列表；其他方面与 `false` 相同。`false` 时使用下述默认合并行为。
- `ignore_little`（布尔值，原帖示例默认值为 `false`）：`true` 时，设备至少有 3 个集群才仅控制非小核集群；`false` 时始终控制所有集群。

### `[game_list]` 与自动帧率识别

每个键是应用包名，值是整数目标 FPS，或字符串 `"auto"`。整数表示固定的 FAS 目标帧率；`"auto"` 在运行时判断目标帧率，**仅支持 30、45、48、60、90、120、144 FPS**。游戏不属于这些档位时应手动指定整数。

### 模式及阈值

模式为 `powersave`、`balance`、`performance`、`fast`。原帖称当时没有官方模式切换管理器，可接入 Scene 配置接口；不使用 Scene 时默认使用 `balance` 配置。了解 Linux 编程的用户可向 `/dev/fas_rs/mode` 写入上述四个模式之一来切换，也可读取该节点查询当前模式。

- `tolerant_frame_jank`：可接受的掉帧数；当前帧相对掉帧数超过此值时升频。
- `tolerant_frame_limit`：可接受的不掉帧数；当前帧相对掉帧数不超过此值时降频。

以下是原帖提供的 TOML 实例，包含其应用列表及各模式参数：

```toml
[config]
ignore_little = false
keep_std = true

[game_list]
"com.hypergryph.arknights" = "auto"
"com.miHoYo.Yuanshen" = "auto"
"com.miHoYo.hkrpg" = "auto"
"com.mojang.minecraftpe" = "auto"
"com.netease.party" = "auto"
"com.netease.wotb" = "auto"
"com.netease.x19" = "auto"
"com.pixeltoys.freeblade" = "auto"
"com.prpr.musedash.TapTap" = "auto"
"com.shangyoo.neon" = "auto"
"com.tencent.tmgp.pubgmhd" = "auto"
"com.tencent.tmgp.sgame" = "auto"

[powersave]
tolerant_frame_limit = 0.05
tolerant_frame_jank = 1.0

[balance]
tolerant_frame_limit = 0.01
tolerant_frame_jank = 0.5

[performance]
tolerant_frame_limit = 0.008
tolerant_frame_jank = 0.1

[fast]
tolerant_frame_limit = 0.003
tolerant_frame_jank = 0.05
```

## 配置合并规则

原帖描述的标准配置与本地配置合并规则：

1. 删除本地配置中标准配置不存在的配置。
2. 插入本地配置缺少、但标准配置存在的配置。
3. 对标准配置和本地配置都存在的配置予以保留。

实现使用自动序列化和反序列化，因此注释等非序列化必需信息无法保存。模块每次安装时会自动执行一次合并。原帖给出的手动命令为：

```bash
fas-rs --merge --local-profile /path/to/local/config --std-profile /path/to/std/config
```

## 原帖所列 Termux 构建方式

在可写父目录克隆仓库，再进入克隆所得项目根目录构建；`cd fas-rs` 是整理时补足的目录切换：

```bash
git clone https://github.com/shadow3aaa/fas-rs
cd fas-rs
apt install rust zip ndk* clang binutils-is-llvm xmake
xmake
```

原帖列出的依赖是历史 Termux 命令；当前 Termux 的包名或可用性不保证相同。

Scene入口原始href末尾多了一个右括号；去掉该结束括号后的规范包名页为[Scene](https://www.coolapk.com/apk/com.omarea.vtools)。这属于机械整理，不表示已核实该页当前版本。

来源：[v1.0.0原帖](https://www.coolapk.com/feed/50563778)。
