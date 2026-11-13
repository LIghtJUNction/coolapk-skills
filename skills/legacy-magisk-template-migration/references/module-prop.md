# 编写 Magisk 模块的 module.prop

`module.prop` 保存模块标识和展示信息。旧教程介绍的 `/sbin/.magisk/img/`、`Magisk/<id>` 安装路径以及 `minMagisk` 字段属于历史环境，不是当前模块的必需配置。教程中 `minMagisk=1600`、`1730` 和“版本号乘以 100”的说法也只是旧例，不能用于推导现代 Magisk 版本码。

## 字段示例

```properties
id=example.module
name=Example Module
version=1.0
versionCode=1
author=Example
description=Module description
```

`id` 识别模块，`name` 是展示名；`version` 是版本文字，`versionCode` 用于版本比较；`author` 是署名字段，`description` 说明作用。示例是整理者自拟，不是原模板参数。

当前 ID 约束、换行和可选更新字段见[当前接口核对](current-interface.md)。旧教程将模块与应用作类比，不代表模块 ID 就是 APK 包名。

来源：[9525508](https://www.coolapk.com/feed/9525508)。5张配图未读。


配图未读；未运行模板或验证安装包。
