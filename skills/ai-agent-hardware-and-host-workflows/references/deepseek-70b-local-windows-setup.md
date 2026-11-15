# Windows 本地部署 DeepSeek R1 70B 与手动导入 GGUF

## 模型规模与示例机器

原帖解释：参数规模中的 B 表示 billion（十亿），70B 即约 700 亿参数。参数规模增加通常会提高模型能力，也会增加对硬件资源的要求。文中使用的是蒸馏版 DeepSeek R1 70B，并报告其体验接近在线 671B 版本；这是发帖者的主观使用评价，不是独立对比测试。

示例主机为 Windows 11 23H2、Intel Core i9-14900K、技嘉 Aorus Z790 冰雕、RTX 4080 16GB、64GB DDR5-6000 内存及致态 TiPro9000 1TB SSD。原帖称 Ollama 官方对 16GB 显存显卡推荐 32B，但作者实测也能运行 70B，速度较慢，需 CPU 与 GPU 协同（原帖对此标注存疑）。该配置下报告生成速度约 2 token/s，模型约 42GB，载入约十多秒；这些是单机报告值，不代表其他机器的表现。

## Ollama 在线拉取

安装 Windows 版 Ollama 后保持其后台运行。原帖给出的命令为：

```cmd
ollama run deepseek-r1:70b
```

首次执行会拉取并运行模型；原帖称模型默认存放在 C 盘。该命令也可在后续启动已下载模型时使用。

资源：
- Ollama 官网：https://ollama.com（原帖以“ollama点com”给出域名，补全 HTTPS 为推断）
- Ollama GitHub 项目：https://github.com/ollama/ollama（原帖仅称可从 GitHub 下载，具体仓库链接为按项目名补全的推断）
- DeepSeek R1 的 Ollama 模型页：原帖指引在 Ollama 官网的 models 中选择 deepseek-r1；未提供具体页面 URL。

## Chatbox 图形界面

原帖步骤：安装 Chatbox 对应版本；启动后选择“使用自己的 API Key 或本地模型”；模型提供方选 `OLLAMA API`，模型选 `deepseek-r1:70b`。上下文消息数量上限可设为不限制，Temperature 建议设为 0.6，再保存。

资源：
- Chatbox 中文官网：https://chatboxai.app/zh（原帖将域名和路径写作“chatboxai.app斜杠zh”，补全 HTTPS 为推断）
- Chatbox GitHub：原帖称可从 GitHub 下载，但未给出仓库链接。

## 离线导入 GGUF 模型

原帖以 7B 模型演示，说明其他大小可按相同步骤操作。先在 Windows 高级系统设置的环境变量中新建 `OLLAMA_MODELS`，变量值设为模型目录路径，例如 `D:\deepseekmodel`；并在该目录创建 `Modelfile`。需显示文件扩展名并移除 `.txt` 后缀。到 ModelScope 搜索目标 DeepSeek 模型并使用关键词 `GGUF`，下载所需 GGUF 文件放入目录。

示例文件路径及 Modelfile 内容（保持原帖路径）：

```text
FROM D:\deepseekmodel\DeepSeek-R1-Distill-Qwen-7B-Q4_K_M.gguf
```

`FROM` 后是模型文件完整路径，路径需与实际位置一致；原帖提醒从文件地址复制时删除引号，并在 `FROM` 后留空格。然后在命令提示符或终端执行：

```cmd
ollama create DeepSeek-R1-7B -f D:\deepseekmodel\Modelfile
```

原帖称完成后可在 `D:\deepseekmodel\blobs` 查看文件，并用以下命令确认模型列表：

```cmd
ollama list
```

之后可在 Chatbox 中通过 Ollama API 使用本地模型。原文没有给出 ModelScope 的具体模型页或下载直链，因此无法据此确定推荐的 GGUF 文件、量化版本或发布者。

资源：ModelScope 官网域名为 `modelscope.cn`（原帖写作“modelscope点cn”；补全 HTTPS 为推断）。

## 存储与显卡经验

原帖称模型文件可能从十几 GB 到数十、上百 GB，加载和使用会产生大量磁盘读取，SSD 性能会影响载入与文件操作体验。其 TiPro9000 PCIe 5.0 SSD 标称最高顺序读取 14,000 MB/s、2TB 版本最高随机读取 2,000K IOPS；这些是原帖所述规格和体验，不是本文独立验证结果。作者还推荐不支持 PCIe 5.0 时考虑高端 PCIe 4.0 SSD，并举 TiPlus7100 为例。

显卡选购部分是原帖在发布时的建议：16GB 显存中端卡可考虑 4070 Ti Super 16G；作者称 70B 可运行但速度较慢，主要由 CPU 与 GPU 协同处理（原帖存疑）。5090 被描述为可获得更快推理并运行更大模型。以上均为历史建议，不代表当前市场价格或所有模型量化版本的兼容保证。

## 证据边界

来源是 2025 年发布的单篇文章，包含 9 张图片；图片内容未提供，性能图、界面截图及图片中的具体设置无法读取。原帖未提供评论、下载直链或 GGUF 模型页面。文中的硬件表现和稳定性属于作者报告，未独立复测。

来源：[Coolapk 帖子 62691565](https://www.coolapk.com/feed/62691565)
