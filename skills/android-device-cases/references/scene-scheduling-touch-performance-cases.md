# Scene 调度、触控与性能测量历史案例

以下均为社区历史说明或单次实验，未独立复现；软件版本、内核、机型和温控设置会改变结果。

## 调度与版本

- [46753166](https://www.coolapk.com/feed/46753166) 解释 Scene 6 辅助调速器：在原有 walt/schedutil 基础上随负载调整频率边界；以天玑 8100 为例，1800MHz 上限、90% 负载和 300MHz 余量得到 1920MHz，再映射到频率表中的 2100MHz。余量限制到 30MHz 时算得 1650MHz，文中称映射 1700MHz。FAS 或 FEAS 生效时该辅助调速器不工作；具体行为取决于版本。作者称热门游戏可有独立线程放置预设，核心分配是否开启不改变这些预设；检测到 AsoulOpt 工作会避让。配置与格式见 [预设仓库](https://github.com/helloklf/scheduler/tree/scene6-presets)、[格式索引](https://github.com/helloklf/scheduler/blob/scene6-presets/docs/0index.md)。
- [47014406](https://www.coolapk.com/feed/47014406) 记载 Scene 历史配置将骁龙 8 Gen 2 游戏线程主/次核心由 CPU7/CPU6 改为 CPU7/CPU4，区分 6.2HP 和 6.2LP 配置。作者报告 A715 极限性能约高 5%、核心功耗约高 10%，测试条件不足以推广为所有负载。
- [48267216](https://www.coolapk.com/feed/48267216) 为 Scene 7 Core 发布前计划，提到 FAS 适配骁龙 855、天玑 1100/1200，144Hz 及以上帧率统计与新调节面板；计划不等同功能已交付。
- [49627920](https://www.coolapk.com/feed/49627920) 描述 Scene 7 Core 预览：简化界面、自定义配置、游戏列表和通知栏刷新率切换；作者称 FAS 不通过 Hook SurfaceFlinger 或 libgui 实现，并讨论刷新率与游戏帧率匹配。反作弊兼容与账号安全声明未独立验证。[应用页](http://www.coolapk.com/apk/com.omarea.vtools)。

## 触控与编译

[47132708](https://www.coolapk.com/feed/47132708) 以 Redmi K60 Pro 为例，称触控控制涉及 `/dev/xiaomi-touch` 的 ioctl，另提及 `/sys/class/touch/touch_dev/touch_thp_cmd`；作者实验中 Goodix 的 `switch_report_rate` 日志并不证明实际采样率被修改。文中使用 `su -c "getevent -r"` 观察事件频率，但事件间隔受输入行为、驱动及调度影响，不能等同面板硬件采样率。删除设备节点是侵入性实验，不作为通用操作，重启及系统行为未验证。

[47344048](https://www.coolapk.com/feed/47344048) 在 Android 12 上比较 QQ 安装后、speed 编译后及删除 odex/vdex 的占用，观察到系统可能在重启或后台重新优化；作者提及 `install-fast=skip`，并指出旧 Android 版本模式不同。单次观测不能保证节省空间、性能或跨版本兼容；直接删除运行时产物可能导致重编译或故障。

## 散热和 SoC 对比

[46867410](https://www.coolapk.com/feed/46867410) 使用拆除摄像头并加硅脂的 K50 Pro、标称 27W 背夹和高负载游戏，约三分钟接近过热关机，电池接近 40°C；作者不确定背夹是否处于最大功率。改装设备、散热接触和功率未知，因此不构成背夹通用评测。

[47374138](https://www.coolapk.com/feed/47374138) 在高负载游戏场景记录多种 SoC 的帧率与整机功耗，例如天玑 9000 约 42fps/8.3W、8 Gen 2 电池约 42°C 时约 52fps/10W；部分设备已 Root、超频或改压，温度和 CPU 负载也不同。作者明确称数据不严谨，不能当成同条件 GPU 能效排名。

以上关联图片尚未读取，内容和效果未实机复测。
