# FileShare v1.0.14：局域网浏览器文件共享

## 架构与传输
帖文报告 FileShare v1.0.14 于 2026-11-05 发布，为非预发布版。项目地址：[github.com/tri5m/file-share](https://github.com/tri5m/file-share)，许可证为 MIT；帖文报告建库于 2026-04-25、当时有 59 stars。

桌面端使用 Tauri 打包，内嵌 Rust 局域网 HTTP 服务并提供文件池与共享页面。Windows 或 macOS 电脑安装桌面端；手机、平板及其他电脑通过浏览器访问，无需登录或注册。拖入文件时只登记本机路径，不复制一份完整文件。两端需在同一局域网，传输速度取决于设备和路由器；原帖没有实机测速数据。

桌面文件可由手机扫码进入共享页并下载。帖文称下载请求使用 `Range`，响应带 `Accept-Ranges: bytes`，因此下载中断后可续传；上传侧是否支持断点续传，原帖未说明。手机浏览器也可上传文件到电脑的 `Downloads` 目录。文本可以从电脑粘贴到网页，再由手机复制。

## 安全与使用条件
共享页没有密码；同一 Wi-Fi 网络中获得地址的设备可能访问共享内容。适合可信任的家庭网络，使用后应停止分享；开放网络中需谨慎。帖文描述桌面端不复制大文件，但没有给出访问控制细节或安全测试结果。

macOS 两个安装包据帖文称未签名、未公证。若首次打开提示“已损坏”，帖文给出的处理方法是先将 `FileShare.app` 拖入“应用程序”，然后在终端执行：

```sh
xattr -dr com.apple.quarantine /Applications/FileShare.app
```

帖文另称 DMG 内附“损坏修复”工具。该命令会移除应用的 quarantine 属性；来源未提供更详细的签名或发布校验资料。

## 帖文列出的安装包规格
- Windows `setup.exe`：3,734,380 字节，约 3.5 MB。
- 简体中文 MSI：5,443,584 字节，约 5.2 MB。
- macOS Intel DMG：8,286,043 字节。
- macOS Apple Silicon DMG：8,082,781 字节。

## 获取
- 光鸭网盘分享：[https://www.guangyapan.com/s/1967562078023364632_auwOgx1YeiVTPWqW](https://www.guangyapan.com/s/1967562078023364632_auwOgx1YeiVTPWqW)
- 夸克网盘分享：[https://pan.quark.cn/s/bf3f7db2dd57](https://pan.quark.cn/s/bf3f7db2dd57)

来源未记录分享提取码。文章含两张图片，未读取；评论未提供且历史评论未完成。项目版本、协议细节、文件大小和平台行为均为原帖报告，未下载或独立验证。

来源：[75005498](https://www.coolapk.com/feed/75005498)（图片与评论未读，未在本库实机复现）。

