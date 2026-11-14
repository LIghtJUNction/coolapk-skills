# Voice Companion Bundle：Windows 与 WSL2 本地语音数字人

## 功能组成
原帖介绍的 Voice Companion Bundle 面向 Windows + WSL2，将 Faster-Whisper 语音识别、OpenAI 兼容的大模型接口、Qwen3-TTS 语音合成与参考音色克隆、LiveTalking + Wav2Lip 口型同步整合在一起。支持自定义助手人设、音色和数字人形象，并通过浏览器麦克风实时对话。作者称模型、人物、音色、API Key 与运行日志保存在本机。

项目仓库：<https://github.com/JunWan666/voice-companion-bundle>
原帖指定 Release：<https://github.com/JunWan666/voice-companion-bundle/releases/tag/v1.0.1>

## 安装与启动
下载 Release 压缩包并解压，在目录中找到 `setup.ps1`。在该目录打开 PowerShell，执行：

```powershell
./setup.ps1
```

原帖说明首次运行会安装 Ubuntu 并创建用户，随后再次执行同一命令继续安装。按提示选择显卡推荐档位和下载源；脚本会下载系统依赖、Python 包管理环境、项目资源、数字人资源和 Qwen TTS 资源。原帖流程要求填写 DeepSeek API Key，默认使用在线 LLM 以减少显卡负担；之后启动服务并自动打开浏览器。此为原帖所述版本的操作流程，未给出完整终端输出或硬件兼容矩阵。

## 页面设置与本地模型
页面设置可选择麦克风、调整噪声门限、上传参考音频做音色克隆、上传人物视频生成数字人形象，并配置对话 LLM。原帖提到形象处理较占显存，可按需降低画质。

若使用 LM Studio 本地模型，原帖示例在 LM Studio 的 Server Settings 中把服务端口改为 `12345`，启动服务后 Base URL 为 `http://localhost:12345`。该 URL 原文为 `http:/删/localhost:12345`，按原文说明去除“删”字恢复。回到 Voice Companion 服务设置新增 OpenAI 兼容提供商，填入 Base URL；先在模型 ID 输入任意字符并拉取模型，再删除占位字符、选择已下载模型，测试成功后保存并切换。原帖用 GPU 占用上升作为其本地模型切换成功的观察现象。

## cpolar 远程访问
原帖提供 cpolar 官网 <https://www.cpolar.com/>，并称 Windows 安装后可用命令检查版本：

```cmd
cpolar version
```

Web UI 示例地址为 `http://127.0.0.1:9200`（原文插入“删”字，按说明移除后恢复）。在隧道管理中新建隧道，将本地地址设为 Voice Companion 服务端口；原帖后续称该隧道为 `7860`。在线列表显示同一隧道对应 HTTP 和 HTTPS 公网入口。随机域名可用于临时访问；若需固定地址，在官网预留二级子域名，再编辑隧道选择该子域名。固定域名章节明确针对 `7860` 隧道。

原帖还描述在隧道高级设置中启用 HTTP Auth，以独立用户名和密码保护公网页面，并建议优先使用 HTTPS。此认证是 cpolar 隧道层保护，不是 Voice Companion 自身账户系统。原帖没有给出具体公网地址或 HTTP Auth 凭据。

## 证据边界
原帖标题称 8G 显存可体验，但未提供可复现的完整硬件清单、量化性能数据或长期稳定性测试。本文记录原帖流程及其观察，不将其视为普遍兼容保证。文章图片未读取。

来源：[73436193](https://www.coolapk.com/feed/73436193)（图片与评论未读，未在本库实机复现）。

