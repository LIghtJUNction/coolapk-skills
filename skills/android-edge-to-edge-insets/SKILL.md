---
name: android-edge-to-edge-insets
description: 核对Android15/16边到边显示的系统和targetSDK条件，整理旧APK布局修复片段、主题资源及WindowInsets处理边界。
---

# Android 15 EdgeToEdge 标题栏遮挡修复

Android 15 上，面向 API 35 的应用默认边到边显示；不能写成所有应用一律如此。原帖称部分布局内容被系统栏遮挡，其“未使用 UI 库时常见”的说法不是严格判定条件。应先区分系统状态栏与应用自己的标题栏，配图未读时不能确定具体遮挡节点。

可尝试以下修复方式：

1. 将 APK 的 Target API 设为 34 或更低。原帖指出，部分系统上此法不生效。
2. 在 `res/values-v35/styles.xml` 中，为实际应用到 Activity 的主题 style 加入以下资源项；不是在 XML 文件任意位置插入：
   ```xml
   <item name="android:windowOptOutEdgeToEdgeEnforcement">true</item>
   ```
3. 若应用内没有 `styles.xml`，在 `res/layout` 中找到发生遮挡的 XML，并在根控件（第一个控件）添加：
   ```xml
   android:fitsSystemWindows="true"
   ```
4. 若应用使用 Java 布局，在 Activity 的 Java `onCreate()` 中找到 `setContentView(xxxView)`，并在其上一行添加：
   ```java
   xxxView.setFitsSystemWindows(true);
   ```

原帖还提到 dex 字节码示例中 `p1` 是 `xxxView`，`v4` 是布尔值 `true`；具体字节码图片未提供可读内容，不能据此补写指令。

**来源与证据边界：**基于原帖文字步骤。示意图片未读取，未独立验证各方法在不同系统和应用上的效果。

## 平台条件与更长期的处理

[Android 15文档](https://developer.android.com/about/versions/15/behavior-changes-15#edge-to-edge)要求布局正确处理系统栏insets；Material组件、AppBarLayout和自定义容器的处理不同，不能认为任意根控件加fitsSystemWindows都保证解决。

[Android 16文档](https://developer.android.com/about/versions/16/behavior-changes-16#edge-to-edge)明确：target API 36的应用在Android 16上不能再用windowOptOutEdgeToEdgeEnforcement退出边到边；同一target在Android 15上该属性仍可工作。因此应同时记录运行系统与targetSDK，旧规避项不等于永久修复。对自定义View容器，官方给出的方向是通过WindowInsets监听处理padding。

降低targetAPI只是原帖提出的规避方式，未必对主动启用边到边或特定系统有效。若编辑APK，还需重建并处理签名；原帖未给完整打包过程。上述XML/Java片段并不是可直接安装的补丁包，未执行APK修改或实机验证。


来源：[原帖67482763](https://www.coolapk.com/feed/67482763)。
