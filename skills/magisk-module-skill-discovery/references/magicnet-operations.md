# MagicNet

MagicNet 是面向 KernelSU、APatch、Magisk 的网络规则过滤模块。它使用域名和 IP 规则拦截目标访问，并支持 Mihomo 默认 WebUI 与 YACD。原帖称项目重新维护、上游规则仓库已更新，GeoX 数据条目有增改，旧模块已不可用。原帖没有提供版本号或发布包下载链接，因此无法据此确定当前版本或复现某个发布包。

项目与相关公开资源：

- MagicNet 源码：https://github.com/LIghtJUNction/MagicNet
- MagicMihomo 配置与规则项目：https://github.com/LIghtJUNction/MagicMihomo
- 示例封禁规则文件：https://github.com/LIghtJUNction/MagicMihomo/blob/main/ruleset/magicnet/ban.yaml
- kamfw 工具函数库：https://github.com/MemDeco-WG/kamfw
- kamfw 的 Mihomo 脚本示例：https://github.com/MemDeco-WG/kamfw/blob/main/__mihomo__.sh

## 网络过滤与 TUN 路径

模块将 TUN 设备路径链接到某些软件使用的 `/dev/net/tun` 路径，以提高路径兼容性。原帖给出的命令如下：

```sh
mkdir -p /dev/net
[ ! -L /dev/net/tun ] && ln -s /dev/tun /dev/net/tun
file /dev/tun
```

原帖示例中，`file /dev/tun` 输出为 `character special (10/200)`。这是字符设备的场景说明；设备节点和权限情况取决于具体系统环境。这个命令只建立路径链接，不创建 TUN 字符设备，也不启用内核功能。判断条件只检查目标是否是软链接；若已有同名普通文件或目录，`ln -s` 仍可能失败。没有 Android 的 VPN 图标也不能作为流量未被转发的证明。

## 构建和安装

原帖建议在终端中安装，以便显示构建进度，也提到可以从管理器安装。使用 kam 构建并安装本地包：

```sh
kam build && kam install dist/MagicNet.zip
```

原帖提到未来上传至 KSU 模块仓库后，计划使用以下命令；当时 `-S` 仅用于下载，作者说后续才考虑整合为直接安装，所以这不是已确认可用的安装流程：

```sh
kam -S MagicNet && kam install MagicNet.zip
```

另一个帖子给出的 Termux 安装 kam 方法：

```sh
pkg install rust openssl && cargo install kam
```

这个最简安装写法与后文较完整依赖清单分开记录，不将它当作所有构建环境的充分依赖。

原帖列出的 kam 功能包括模块创建、构建、签名、搜索、下载和安装；另有 kam MCP 服务器扩展。帖子未给 kam 版本、仓库地址或安装后验证步骤。

## 常见操作

- 结束 Mihomo 进程：`pkill -f mihomo`
- 也可以使用模块的 `action.sh` 按钮操作。
- 手机开启热点后代理效果异常时，原帖建议结束进程，或在 WebUI 中关闭 TUN。
- 共享代理时，在 WebUI 配置中启用允许局域网访问，查看设备局域网 IP 与代理端口，再在客户端代理设置中填写 `IP:端口`。具体端口以当前配置界面为准。
- 原帖示例环境变量：

```sh
export HTTP_PROXY=IP:7890
```

其中 `IP` 和端口应替换为配置界面显示的值。原帖还提到存在其他代理环境变量，但没有列出名称和配置方式。

## 规则与配置注意事项

原帖建议检查 `/data/adb/modules/MagicNet/.config/mihomo/config.yaml` 中的 `*****-provider` 配置组；如果填写了 `*****-provider.my*****.url`，应确认 URL 来自正规渠道。字段中含有星号遮蔽内容，不能据此推断完整键名或配置格式。

原帖称规则集通过子模块定期同步，并称构建时使用最新 Mihomo 版本；未给出对应版本号、同步频率或可复核的构建记录。作者还说明 release 使用其 KSU 开发者私钥签名；这不构成对签名密钥真实性或某个安装包的独立验证。

## kamfw 与管理器适配

kamfw 是面向模块开发者的开源工具函数库，原帖称其函数按需加载，并用于处理不同 Root 管理器的配置存储差异。帖子指出 `magisk.sh`、`ksu.sh`、`ap.sh` 中相关函数名称保持一致，由 kamfw 按管理器加载适配实现。作者欢迎代码审查与兼容性贡献。原帖称参考了 nga-utils，但未提供该项目链接或具体引用清单。

## 信息边界

原帖的实际 href 已列在资源部分；图片和外链实现本轮未读取，不能从中补出完整配置或额外操作步骤。规则贡献与配置格式可查看上列公开项目；实际安装前应以项目发布页和设备当前管理器支持情况为准。
## 后续配置与发布修复

[配置更新帖 69488259](https://www.coolapk.com/feed/69488259)记录：`config.yaml` 利用 YAML 锚点减少重复，支持8个链接槽，分组按国家/地区组织；基本组选择最优，其他组可手动选择地区，并有快速选择组。作者称调整 DNS 以减少请求，没有给出配置差异和测量。

该帖先写3个付费、5个免费槽，后面又称默认8个免费，两处表述不一致；只保留“8槽且预填供初始化使用”，不推断预设服务的资费或可信度。原帖建议自行替换预设，并把组切到直连；这不是对预设质量的验证。

[修复帖 69440942](https://www.coolapk.com/feed/69440942)称 release 构建未初始化子模块，导致两个目录为空，另有 `action.sh` 切换 Mihomo 状态的逻辑错误，二者当时已修复。没有目录名称、修复提交或版本号，不能由此保证当前任一包已包含修复。

[更新帖 69427561](https://www.coolapk.com/feed/69427561)另称修正了上游路径变化造成的 `bing` rule-set 地址，并调整匹配顺序；没有给出新旧 URL，不补造规则下载直链。

来源：[69410665](https://www.coolapk.com/feed/69410665)、[69318919](https://www.coolapk.com/feed/69318919)及上列更新帖。安装、设备网络、进程操作和签名均未在本库执行或验收。不同版本的功能不混用。
