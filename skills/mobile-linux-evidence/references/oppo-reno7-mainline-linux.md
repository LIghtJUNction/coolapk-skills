# OPPO Reno7 运行 7.3 主线 Linux 分区与刷机线索

本参考整理酷安社区中将 OPPO Reno7 刷入 7.3 主线内核 Linux 的操作线索与风险边界。外部 123 盘附件未下载验证。

- **来源**: https://www.coolapk.com/feed/75025120
- **前提与风险**: 帖中明确不提供 `rootfs` 文件；分区操作会清空全部数据；熔断机型需提前备好强制断电与救砖手段，且所附 TWRP 对熔断机型尚未实测。
- **分区流程线索**: 将 `recovery` 镜像刷入 `boot`（或其他方式进入 Recovery）后，通过 `adb push <path> /sbin/parted` 推送 `parted` 二进制并 `chmod +x /sbin/parted`，随后对 `/dev/block/sd*` 执行分区（具体分区表命令详见原帖后续部分）。
