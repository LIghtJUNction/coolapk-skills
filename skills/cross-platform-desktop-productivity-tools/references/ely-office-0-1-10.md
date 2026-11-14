# Ely Office 0.1.10：组件、离线边界与许可

## 应用组成
原帖称 Ely Office 0.1.10 是 ELY-Open-Source 的桌面套件，七个应用可分别安装，也可合装：
- ElyDoc：文字处理，DOCX、ODT。
- ElyTable：表格，XLSX、ODS、CSV，支持公式、筛选、图表。
- ElyShow：幻灯片，PPTX、ODP，支持动画和演讲者视图。
- ElyPDF：阅读、编辑、批注、签名、填表及扫描件 OCR 检索。
- ElyTree：Git 客户端，支持按行/块暂存、提交图、引用日志。
- ElyMark：Markdown 编辑，所见即所得，并可渲染公式、流程图。
- ElyMail：邮件客户端，支持 Gmail/Microsoft OAuth、iCloud、IMAP、POP3。帖子称它在 0.1.10 首发；ElyAgent 属于第二批，未包含在此版本。

## 技术栈与离线边界
外壳使用 Rust、GPUI，不打包 Chromium。原帖列出的本地引擎为：文档应用使用自行编译并打补丁的 LibreOfficeKit <服务器IP>；ElyPDF 使用 MuPDF 1.28.5 与 Tesseract；ElyTree 调用 Git；ElyMark 使用 markdown-rs、RaTeX、mermaid-rs-renderer。

原帖称除 ElyMail 外的六个应用在启动和空闲时均不联网；ElyMail 仅连接用户配置的邮件服务器；检查更新需手动触发。有未保存改动时，每 10 秒在本机写入加密恢复点，称仅本地保存、不上传。以上联网与恢复机制未在输入中独立验证。

## 体积与许可
原帖报告 Windows 安装包实测合集 506.5 MB；macOS 两个包分别 811.7 MB、857.8 MB。单应用体积（MB）：ElyDoc 130.6、ElyTable 128.8、ElyShow 130.8、ElyPDF 36.3、ElyTree 35.5、ElyMark 33.8、ElyMail 10.6。帖子称体积主要来自 LibreOffice 引擎。

外壳许可为 MIT；内核保留各自许可：LibreOfficeKit MPL-2.0、Git GPL-2.0、MuPDF AGPL-3.0、Tesseract Apache-2.0。因而不能把整套软件概称为 MIT。原帖提示二次开发或商用分发需特别审视 AGPL 部分。

## 兼容性与限制
- 原帖称 Windows 包未做代码签名，首次运行可能触发 SmartScreen。
- 基于 LibreOffice 的微软格式兼容性并非逐像素一致；复杂样式、宏和特殊字体需实测。
- 带宏文件、已签名 PDF、修复过的 PDF 只能另存副本，不能原地保存；原帖称这是保护策略。
- 自动保存默认关闭，需手动启用，且只适用于无宏、无签名的自有格式。
- 仅桌面端，无手机版、在线协作或模板库。
- 原帖称 Linux 有 deb、rpm、Arch 包，但未放入该帖网盘；打包脚本未在 Linux 上运行过。
- 项目新建日期、star/fork 数及价格比较是原帖发布时的陈述，不作为当前状态保证。

## 资源
- 原帖提供的下载分享页：[https://pan.quark.cn/s/a99c46254f4e](https://pan.quark.cn/s/a99c46254f4e)。原帖称 Linux 包未放在该网盘。输入未提供提取码或分享页文件清单。

## 来源与证据边界
来源：酷安帖子 [74542651](https://www.coolapk.com/feed/74542651)。帖子附两张未提供内容的图片；本文仅依据正文。评论未提供，评论范围不完整。版本、体积、联网行为及兼容性均按原帖报告记录。
