# Magisk 模块中的路径映射与属性

先区分系统文件映射、系统属性和运行时接口。它们的处理方式不同，不能把路径名称直接当作可写入位置。

## 映射系统文件

模块的 `system/` 目录递归合并到真实 `/system`。例如：

- `/system` 文件放入模块 `system/` 对应位置。
- `/vendor` 文件放入 `system/vendor/`。
- `/product` 文件放入 `system/product/`。
- `/system_ext` 文件放入 `system/system_ext/`。

当前模块目录见[接口核对](current-interface.md)。不要把 `/proc`、`/sys` 当作 system 镜像映射目录；它们是运行中的内核和系统接口。原帖提到通过脚本处理这些节点，但是否可写取决于具体节点、权限和运行时状态。没有确认这些条件前，不应假设脚本写入必然成功，也不能照抄未经验证的写节点命令。

## system.prop 与设备属性

`system.prop` 使用 `key=value` 格式，Magisk 会通过 `resetprop` 将其中属性作为系统属性加载。旧帖用它举例修改机型或 MIUI 信息，但本轮未读取原帖图片中的具体属性值，因此不补写任何属性项。修改属性只改变软件报告的属性值，不会改变实体硬件，也不保证性能提升。不要把网上流传的“玄学优化”值当作通用配置。

## 用户存储路径的边界

旧帖记录某些布局中 `/storage/emulated/0/Android` 与 `/data/media/0/Android` 的对应关系。这只能视为原帖环境下的路径说明，不是所有设备、用户编号或加密状态下都成立的通用写入路径。应先核对目标设备实际挂载和应用存储状态，再决定模块是否需要处理该位置。

参考：[第四期属性示例帖](https://www.coolapk.com/feed/22223474)、[第五期路径说明帖](https://www.coolapk.com/feed/28474963)、[官方 Magisk 开发指南](https://topjohnwu.github.io/Magisk/guides.html)。

配图未读；未运行模板或验证安装包。

原帖还将 `/storage/emulated/` 与 `/data/media/` 作概括对应；这同样只记录其布局理解，不能省略多用户、加密和挂载检查。第四期另分享[音量键救砖模块相关帖](https://www.coolapk.com/feed/22171000)，该帖与安装包尚未读取，不据链接补写救砖步骤。
