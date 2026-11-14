# Flexus X 上部署 Ghost：原帖环境与 Docker 安装片段

## Ghost 简介与项目入口

Ghost 是面向出版和内容业务的开源平台。原帖列出的能力包括网站与主题定制、网页和邮件编辑器、新闻通讯、免费或付费会员订阅，以及会员和内容参与数据分析；并称其支持 Markdown 编辑。原帖关于安装包体积、运行内存和相对 WordPress 性能的数字没有给出测量方法，作为原帖说法记录，不作为可比较的基准结论。

- Ghost 项目官网：<https://ghost.org/>
- Ghost GitHub 项目：<https://github.com/tryghost/ghost>

## 原帖实验环境

原帖列出的服务器为 Flexus X 实例，Ubuntu 22.04.4 LTS，Docker `27.2.0`，Compose `v2.29.2`，内网 IP `192.168.0.168`。作者称测试规格为 `4核12G-100G-3M` 基础模式，并报告 Ghost 使用过程流畅；没有负载、延迟或资源数据可独立验证。

## Docker 安装步骤片段

原帖提供了安装依赖与添加 GPG 密钥的步骤：

```sh
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

sudo curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

正文随后在“配置仓库源”处被截断，没有给出完整仓库地址或后续 Docker 安装、Compose 文件和 Ghost 服务启动步骤。原帖外链清单中 Docker GPG URL 与上述命令一致；Ghost 项目链接分别如上。没有足够信息复原一套可执行的 Ghost 部署流程。图片未读取，可能包含正文未保留的部署细节。

来源：[61923302](https://www.coolapk.com/feed/61923302)（图片与评论未读，未在本库实机复现）。

