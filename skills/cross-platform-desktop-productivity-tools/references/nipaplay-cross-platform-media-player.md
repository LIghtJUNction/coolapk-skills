# NipaPlay：跨平台本地媒体、弹幕与媒体库

## 版本与平台

原帖记载，NipaPlay 使用 Flutter + Rust，Windows 包约 142 MB，采用 MIT 许可。1.11.10 于 2026-10-23 发布。项目仓库：`https://github.com/AimesSoft/NipaPlay-Reload`（原帖仅写仓库名，补全 HTTPS 为推断）。

Windows、macOS、Linux、Android、iOS 提供客户端；另有 iPad、Android TV、tvOS 与鸿蒙构建。原帖提醒 tvOS、鸿蒙的构建状态应以官方平台矩阵为准，不能据此视为完整成品。文中列出的下载包为 Windows x64、Android arm64、macOS Apple Silicon；Windows 为解压即用的 ZIP，macOS DMG 不适用于 Intel Mac。iOS 可在 App Store 搜索 NipaPlay。该次未提供 Intel Mac 或 Linux 包。

## 播放、弹幕与媒体库

客户端直接播放设备本地文件，也可连接 NAS；这与只能远程控制电脑播放器的方案不同。拖入视频后，应用依据文件名识别番剧和集数，再从弹弹play 弹幕库匹配弹幕。因此改成 `01.mkv` 这类缺少番名的文件可能无法自动识别，需要手动指定；弹幕来源是弹弹play，不包括 B 站弹幕。支持滚动、顶部、底部弹幕、轨迹记忆、防遮挡和本地 XML/JSON 弹幕文件。1.11.10 新增每条轨道独立调轴，以小数秒为单位，正数提前、负数延后。

追番功能可关联 Bangumi 账号，同步观看进度，并在应用内评分、评论；新番表按星期展示。媒体库可连接 Emby、Jellyfin、SMB、WebDAV，并使用 Bangumi 数据刮削作品信息。字幕支持外挂与内嵌 ASS、SRT，多轨切换和样式设置；支持音轨切换与倍速播放。

## 播放内核和画质功能

可切换四种内核：自研 Erika（Rust）、FVP（libmdk）、Media Kit（libmpv）和系统视频播放器。遇到播放兼容性问题可尝试切换内核。Anime4K 超分与 CRT 滤镜的表现依平台而异；原帖称 Linux 是没有自研 Erika 内核的平台，因此效果可能打折。AI 防剧透是原帖所述功能，宣称识别并遮挡可能剧透内容，未提供独立测试条件。

## 获取

- 项目仓库：`https://github.com/AimesSoft/NipaPlay-Reload`（由原帖仓库名补全协议，HTTPS 为推断）。
- 原帖下载分享：夸克网盘 https://pan.quark.cn/s/5df11790c92b
- 原帖称下载后可核对文件哈希，但没有给出哈希值；本文未验证分享文件。

## 证据边界

以上版本、功能和兼容性是原帖报告。原帖含 2 张图片，图片内容未读取；评论记录未提供且历史评论未完成。

来源：[74589781](https://www.coolapk.com/feed/74589781)（图片与评论未读，未在本库实机复现）。

