# Salt Player 跨平台版本变更

## 鸿蒙 1.0.1 与 1.1.0

[1.0.1 更新](https://www.coolapk.com/feed/71268128)新增 WAV 内嵌歌词支持并扩展 M4A 歌词识别，调整扫描授权和文件夹屏蔽逻辑；修复清空队列后歌词残留、重新扫描无法恢复歌曲以及部分 M4A 歌词识别问题。文档选择器单次扫描上限提高至 1000，实际能力按版本与系统权限核对。

[1.1.0 更新](https://www.coolapk.com/feed/71481224)新增歌单新建、编辑及 TXT 导入导出；播放页右滑可进入播放信息页，音乐库可查看容量与格式统计；支持列表多选和定位当前播放。更新还修复扫描暂停/继续和退出取消授权、播放被其他应用抢占时歌词进度不停止等问题。

## Android 11.0 和 11.1 测试版

[11.0.0 发布记录](https://www.coolapk.com/feed/69414921)列新增 Flyme 状态栏歌词及灵动光环设置，优化车载蓝牙歌词与 vivo 智能车载歌词选项，修复部分 MP3 歌词和 FLAC 封面读取问题。[11.1 beta](https://www.coolapk.com/feed/69957156)引入新队列并禁止重复添加、支持重叠时间歌词多行高亮，同时移除 DSD 扫描与内置播放支持；这是测试版变更，后续正式版应另核对。

## Windows 1.9 和 1.11

[1.9.0 记录](https://www.coolapk.com/feed/69434193)增加 DISC/TRACK 中 `02/18` 格式标签解析、按专辑排序时按音轨号细排，并修复部分 Windows 11 桌面歌词不显示问题。[1.11.1 补丁](https://www.coolapk.com/feed/70099314)修复 DirectX 模式单曲循环线程死锁；1.11.0 同时调整低速存储场景的默认缓冲，增加桌面歌词颜色和窗口尺寸保存。更新记录反映开发者所述修复，未进行设备复现。

[Windows 1.12.0](https://www.coolapk.com/feed/70486556)新增列表单次播放及从音乐库安全移除时保护歌单歌曲的选项；不支持 DirectX 12 的设备会回退 OpenGL，并修复扫描建库时删除歌曲导致崩溃等问题。4 张配图未读。[鸿蒙 1.2.2](https://www.coolapk.com/feed/71674716)的开发者公告称已上架并增加逐字歌词；[另一公告](https://www.coolapk.com/feed/71924227)称鸿蒙版适配 6.1 沉浸光效，设备和具体版本范围待核实，配图未读。

[播放界面设置提示](https://www.coolapk.com/feed/69927990)：在设置的用户界面、播放界面选项中，可单独选择浅色或深色文字方案；不同版本菜单可能变化。

[Windows 1.7.0](https://www.coolapk.com/feed/67556250)新增文件夹页、播放队列和歌单拖拽排序，修复慢速传输曲目自动切歌时可能卡住的问题。[Android 10.10.2](https://www.coolapk.com/feed/68007319)将目标 SDK 提升至 Android 16（API 36），并修复部分返回崩溃；目标 SDK 不等于最低支持系统版本。

[青盐云听 2.0 dev08](https://www.coolapk.com/feed/69397424)记录修复 WebDAV 播放中途跳到下一首的问题。原帖公开下载：[123 云盘](https://www.123865.com/s/NMWXjv-uYYMv?pwd=p1Xy)，提取码 `p1Xy`；链接有效性和文件完整性未验证。[椒盐笔记 0.31.0](https://www.coolapk.com/feed/68523094)记录界面优化和崩溃修复，原帖公开下载：[123 云盘](https://www.123865.com/s/NMWXjv-UZFMv?pwd=oECh)，提取码 `oECh`；该链接同样未验证。

[Flyme 适配公告](https://www.coolapk.com/feed/68080964)称 Flyme 12.3.1.2A 增加 Salt Player 灵动光环音乐灯效支持；[V11 更新说明](https://www.coolapk.com/feed/69415114)列专用状态栏歌词与光环设置，以及暂停后歌词未隐藏的修复。实际效果依设备和系统版本而异，配图未读。

[Windows Steam 1.4.21 EA 公告](https://www.coolapk.com/feed/66203840)称修复系统托盘乱码，微软商店渠道同期为 1.4.20 EA；跨渠道版本号不能视作完全同步，配图未读。
