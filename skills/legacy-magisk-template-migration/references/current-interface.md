# 当前接口核对

依据[Magisk 官方指南](https://topjohnwu.github.io/Magisk/guides.html)：模块目录为 `/data/adb/modules/<id>`；`system/` 合并 `/system`，其他系统分区内容放 `system/vendor`、`system/product`、`system/system_ext`。

`module.prop` 包含 id、name、version、versionCode、author、description；updateJson 可选。ID 规则为 `^[a-zA-Z][a-zA-Z0-9._-]+$`，版本码用整数，文件使用 LF。旧 minMagisk 例子不构成当前必需字段或编码公式。

安装时 `customize.sh` 被 source，目标为 `MODPATH`；运行脚本可用 `MODDIR=${0%/*}` 取得自身目录。`post-fs-data.sh` 与 `service.sh` 分别对应 post-fs-data 和 late_start 阶段。不要混用旧 common 目录结构与当前接口。

这里只核对文档；没有安装、执行或设备验收。
