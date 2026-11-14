---
name: conda-python-environment-case
description: 参考 Linux 上 Anaconda 创建与激活独立 Python 环境的历史案例，并识别安装路径和版本前提。
---

# Linux Conda 独立环境案例

[来源文章](https://www.coolapk.com/feed/61931426)使用 Linux x86-64 的 Anaconda 安装脚本，在安装完成后创建名为 `pytorch` 的 Python 3.9 环境：

```sh
conda create --name pytorch python=3.9
conda activate pytorch
python --version
```

`pytorch` 是环境名称，上述命令只指定 Python 版本，不会自动安装 PyTorch。激活命令依赖当前 shell 已配置 conda；应按所用 conda 版本的实际安装路径和 shell 初始化方式处理，不能照搬来源中的 `/root/anaconda3/bin`。原帖对安装脚本使用 `chmod 777` 并以 root 安装，这不是运行脚本的必要条件；避免给安装文件开放所有用户写权限。

原帖使用 `Anaconda3-2024.06-1-Linux-x86_64.sh`，未提供安装包直接链接或校验结果。示例是历史版本，安装前需根据当前平台核对官方发行包、架构、完整性及许可；不假定示例版本是最新版本。文章图片未读取，命令未在本库独立运行。
