# fas-rs v3.7.0 与 v4.5.0：历史变化

这是两个不同版本的历史记录。项目仓库：[shadow3aaa/fas-rs](https://github.com/shadow3aaa/fas-rs)，公开下载页：[GitHub Releases](https://github.com/shadow3aaa/fas-rs/releases)。两帖也列出蓝奏云镜像 [v3.7 帖所列镜像](https://wwr.lanzoui.com/b019jb8mb) 与公开分享提取码 `22bw`；两篇原帖给出相同镜像地址。请按发布页确认对应版本。

## v3.7.0

- 将目标帧率降低 **0.5 FPS** 以增加稳定性，见 [commit 1e2ebf3](https://github.com/shadow3aaa/fas-rs/commit/1e2ebf3)。
- 修正温度比较方向，见 [commit 258738d](https://github.com/shadow3aaa/fas-rs/commit/258738d)。
- 更新 frame-analyzer，以及无法接收帧数据时重启、部分游戏接收帧数据等修复： [cfc2534](https://github.com/shadow3aaa/fas-rs/commit/cfc2534)、[2794f73](https://github.com/shadow3aaa/fas-rs/commit/2794f73)、[3f717e9](https://github.com/shadow3aaa/fas-rs/commit/3f717e9)。
- 其余帖中列出的代码、文档及 CI 变更可按提交查看： [ff746ff](https://github.com/shadow3aaa/fas-rs/commit/ff746ff)、[d313565](https://github.com/shadow3aaa/fas-rs/commit/d313565)、[0b25171](https://github.com/shadow3aaa/fas-rs/commit/0b25171)、[202c1dc](https://github.com/shadow3aaa/fas-rs/commit/202c1dc)、[953c2ef](https://github.com/shadow3aaa/fas-rs/commit/953c2ef)、[e677b87](https://github.com/shadow3aaa/fas-rs/commit/e677b87)、[3a33040](https://github.com/shadow3aaa/fas-rs/commit/3a33040)、[6c83c7d](https://github.com/shadow3aaa/fas-rs/commit/6c83c7d)、[1e06fd9](https://github.com/shadow3aaa/fas-rs/commit/1e06fd9)、[745d59d](https://github.com/shadow3aaa/fas-rs/commit/745d59d)、[3de8051](https://github.com/shadow3aaa/fas-rs/commit/3de8051)、[3987ed5](https://github.com/shadow3aaa/fas-rs/commit/3987ed5)、[8be78d7](https://github.com/shadow3aaa/fas-rs/commit/8be78d7)、[4029189](https://github.com/shadow3aaa/fas-rs/commit/4029189)。

## v4.5.0

**初始构建提示：**帖子说明 v4.4.0 和 v4.5.0 最初上传的版本因构建问题有误；首次看到该提示的用户需要重新下载 v4.5.0。下载入口见上方 Releases。

- `margin` 改为更直观的 `margin_fps`，并调整 `adjust_target_fps` 的 util 阈值，见 [commit 72651a9](https://github.com/shadow3aaa/fas-rs/commit/72651a9)。帖子没有提供旧 `margin` 到新 `margin_fps` 的迁移公式，因此不能据此推算换算值。
- 基于 cycles 的线程跟踪改为基于 CPU 时间比例，见 [commit de0b92e](https://github.com/shadow3aaa/fas-rs/commit/de0b92e)。
- 分析从 CPU usage 转为 CPU Cluster utilization，见 [commit 28ed4b6](https://github.com/shadow3aaa/fas-rs/commit/28ed4b6)。另修复 `util_max` 单位错误导致超过 100% 的问题，并增加 jank 场景忽略 util 的处理，见 [commit 9cb07b6](https://github.com/shadow3aaa/fas-rs/commit/9cb07b6)。
- 其他目标 FPS、CPU 利用率阈值、偏移及 jank 处理调整： [c7c6c09](https://github.com/shadow3aaa/fas-rs/commit/c7c6c09)、[f8ef859](https://github.com/shadow3aaa/fas-rs/commit/f8ef859)、[699abcf](https://github.com/shadow3aaa/fas-rs/commit/699abcf)、[da357fe](https://github.com/shadow3aaa/fas-rs/commit/da357fe)、[0c94b8b](https://github.com/shadow3aaa/fas-rs/commit/0c94b8b)、[d2f176e](https://github.com/shadow3aaa/fas-rs/commit/d2f176e)、[a98526d](https://github.com/shadow3aaa/fas-rs/commit/a98526d)、[d7cc2b5](https://github.com/shadow3aaa/fas-rs/commit/d7cc2b5)、[0fa3ad7](https://github.com/shadow3aaa/fas-rs/commit/0fa3ad7)。
- 帖子明确列出撤回“提供默认限制策略”的提交 [b0f4f76](https://github.com/shadow3aaa/fas-rs/commit/b0f4f76)。该策略不能表述为仍启用。
- 帖中其他记录包括 perf-event 替换、构建文档/CI、依赖更新和资源清理： [3ff7ec7](https://github.com/shadow3aaa/fas-rs/commit/3ff7ec7)、[5762ad5](https://github.com/shadow3aaa/fas-rs/commit/5762ad5)、[95008fb](https://github.com/shadow3aaa/fas-rs/commit/95008fb)、[5cf5005](https://github.com/shadow3aaa/fas-rs/commit/5cf5005)、[53a7ed6](https://github.com/shadow3aaa/fas-rs/commit/53a7ed6)、[9b8bf50](https://github.com/shadow3aaa/fas-rs/commit/9b8bf50)、[bbd01b4](https://github.com/shadow3aaa/fas-rs/commit/bbd01b4)、[1b815a1](https://github.com/shadow3aaa/fas-rs/commit/1b815a1)、[2907c7f](https://github.com/shadow3aaa/fas-rs/commit/2907c7f)、[690cfe5](https://github.com/shadow3aaa/fas-rs/commit/690cfe5)、[92d86aa](https://github.com/shadow3aaa/fas-rs/commit/92d86aa)、[b02d13e](https://github.com/shadow3aaa/fas-rs/commit/b02d13e)。

v3.7日志还明确将温度指标加入进化算法fitness评定。v4.5则记录以`perf-event2`替代`perf-event`修复长时间运行panic，并移除Python构建依赖；这些历史变动不能直接套到其他版本。v4.5文中的提交URL按原帖短哈希与已知项目仓库组合生成，尚未逐一访问；v3.7已有实际href的链接按原址保留。

来源：[v3.7.0原帖](https://www.coolapk.com/feed/59885254)、[v4.5.0原帖](https://www.coolapk.com/feed/62362897)。
