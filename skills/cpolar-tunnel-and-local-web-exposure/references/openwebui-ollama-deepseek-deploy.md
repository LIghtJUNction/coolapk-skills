# OpenWebUI、Ollama 与 DeepSeek R1 部署记录

## OpenWebUI 安装与启动

原帖建议 Python 3.11，列出的环境为 Windows 10/11、macOS 10.15+、Ubuntu 18.04+、CentOS 7+，内存建议 8GB 以上、可用空间至少 10GB。Ollama 可选：也可连接远程 API。

原帖命令如下：

```sh
brew install python@3.11
sudo apt install python3.11
python --version
pip config set global.index-url
pip install open-webui
open-webui serve
```

其中 Windows 安装步骤、Python 下载地址和浏览器访问地址在原帖中留空；`pip config set global.index-url` 后的镜像地址也缺失。文章开头列出的清华 PyPI 镜像为 `https://pypi.tuna.tsinghua.edu.cn/simple`，但命令中没有写出该值，不能视作完整可直接执行的命令。首次进入 Web UI 后按页面创建管理员账号。

## 接入本地 Ollama

原帖提供的安装及服务命令：

```sh
winget install Ollama.Ollama
brew install ollama
curl -fsSL https://ollama.ai/install.sh | sh
ollama serve
ollama
```

Linux 安装脚本原文写作 `https:/删/ollama.ai/install.sh`，按原帖明确说明删除“删”标记恢复为 `https://ollama.ai/install.sh`。这条恢复后的链接是根据原文机械还原，未独立访问验证。

OpenWebUI 可识别本机 Ollama 模型。原帖报告本机已有 `qwen3:32b`，并以 `deepseek-r1:1.5b` 演示拉取；文中称模型约 1.1G，属于原帖记录。接入局域网中另一台 Ollama 服务时，在管理员设置的“外部连接”中添加 Ollama API 连接，再检查首页模型列表；原帖报告显示 `deepseek-r1:671B` 并成功演示对话。具体连接地址及截图未提供，不能据此补写连接参数。

## 接入云端 OpenAI 兼容 API

原帖以阿里云百炼的 DeepSeek R1 为例：在百炼领取新用户模型 Token 额度、创建 API Key，再从 OpenWebUI 管理员设置的“外部连接”添加 OpenAI API。原帖所示额度、服务界面和模型可用性属于发帖时的描述，不代表当前政策。原帖包含一段形似 API Key 的字符串；此处不复述账户凭据。API 地址及界面截图内容未在文字中给出。

## cpolar 远程访问说明

原帖以 cpolar 将 OpenWebUI 服务暴露到公网：编辑 `website` 隧道使其指向 WebUI 服务端口，随后从在线隧道列表使用 HTTP 或 HTTPS 地址访问。原帖描述免费随机域名约每 24 小时变化；固定二级子域名需在预留页面登记后绑定隧道。原帖未明确写出 OpenWebUI 的监听端口、隧道本地地址、cpolar 官网链接或实际公网地址，因此这些值不能从文中确定。

## 证据边界

内容基于酷安原帖文字。部署演示 GIF、截图和外链正文未读取；文中未给出 OpenWebUI 项目仓库链接，也未提供各连接配置的完整字段。功能介绍、兼容性和部署成功均为原帖陈述，未独立验证。

来源：[68008518](https://www.coolapk.com/feed/68008518)（图片与评论未读，未在本库实机复现）。

