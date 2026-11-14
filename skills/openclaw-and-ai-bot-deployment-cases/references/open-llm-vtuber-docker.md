# Windows Docker 部署 Open-LLM-VTuber 并配置模型与远程访问

## 适用范围与证据边界
原帖采用 Windows + Docker Desktop 部署 Web 版，说明其部分本地交互能力较桌面版精简。文中报告模型对话、语音和画面识别体验，但未提供项目仓库 URL，且图片未读取；模型名称及版本按原文记录，不代表当前可用性或能力保证。评论未提供。

## 功能与部署脚本
Open-LLM-VTuber 将大语言模型、TTS、ASR 和 Live2D 虚拟形象整合为交互界面。原帖列出摄像头/屏幕/截图感知、触控互动、情绪映射、透明置顶宠物模式、聊天记录、主动说话及语音翻译等功能。

先启动 Docker Desktop，在 PowerShell 执行 `docker version`；原帖说明需能看到客户端和服务端版本信息，只有客户端信息或无法连接 Engine 时表示 Docker 尚未就绪。

执行原帖的一键部署管理命令（“删”字按原帖说明删除）：

```powershell
irm https://gitee.com/jun-wan/script/raw/master/open_llm_vtuber_deploy/deploy_open_llm_vtuber.ps1 | iex
```

菜单提供安装/更新、卸载、重启及打开 `conf.yaml` 目录等操作。首次选择 `1`，按提示输入 OpenAI 兼容接口的 Base URL、模型名称和 API Key。原帖用硅基流动接口举例，模型示例为 `deepseek-ai/DeepSeek-V4-Pro`；这些是原帖当时示例，不应视为当前模型目录或服务可用性的保证。配置通过后脚本拉取 Docker 镜像并启动容器；原帖称默认 Web 端口通常为 `12393`。

## 人设与视觉模型
通过脚本菜单选择 `4` 打开 `conf.yaml` 所在目录，用文本编辑器修改提示词并保存，再重新运行脚本选择重启容器使配置生效。原帖示例替换人设提示词后再次对话，报告回复风格发生变化。

原帖称示例 `deepseek-ai/DeepSeek-V4-Pro` 不支持视觉，演示时将模型名换成 `moonshotai/Kimi-K2.7-Code` 后测试画面识别；这只是原帖的模型能力陈述和体验记录，未独立核验。语音转文字需要在浏览器授予麦克风权限。

## cpolar 远程访问
Windows 安装 cpolar 后，在 CMD 执行 `cpolar version` 查看版本；本机 Web UI 示例为 `http://127.0.0.1:9200`。创建 HTTP 隧道，目标端口填写 `12393`，原帖地区示例为 `China`。在线隧道列表会显示 HTTP 和 HTTPS 公网地址。随机域名约每 24 小时变化；固定二级子域名需先在官网预留，再修改隧道域名类型和名称。

原帖演示了公网访问，但没有在正文给出真实公网域名。cpolar 仅提供访问通道，Open-LLM-VTuber 仍运行在本地 Docker 环境。

## 来源
- 原帖：<https://www.coolapk.com/feed/72600050>
- PowerShell 部署脚本：<https://gitee.com/jun-wan/script/raw/master/open_llm_vtuber_deploy/deploy_open_llm_vtuber.ps1>（按原帖明确说明删除插入的“删”字恢复）
