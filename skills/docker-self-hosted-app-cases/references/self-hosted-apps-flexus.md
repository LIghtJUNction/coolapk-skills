# Flexus X 实例上的自托管应用部署记录

## 通用环境与 Docker 配置

Spacedrive、MaxKB、Audiobookshelf、CodiMD 文章共同记录的环境为 Ubuntu 22.04.4 LTS、Docker 27.2.0、Docker Compose v2.29.2；测评规格为 Flexus X 基础模式 4核12G、100G、3M。Docker 安装步骤及原帖给出的镜像源见《华为云 Flexus X 实例：购买、连接与基础配置》。原文仓库地址在命令中显示为“查看链接”，Docker 镜像加速器 JSON 也有排版损坏，故不能把缺失仓库 URL 或无效 JSON 当作完整可执行配置。

## Spacedrive：分布式文件管理器

原帖将 Spacedrive 描述为开源跨平台文件管理器，核心为 Rust 编写的虚拟分布式文件系统（VDFS）。其内容可寻址存储（CAS）用于标识文件并记录逻辑路径；文章称 VDFS 通过同步客户端数据库支持多设备一致操作。原帖介绍的技术栈为 Prisma、Rust、React、TypeScript、Tauri，并称可预览常见媒体、图片、PDF 和文本格式。iOS、Android 在该文中属于未来计划，不应视为当时已支持的平台。

项目主页：https://www.spacedrive.com/

GitHub 项目：https://github.com/spacedriveapp/spacedrive

原帖提供的 Compose 配置：

```yaml
version: '3.5'

services:
  spacedrive:
    image: ghcr.io/spacedriveapp/spacedrive/server
    container_name: spacedrive
    ports:
      - "6769:8080"
    environment:
      - SD_AUTH=admin:admin
    volumes:
      - /root:/var/spacedrive
    restart: unless-stopped
```

配置将宿主机 `/root` 挂载到容器 `/var/spacedrive`，并将宿主机 6769 映射至容器 8080。`SD_AUTH=admin:admin` 是原帖示例值，部署时应按项目实际配置设置认证信息；原帖没有提供登录验证或安全加固步骤。文章未报告该 Compose 服务实际启动结果。

## MaxKB：RAG 知识库问答系统

原帖将 MaxKB 展开为 Max Knowledge Base，介绍其文档上传、在线文档抓取、文本切分、向量化、RAG 问答、业务系统嵌入和工作流编排能力；可对接本地模型及多家公共模型。上述是文章对功能的概述，未附具体 MaxKB Compose 文件或启动命令。

官网：https://maxkb.cn/

GitHub 项目：https://github.com/1Panel-dev/MaxKB

文章包含 Docker 安装环境段落，但应用本身的部署步骤未出现在所提供正文中。

## Audiobookshelf：自托管有声书与播客库

原帖介绍 Audiobookshelf 支持 mp3、m4a、m4b 等音频格式，具备库管理、上传、分类、排序、收藏、播放进度书签、快进/倒带和速度调节等功能，可通过 Web 在电脑、平板和手机访问。原帖没有给出应用的 Compose 配置、端口或部署启动结果。

官网：https://www.audiobookshelf.org/

GitHub 项目：https://github.com/advplyr/audiobookshelf

## CodiMD：协作式 Markdown 笔记

CodiMD 文章介绍其浏览器编辑、实时协作、Markdown 实时预览、图表与 MathJax、幻灯模式，以及导入和导出 Markdown、HTML、PDF 的功能。文章的部署记录使用 PostgreSQL 11.6 Alpine 和 `nabo.codimd.dev/hackmdio/hackmd:2.5.4`，映射 TCP 3000，并将数据库和上传目录放入命名卷：

```yaml
version: "3"
services:
  database:
    image: postgres:11.6-alpine
    environment:
      - POSTGRES_USER=codimd
      - POSTGRES_PASSWORD=change_password
      - POSTGRES_DB=codimd
    volumes:
      - "database-data:/var/lib/postgresql/data"
    restart: always
  codimd:
    image: nabo.codimd.dev/hackmdio/hackmd:2.5.4
    environment:
      - CMD_DB_URL=postgres://codimd:change_password/codimd
      - CMD_USECDN=false
    depends_on:
      - database
    ports:
      - "3000:3000"
    volumes:
      - upload-data:/home/hackmd/app/public/uploads
    restart: always
volumes:
  database-data: {}
  upload-data: {}
```

文章示例把 `change_password` 直接用于 PostgreSQL 密码和连接串；这是原帖示例配置，不是安全密码建议。部署时还需在云安全组开放 TCP 3000。原帖给出的启动命令有两种写法：`docker compose up -d` 和终端记录中的 `docker-compose up -d`；停止并清理服务的命令写作 `docker-compose down`。启动后可访问 `http://localhost:3000` 或 `http://公网IP:3000`。原帖报告容器处于 Up 状态、端口映射为 `0.0.0.0:3000->3000/tcp`，并称页面可访问；这是单次原帖报告，未独立复测。原帖还称未点击发表的笔记也会自动保存，并介绍账户注册、历史记录、权限修改、分享、主题和语言切换等操作。

项目主页：https://hackmd.io/

GitHub 项目：https://github.com/hackmdio/codimd

Docker Hub 镜像页：https://hub.docker.com/r/hackmdio/hackmd

## AgentOps：智能体开发评估与监控

原帖将 AgentOps 描述为用于构建、评估和监控 AI 代理的工具，列出执行步骤重放与调试、LLM 成本跟踪、超过 1,000 项评估测试、安全合规检测，以及 CrewAI、AutoGen、LangChain 集成。

部署记录是在已激活的 Python 虚拟环境中执行：

```sh
source autogentest/bin/activate
pip install agentops
```

日志显示安装 `agentops 0.3.10`，并安装/调整 `requests 2.31.0`、`psutil 5.9.8`、`packaging 23.2`、`PyYAML 6.0.1`。pip 同时报告依赖冲突：已安装的 `arxiv 2.1.3` 要求 `requests~=2.32.0`，而安装后为 `requests 2.31.0`。因此这份记录表明安装命令完成，但也留下明确的环境依赖冲突；文章没有给出 AutoGen 的具体安装、接入代码或实测监控结果。Flexus、AS、ELB、WAF、DDoS 等部分是总结性描述，没有相应部署配置或测试数据。

## 来源与证据边界

来源：酷安文章 [61831594](https://www.coolapk.com/feed/61831594)、[61831689](https://www.coolapk.com/feed/61831689)、[61831472](https://www.coolapk.com/feed/61831472)、[61831305](https://www.coolapk.com/feed/61831305)、[61832235](https://www.coolapk.com/feed/61832235)。文章图片未提供读取结果；评论未提供。Spacedrive、MaxKB、Audiobookshelf 与 CodiMD 的功能和配置按原帖记录；MaxKB 与 Audiobookshelf 正文没有应用部署配置。AgentOps 文章无项目链接，故不补造下载或仓库地址。
