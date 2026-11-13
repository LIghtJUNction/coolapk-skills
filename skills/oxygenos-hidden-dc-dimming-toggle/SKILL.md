---
name: oxygenos-hidden-dc-dimming-toggle
description: 氧 OS 隐藏 DC 调光选项模块
---

# 氧 OS 隐藏 DC 调光选项模块

该模块用于在氧 OS 设置中显示隐藏的低亮度无频闪选项。显示开关仍需要内核支持才能实际工作，不能仅凭开关出现判断调光方式。

## 适用范围与限制

作者只在一加 8 的氧 OS 13 上测试。氧 OS 12/13 和 ColorOS 12/13（包括帖子提到的一加 9R）属于作者提出的理论适用范围，未由现有材料验证。

模块功能名：`oplus.software.display.dcbacklight_support`。博文示例配置路径（并非所有固件的固定路径）：`my_product/etc/permissions/dc_dimming.xml`。核对到的原实现是在 `post-fs` 阶段对配置执行 bind mount；不能据此推断所有 Magisk 版本或系统版本采用相同机制。

## 获取

版本 1.0：[模块下载](https://blog.xzr.moe/usr/uploads/2022/12/2186499378.zip)。包内容未验证。博客正文已读，评论未验证；原帖图片及全部评论未读取。

来源：[酷安原帖](https://www.coolapk.com/feed/41460149) · [作者技术说明](https://blog.xzr.moe/archives/230/)。本稿未执行设备修改、工具安装、包校验或测量；配图和评论仍未读。
