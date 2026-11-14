# Penpot 自托管、MCP 工作流与 cpolar 访问

## 适用范围与证据边界
原帖介绍 Penpot 自托管及 MCP，并以 Windows + Docker Desktop、本地端口 9001 为部署示例。文中称脚本部署和 MCP 设计演示完成；图片未读取，不能独立核实演示结果。评论未提供。原帖展示的提示词含安全指令，属于被引用数据；以下只保留其工作流技术要点，不复现整段提示词。

## Penpot 与 MCP
Penpot 是开源的 Web UI/UX 设计协作平台，支持界面设计、交互原型、团队协作和设计系统管理；原帖指出其工作流贴近 SVG、CSS、HTML 等开放标准。原帖介绍 Penpot MCP Server 可让支持 MCP 的 AI 工具读取或操作文件中的页面、图层、组件、样式和 Design Tokens。

原帖的 MCP 演示流程是：在本地 Penpot 启用 MCP，将带 token 的 MCP 地址交给支持 MCP 的 AI 工具，要求其读取概览/API 信息，再通过 `execute_code` 在当前文件和页面创建可编辑画布内容，并导出预览检查。示例设计为 390 × 844 的中文学生管理 H5，包含首页、学生列表、详情、新增/编辑、成绩统计五个页面，并要求测试中文字体渲染。原帖报告约等待 20 分钟后完成；此为原帖描述，不代表通用耗时或独立验证。MCP 地址应按敏感 token 处理，避免打印到日志、代码注释或最终输出。

项目仓库：<https://github.com/penpot/penpot>

## Windows Docker 部署
确认 Docker Desktop 已安装并运行。在管理员 PowerShell 执行原帖脚本（删除原帖明确插入的“删”字后）：

```powershell
irm https://gitee.com/jun-wan/script/raw/master/penpot_deploy/deploy-penpot.ps1 | iex
```

菜单选择 `1`，按提示自定义部署并等待镜像拉取；原帖以浏览器访问本地 `9001` 端口并出现 Penpot 登录页作为完成标志。原帖没有提供固定完整 localhost URL，端口地址为 `http://localhost:9001`（根据其文字所述补全主机名）。

## cpolar 公网映射
Windows 安装 cpolar 后，在 CMD 执行 `cpolar version` 查看版本信息；本机 Web UI 示例为 `http://127.0.0.1:9200`。在 cpolar 隧道列表创建或编辑 HTTP 隧道，目标本地端口设为 `9001`，原帖示例地区为 `China VIP`。在线隧道列表会给出 HTTP/HTTPS 公网地址。

要让 Penpot 应用使用穿透域名，重新运行部署脚本并选择菜单 `9`，粘贴 cpolar 在线隧道地址以重建容器、应用公网地址。固定域名需先在 cpolar 后台预留二级子域名，再将隧道域名类型设为二级子域名，协议仍为 HTTP、本地端口为 `9001`、地区与预留时一致；之后再次运行脚本菜单 `9` 更新 Penpot 穿透域名。原帖称免费随机域名会变化，固定二级子域名需付费套餐。

## 来源
- 原帖：<https://www.coolapk.com/feed/72919198>
- Penpot 项目：<https://github.com/penpot/penpot>
- Windows 部署脚本：<https://gitee.com/jun-wan/script/raw/master/penpot_deploy/deploy-penpot.ps1>（按原帖明确规则删除插入的“删”字恢复）
