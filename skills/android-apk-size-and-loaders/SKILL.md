---
name: android-apk-size-and-loaders
description: 在 Android 应用体积主要来自 Native 库、Geo 数据或 DEX 时，评估旧式 JNI 打包、按需扩展、XZ 与 Loader；包含具体 Gradle 和 Java 片段。
---

# Android APK 瘦身与按需加载

先解包区分体积来自 Native 库、数据还是 DEX，再选择对应手段。原文项目中，旧式 JNI 打包使 APK 从约 82MB 降到 22MB；这不是安装占用同比下降，也不是其他项目的收益保证。

```kotlin
packaging {
    jniLibs {
        excludes += listOf("lib/**/libjavet*.so")
        useLegacyPackaging = true
    }
}
```

这里同时做了两件事：排除 Javet 供按需加载、选择旧式 JNI 打包。排除库后必须有实际扩展/下载与加载逻辑；仅复制配置会让依赖缺失。

可选方案：把 Javet 放进扩展 APK，或者下载到应用私有位置后加载；把 Geo 数据、SO、DEX 分别 XZ 压缩，由未被压缩的 Native Loader 解压。XZ 资源放 `assets`；Loader 不能依赖先解压自己。DEX 壳还涉及原 Application 创建、替换与隐藏 API 兼容，不能等同于普通资源压缩。

[完整配置与机制](references/packaging-and-loader.md)保留了扩展 APK 构建、固定 Javet 5.0.9 收集路径、`7z -txz -mx=9`、Geo 更新逻辑、xz 5.8.1 URL/哈希及 ApplicationBridge 代码。资源缺省、动态加载要求和原文笔误也在那里逐项说明。

来源：[让 APK 轻一点](https://yumeyuka.moe/blog/apk-shrink/index.md)。


本技能来自酷安原帖所链接的公开站点文章，单独作为外链知识整理；不算作酷安帖子正文覆盖。正文已读，配图未读。除单独注明的本地验证外，未运行项目、安装扩展或下载发行包。
