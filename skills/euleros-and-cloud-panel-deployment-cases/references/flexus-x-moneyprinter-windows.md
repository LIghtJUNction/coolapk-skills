# Windows 云主机部署 MoneyPrinterTurbo 短视频生成工具

本记录整理自基于 Windows 云主机（Flexus X）部署 MoneyPrinterTurbo 的社区步骤；原帖含 5 张图片（未读取），未在本库实机复现。

## 核心步骤与配置

1. 在项目根目录激活 Conda 环境：

```bat
conda activate MoneyPrinterTurbo
```

2. 使用 Docker 启动本地 OpenAI 兼容代理服务（原帖示例镜像 `missuo/freegpt35`，映射 `3040` 端口）：

```bash
docker run -p 3040:3040 missuo/freegpt35
```

3. 修改项目 `config.toml` 中的 LLM 配置，接入本地代理地址 `http://localhost:3040/v1/`：

- `llm_provider = "openai"`
- `openai_api_key = "123456"`（原帖示例占位值）
- `openai_base_url = "http://localhost:3040/v1/"`
- `openai_model_name = "gpt-3.5-turbo"`

4. 启动后可在本地查看 API 文档入口 `http://127.0.0.1:8080/docs` 与 `http://127.0.0.1:8080/redoc`，并通过 Web 界面生成横屏/竖屏视频。

## 证据边界

- 原帖未给出完整的 Conda 环境创建、依赖安装与 WebUI 启动命令（可能位于未读图片或前置步骤中）；`missuo/freegpt35` 的可用性与上游服务状态未在本库验证。
- 来源：[Coolapk 帖子 61913643](https://www.coolapk.com/feed/61913643)。
