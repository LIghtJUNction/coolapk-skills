# Flexus X 的 OpenCart 与 Dufs 部署环境记录

## 共同环境与实例配置

两篇原帖均以 Flexus X 上的 Docker 部署为背景，记录 openEuler 22.03 LTS、Docker 26.1.3；环境表使用 openEuler 镜像，内网地址信息不复录。作者说明这是个人测试学习环境，并提示生产环境谨慎。原帖的购买配置参考为：包年/包月；华北—北京四；自定义 4 vCPU、12 GiB；系统盘通用型 SSD 100G；弹性公网 IP，全动态 BGP，带宽 3 Mbit/s；登录凭证自定义密码。OpenCart 文中另写了 Huawei Cloud EulerOS 2.0 Standard 64 bit（10GiB）作为配置参考，同时明确说明本次实践已从 Huawei Cloud EulerOS 切换为 openEuler 22.03 LTS。应以其环境表中的 openEuler 记录理解本次环境，不能把旧镜像参考当成实际系统。

远程连接部分建议在控制台取得弹性公网 IP，再使用 Xshell 输入 IP、登录用户和密码连接。安全组及购买操作有图示，但图未读取，正文没有具体安全组规则或完整控制台步骤。

## OpenCart

OpenCart 是开源电商平台。原帖列出的功能包括多语言、多货币、支付与配送方式、插件和主题定制、SEO、多商店管理及移动设备适配。文章说明计划在 Docker 环境部署 OpenCart，但提供的正文没有镜像、容器命令、端口或持久化配置，无法据此复现部署。原帖关于运行流畅及稳定性的描述是作者体验，不是独立测试结论。

原帖活动页链接：[https://activity.huaweicloud.com/](https://activity.huaweicloud.com/)。这是活动页，不是 OpenCart 项目或安装文件链接。

## Dufs

Dufs 是原帖介绍的文件服务器，所列功能包括静态文件服务、文件夹 ZIP 下载、拖放上传、创建/编辑/搜索文件、断点续传及部分上传/下载、访问控制、HTTPS、WebDAV 和 cURL 兼容。原帖称在上述 openEuler 环境通过 Docker 成功部署，但没有给出 Dufs 镜像、运行命令、端口映射或访问控制配置，不能从正文复原部署步骤。运行体验是作者报告，未经独立验证。

原帖活动页链接：[https://activity.huaweicloud.com/](https://activity.huaweicloud.com/)。该链接指向活动页，非 Dufs 项目链接。

## 证据边界

来源：[OpenCart 原帖](https://www.coolapk.com/feed/61900898)、[Dufs 原帖](https://www.coolapk.com/feed/61861372)。两篇原帖各有 6 张图片，未读取；评论未读取。文中没有提供 OpenCart 或 Dufs 的实际项目、镜像和下载地址。
