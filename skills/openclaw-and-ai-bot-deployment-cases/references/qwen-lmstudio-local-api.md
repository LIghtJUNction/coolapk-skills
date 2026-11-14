# LM Studio 运行 Qwen3.8：显存选择、OpenAI 兼容 API 与远程访问

## LM Studio 与模型选择
LM Studio 提供 Windows、macOS、Linux 桌面端，可图形化搜索、下载和加载模型，并调整内存、上下文长度和 GPU 卸载层数。它也可启动 OpenAI 兼容的本地 API，供其他客户端调用。下载页：<https://lmstudio.ai/download>。

原帖测试称，同一模型可有 Q4、Q5、Q6、Q8 等量化版本；量化通常减小模型体积和资源需求，选择时可参考 LM Studio 的资源占用提示。原帖所称 Qwen3.8 版本及其测试数据按发帖时记录，不代表今天可下载到同名版本。

## 原帖报告的性能对比
- RTX 3060 Ti、8 GB 显存运行 Qwen3.8 27B：约 3–4 token/s，作者认为日常对话等待较久。
- 另一台 4×RTX 2080 Ti、22 GB 显存机器运行 27B，并接入 DeepSeek Harness 完成简单登录页面任务：任务约 6 分 17 秒，平均约 38 token/s。原帖同时报告页面包含账号登录及三种常见第三方登录方式。
- RTX 3060 Ti、8 GB 显存运行社区模型 `Noobito45/Qwen3.8-9B-heretic-uncensored-NVFP4-GGUF` 的 Q8_0 版本，模型约 6.99 GB，约 21 token/s。作者还报告其图像识别测试能识别主要内容，并用提示词“写一个好看的登录页面，适合国内用户使用的”测试编码。

这些数据是原帖在其配置与测试条件下的报告；未提供完整软件版本、上下文长度、推理参数或重复测试统计。

## 启动本地 API 并接入 Cherry Studio
在 LM Studio Developer 页面打开 Server Settings，将端口改为未被 Windows 保留的端口；原帖例子为 `12345`，然后通过任务栏 LM Studio 图标启动服务（也可用 Server Settings 的启动按钮）。Base URL 原文为 `http:/删/localhost:12345`；按原文说明去除插入的“删”字后为 `http://localhost:12345`。

Cherry Studio 官网：<https://cherryai.com/>。在其模型服务商设置中添加 LM Studio Base URL，获取模型列表并选中已加载模型，再按模型能力配置选项。原帖称该方式成功调用本地模型。

## cpolar 远程访问
cpolar 下载页：<https://www.cpolar.com/download>；官网：<https://www.cpolar.com/>。原帖 Windows 安装后用以下命令确认安装：

```cmd
cpolar version
```

本机 Web UI 示例地址为 `http://127.0.0.1:9200`（原文插入“删”字，按文末说明删除后恢复）。在 cpolar Web UI 新建 HTTP 隧道，将本地地址指向 LM Studio 的 `12345` 端口；在线隧道列表会显示公网入口。可把 Cherry Studio 中原本的本地 API 地址替换为公网地址。随机域名可能变化；需要固定地址时，先在 cpolar 后台预留二级子域名，再将隧道类型改为二级子域名并填写预留值。

## API Token 身份验证
原帖指出 LM Studio API 默认不要求身份验证；映射公网前，可在 Developer 页面 Server Settings 启用 `Require Authentication`，再通过 `Manage Tokens` 创建 Token。若 Cherry Studio 不带 Token，原帖观察到获取模型列表失败；填写 Token 后恢复成功。MCP Server 的两个权限选项若不使用 MCP，可按原帖建议保持 `Deny`，不影响基础模型 API 调用。

输入中出现了一串看似 API Token 的示例值。为避免传播可能有效的凭据，本文不复述该字符串；实际配置应在自己的 LM Studio 中创建并保存自己的 Token。该示例不应被当作可用凭据。

## 证据边界
原帖使用的 Qwen3.8 名称可能反映发帖时的版本或命名，不能据此确认当前模型目录中的版本状态。图片未读取；这里只保留正文明确的模型标识、端口、流程和性能报告。

来源：[73612001](https://www.coolapk.com/feed/73612001)（图片与评论未读，未在本库实机复现）。

