# 让 APK 轻一点：按需加载与压缩资源

**来源：**[原文《让 APK 轻一点》](https://yumeyuka.moe/blog/apk-shrink/index.md)

## 背景与打包体积

把 [Sub-Store](https://github.com/sub-store-org/Sub-Store) 集成进 Android 应用时，作者借助 [Javet](https://github.com/caoccao/Javet) 驱动 JavaScript。Release APK 一度约 80 MB，体积主要来自 Javet。作者在 [SubCase PR #4](https://github.com/sionnx/SubCase/pull/4) 中启用旧式 JNI 库打包：

```kotlin
// https://developer.android.com/reference/tools/gradle-api/8.0/com/android/build/api/dsl/JniLibsPackagingOptions#useLegacyPackaging()
useLegacyPackaging = true
```

文中记载启用后体积从约 82 MB 降到 22 MB。这个数字来自该项目的打包对比；实际收益取决于库、ABI、压缩和发行配置，不能直接套作其他项目的预期值。

## 将大型 Native 库放进扩展 APK

若大型功能不是每位用户都需要，可以把 `.so` 从主 APK 排除，改为按需安装/下载扩展 APK。文中给出的 [YumeBox 扩展 APK Demo](https://github.com/YumeYucca/YumeBox/tree/Moe/extension) 展示了这种组织方式：扩展 APK 可将二进制放在 `lib/arm64-v8a/` 等目录，主应用则排除 Javet 库：

```kotlin
packaging {
    jniLibs {
        excludes += listOf("lib/**/libjavet*.so")
        useLegacyPackaging = true
    }
}
```

主应用需要查询设备上是否安装扩展时，原文示例在 `AndroidManifest.xml` 声明：

```xml
<uses-permission android:name="android.permission.QUERY_ALL_PACKAGES"/>
```

这是原文使用的广泛包可见性声明，不是加载 SO 的必需权限。已知扩展包名时，可在 `<queries>` 中指定 `<package>`，参见 [Android 包可见性声明](https://developer.android.com/training/package-visibility/declaring)。

扩展 APK 的 Gradle 片段来自 [扩展模块 build.gradle.kts](https://github.com/YumeYucca/YumeBox/blob/Moe/extension/build.gradle.kts)。它清理默认 `jniLibs` 目录并指定 `jniLibs`，关闭 APK 元数据、排除 `META-INF/**`，Release 开启代码与资源压缩，并通过 ABI splits 生成分 ABI APK。ABI 列表读取 `gropify.abi.extension.list`，缺省为 `arm64-v8a`；逗号分隔后去空白并滤掉空项，最后关闭 universal APK：

```kotlin
plugins {
    id("com.android.application")
}

dependencies {
    implementation(libs.javet.node.android)
}

android {
    sourceSets {
        getByName("main") {
            jniLibs.directories.apply {
                clear()
                add("jniLibs")
            }
        }
    }

    tasks.withType<PackageAndroidArtifact>().configureEach {
        doFirst { appMetadata.asFile.orNull?.writeText("") }
    }

    packaging {
        jniLibs {
            useLegacyPackaging = true
        }
        resources {
            excludes += listOf("META-INF/**")
        }
    }

    dependenciesInfo {
        includeInApk = false
        includeInBundle = false
    }

    buildTypes {
        debug {
            isMinifyEnabled = false
        }
        release {
            isMinifyEnabled = true
            isShrinkResources = true
            vcsInfo.include = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro",
            )
        }
    }

    splits {
        abi {
            isEnable = true
            reset()
            val abiList =
                (gropify.abi.extension.list ?: "arm64-v8a")
                    .split(',')
                    .map { it.trim() }
                    .filter { it.isNotEmpty() }
            // Qualify receiver so Kotlin does not resolve include() against Iterable from the
            // chain.
            @Suppress("SpreadOperator") this@abi.include(*abiList.toTypedArray())
            isUniversalApk = false
        }
    }
}

tasks.register<Sync>("collectJavetNative") {
    group = "distribution"
    description = "Extracts the arm64-v8a Javet native library for the Expand release"
    from(provider { configurations.getByName("releaseRuntimeClasspath").files.map(::zipTree) }) {
        include("jni/arm64-v8a/libjavet-node-android.v.5.0.9.so")
        rename { "libjavet.so" }
        eachFile { relativePath = RelativePath(true, name) }
        includeEmptyDirs = false
    }
    into(rootProject.layout.projectDirectory.dir("output_apk/Expand"))
}

tasks.register<Exec>("compressJavetNative") {
    group = "distribution"
    description = "Compresses the Javet native library as an XZ release asset"
    dependsOn("collectJavetNative")

    val inputFile = rootProject.layout.projectDirectory.file("output_apk/Expand/libjavet.so")
    val outputFile = rootProject.layout.projectDirectory.file("output_apk/Expand/libjavet.so.xz")
    inputs.file(inputFile)
    outputs.file(outputFile)
    commandLine("7z", "a", "-y", "-txz", "-mx=9", outputFile.asFile, inputFile.asFile)
}
```

注意事项：示例依赖项目内的 `gropify` 配置、Gradle 类型和 `libs.javet.node.android` 版本；不能作为独立可直接运行的通用脚本。收集任务只提取 `arm64-v8a` 下名字固定为 `libjavet-node-android.v.5.0.9.so` 的文件，并重命名为 `libjavet.so`。升级依赖或增加 ABI 时，需要同步修改路径/版本筛选和扩展分发流程。

## `.so` 的加载和文件权限

原文更正了一个误判：共享库通过 `System.load` 加载时，不要求文件带可执行权限；文件权限位本身不是全部加载条件；还需满足 ABI、依赖库、加载器命名空间与平台权限限制。不能保证任意私有目录内的库都可加载。相关讨论见 [DexKit issue #24](https://github.com/LuckyPray/DexKit/issues/24) 和 [Android NDK issue #979](https://github.com/android/ndk/issues/979)。这项结论针对共享库的加载；不能据此推断 PIE 可执行文件也能从任意私有目录运行。

应用可按“检查文件—存在则加载—加载失败则禁用功能—等待文件可用后再试”的状态处理，避免扩展未安装或下载未完成时强行加载。原文没有给出完整的下载校验、版本兼容、并发或重试实现；这些需要应用自行处理。

## 压缩 Geo 等资源

作者将 Geo 数据视作可更新资源，使用 XZ 后记录约从 15 MB 降到 8 MB。文中认为内核在资源缺失时可以下载，后续也可由应用更新，因此资源可以从后续 APK 中移除；但首次安装需考虑兼容性，尤其需要内置一份可用版本。覆盖安装且不清除数据时，已有文件可能继续可用；清除数据、首次安装或更新逻辑变更时则不能假设文件仍在。

原文引用 CMFA 的 `extractGeoFiles()` 逻辑：先创建目录；以应用 `lastUpdateTime` 为比较基准，若旧文件修改时间早于安装更新日期则删除；文件不存在时从 assets 复制。示例列出的文件为 `geoip.metadb`、`geosite.dat`、`ASN.mmdb`、`BundleMRS.7z`：

```kotlin
private fun extractGeoFiles() {
    clashDir.mkdirs()

    val updateDate = packageManager.getPackageInfo(packageName, 0).lastUpdateTime
    val geoipFile = File(clashDir, "geoip.metadb")
    if (geoipFile.exists() && geoipFile.lastModified() < updateDate) {
        geoipFile.delete()
    }
    if (!geoipFile.exists()) {
        FileOutputStream(geoipFile).use {
            assets.open("geoip.metadb").copyTo(it)
        }
    }

    val geositeFile = File(clashDir, "geosite.dat")
    if (geositeFile.exists() && geositeFile.lastModified() < updateDate) {
        geositeFile.delete()
    }
    if (!geositeFile.exists()) {
        FileOutputStream(geositeFile).use {
            assets.open("geosite.dat").copyTo(it)
        }
    }

    val asnFile = File(clashDir, "ASN.mmdb")
    if (asnFile.exists() && asnFile.lastModified() < updateDate) {
        asnFile.delete()
    }
    if (!asnFile.exists()) {
        FileOutputStream(asnFile).use {
            assets.open("ASN.mmdb").copyTo(it)
        }
    }

    val bundleMRSFile = File(clashDir, "BundleMRS.7z")
    if (bundleMRSFile.exists() && bundleMRSFile.lastModified() < updateDate) {
        bundleMRSFile.delete()
    }
    if (!bundleMRSFile.exists()) {
        FileOutputStream(bundleMRSFile).use {
            assets.open("BundleMRS.7z").copyTo(it)
        }
    }
}
```

此策略用 APK 更新时间判断资源是否需重置，可能覆盖用户在应用目录中的更新数据；实际采用时应明确内置资源与用户下载资源的优先级，并检查更新失败、文件损坏及原子替换情况。原文特别说明其“Geo 等文件”统计不含 `buildMRS.7z`，不要把该文件计入所述压缩结果。

## Native 解压 Loader

作者尝试把 Native 库也压缩为 XZ。Java 层解压速度不理想，于是用 C 实现所需解压部分；Loader 本身不能再被压缩在待解压资源中。文中公开了使用 CMake FetchContent 固定下载 xz 5.8.1 源码及 SHA-256 校验值的片段：

```cmake
FetchContent_Declare(
    xz
    URL https://github.com/tukaani-project/xz/releases/download/v5.8.1/xz-5.8.1.tar.gz
    URL_HASH SHA256=507825b599356c10dca1cd720c9d0d0c9d5400b9de300af00e4d1ea150795543
)
```

实现目录见 [YumeBox pack/native](https://github.com/YumeYucca/YumeBox/tree/Moe/pack/native)。原文没有提供完整的 CMake 配置、解压调用接口、错误处理或 ABI 构建命令，因此不能据此单独复现完整 Loader。

## 壳加载压缩后的 Java 层

APK 中编译后的 Java 字节码位于 `classes.dex`。文章描述以 Loader 壳先解压资源，再装载原应用类和 Native 库；Java 侧复用 Native 解压实现。作者指出，XZ 资源应放在 `assets`，不要放进 `lib`，以免 Android 对 `lib` 目录中的文件再次执行库处理。

原文公开的 [ApplicationBridge.java](https://github.com/YumeYucca/YumeBox/blob/Moe/pack/src/dev/yume/loader/ApplicationBridge.java) 负责通过指定 `ClassLoader` 创建原 `Application`，反射调用 `attach(Context)`，并将 Android 运行时记录的 Application 引用替换为原实例：

```java
final class ApplicationBridge {
    private ApplicationBridge() {
    }

    static Application create(Context base, ClassLoader loader, String className) {
        try {
            Class<?> type = Class.forName(className, true, loader);
            Application application = (Application) type.getDeclaredConstructor().newInstance();
            Method attach = ReflectionAccess.findMethod(Application.class, "attach", Context.class);
            attach.invoke(application, base);
            return application;
        } catch (ReflectiveOperationException error) {
            throw new IllegalStateException("Unable to create the original Application: " + className, error);
        }
    }

    @SuppressWarnings("unchecked")
    static void replace(Application shell, Application original) {
        try {
            Object base = ReflectionAccess.get(shell, "mBase");
            Object loadedApk = ReflectionAccess.get(base, "mPackageInfo");
            Object activityThread = ReflectionAccess.get(loadedApk, "mActivityThread");
            ReflectionAccess.set(loadedApk, "mApplication", original);
            ReflectionAccess.set(activityThread, "mInitialApplication", original);
            List<Application> applications = (List<Application>) ReflectionAccess.get(activityThread, "mAllApplications");
            int index = applications.indexOf(shell);
            if (index >= 0) {
                applications.set(index, original);
            }
            ReflectionAccess.set(base, "mOuterContext", original);
        } catch (ReflectiveOperationException error) {
            throw new IllegalStateException("Unable to replace the loader Application", error);
        }
    }
}
```

该代码依赖 `ReflectionAccess` 及 Android 内部对象字段，且以反射更换框架持有的 Application。原文未给出兼容性矩阵；Android 版本或隐藏 API 行为变更都可能影响它。它是项目壳实现的一部分，不是只复制这个类就能工作的通用加固模板。

## 文中体积数据

作者报告：不打包 Geo、作为更新包安装时约 18–20 MB，随内核大小变化；含 Geo 的对比为 YumeBox 35 MB、ClashMetaForAndroid 42.8 MB、FlClash 52.4 MB、ClashMi 43.9 MB。这些是原文对特定构建的统计，不代表同版本、同 ABI、同压缩配置下的普遍基准。
## 协调补充与修正

- 原文一个源码链接协议写成 `shttps`，这里修正为 `https`；未把修正后的项目路径当作已检查可用。
- 正文一处写 `buildMRS.7z`，代码示例实际写 `BundleMRS.7z`；未证明两者是同一文件，不自动统一。
- Geo 的时间戳删除逻辑不能原样用于删掉内置 assets 的更新包：旧文件被删后仍会执行 `assets.open()`，若资源已移除就会失败。需要同时调整资源更新逻辑，保留现有数据或提供实际可用的获取路径。
- 目标 Android 14/API 34 及以上的 DEX/JAR/APK 动态加载文件需按平台要求设为只读，官方示例在打开文件后、写内容前设置只读；不能只解压后立即加载。见 [Android 14 动态代码加载要求](https://developer.android.com/about/versions/14/behavior-changes-14#safer-dynamic-code-loading)。
