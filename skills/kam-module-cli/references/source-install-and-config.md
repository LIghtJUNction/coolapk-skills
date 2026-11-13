# Kam：Termux 依赖、仓库安装与配置保留

## 历史安装写法

[69487288](https://www.coolapk.com/feed/69487288)补充了以下依赖清单。它是作者当时认为足够的组合，没有锁定版本，不能保证所有系统当前均可构建。

```sh
pkg install rust # cargo
pkg install cmake make perl openssl # 这些应该就够了
cargo install kam
```

另一篇[功能介绍 69318919](https://www.coolapk.com/feed/69318919)给了最简写法 `pkg install rust openssl && cargo install kam`，并列出模块创建、构建、签名、搜索、下载、安装与 MCP 扩展；不将较短命令视作否定上述额外依赖。

## 从仓库本地构建并安装

[69427561](https://www.coolapk.com/feed/69427561)提供的完整安装标识为：

```sh
kam install lightjunction/magicnet
```

原帖说明：先克隆到临时目录，进入目录调用 `kam.sh` 安装项目自定义的外部依赖，最后调用安装接口；因此可在没有 release 的情况下安装当时的 Git 版本。这不是任意 GitHub 仓库都能安装的承诺。项目地址已在其[所引用的介绍帖](https://www.coolapk.com/feed/69410665)给出：[MagicNet](https://github.com/LIghtJUNction/MagicNet)，这里没有根据安装标识单独猜仓库。

## 更新时保留配置

同帖记录 kamfw 新增按键确认指定文件是否覆盖的功能，以 `config.yaml` 为例：选择不覆盖可保留本地配置，新配置会改名加后缀保存，之后可用 `diff` 比较再手动合并；也可以选择覆盖。原帖没有给出后缀、完整路径或具体调用函数，不能补写成稳定 API。

此处保留的是当时的命令语义和流程说明；没有实际安装、调用项目钩子或覆盖文件。跨版本差异应与主技能中 0.5.x 记录分开。
