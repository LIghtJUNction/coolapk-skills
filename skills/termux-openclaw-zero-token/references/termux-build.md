# 在 Android Termux 上构建和运行 OpenClaw Zero Token

OpenClaw Zero Token 是 OpenClaw 的一个分支。来源帖称其目标是免除 API Token 费用并免费访问各大 AI 平台；这属于项目目标描述，不代表其中各平台的访问方式或费用已独立验证。

## 公开项目与镜像

- 项目仓库：https://github.com/linuxhsj/openclaw-zero-token
- pnpm registry 镜像：https://registry.npmmirror.com
- 相关问题讨论：https://github.com/linuxhsj/openclaw-zero-token/issues/224

## 前置条件

- Android 设备上安装 Chrome。
- 在 Termux 中安装 `android-tools`，并且设备支持网络调试。

## 安装与构建

在 Termux 中依次执行：

```sh
pkg install android-tools
git clone https://github.com/linuxhsj/openclaw-zero-token
npm install -g pnpm
pnpm config set registry https://registry.npmmirror.com
cd openclaw-zero-token
pnpm install --ignore-scripts
NODE_OPTIONS="--max-old-space-size=4096" pnpm build
NODE_OPTIONS="--max-old-space-size=4096" pnpm ui:build
```

来源帖说明，`/matrix-sdk-crypto-nodejs` 不支持 Android，而此处不需要 Matrix 聊天加密，因此安装依赖时使用 `pnpm install --ignore-scripts` 跳过依赖脚本。构建命令通过 `NODE_OPTIONS="--max-old-space-size=4096"` 将 Node.js 堆内存上限设为 4096 MB（4 GB），帖中称此设置用于避免构建时因内存不足崩溃。

## 配对设备、启动与配置模型

1. 在 Termux 中用 `adb pair` 配对设备，再用 `adb connect` 连接设备。来源未提供这两个命令的具体参数或示例。
2. 执行 `./start-chrome-debug.sh`，启动 Chrome 调试模式。
3. 执行 `./onboard.sh webauth`，启动向导。
4. 执行 `./server.sh`，使用 Gateway 服务管理脚本。
5. 来源帖要求下载附件并替换原始文件，随后确保文件具有可执行权限（`chmod +x`）再执行。HTML 锚点把附件入口明确指向 https://github.com/linuxhsj/openclaw-zero-token/issues/224 。尚未读取该 issue 或下载附件，具体附件名、替换文件名和内容未知。
6. 在浏览器对话框中使用 `/models` 查看模型提供商。按来源说明，只有通过 `./onboard.sh webauth` 配置的模型提供商会写入 `openclaw.json` 并显示在 `/models` 中。
7. 使用 `/model` 切换模型；来源给出的示例为：

   ```text
   /model deepseek-web/deepseek-chat
   ```

来源记录的是一篇操作说明，没有附构建日志、设备型号或实际运行结果；以上命令与适用条件是帖子明确给出的步骤，不能据此推断已在所有 Android 设备上测试成功。

来源：https://www.coolapk.com/feed/71556916 。原帖4张图片未读。命令中的仓库和registry地址按同一原文HTML锚点恢复；未执行安装或构建。
