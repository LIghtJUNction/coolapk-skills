# 飞牛 NAS 部署 Gitea 并通过 cpolar 远程克隆

## 适用范围与证据边界
原帖以飞牛 NAS、Docker 和 RK3566 ARM 设备为例，设备约 4 GB 内存、8 GB eMMC，另可接 SATA 硬盘。正文报告 Gitea 初始化、仓库迁移及公网 HTTPS 克隆成功；没有独立复测。图片未读取，评论未提供。

## Gitea 能力与部署
Gitea 是轻量级自托管 Git 平台，支持公开/私有仓库、在线浏览、提交记录、分支、Issue、Pull Request、Wiki、用户权限管理及 SSH/HTTP 克隆。原帖称可从 GitHub、Gitee、GitLab 迁移仓库。

项目仓库：<https://github.com/go-gitea/gitea>

在飞牛 Docker 镜像仓库搜索 `gitea/gitea`，标签使用 `latest`。创建容器时设置开机自动启动；主机端口映射中将容器 SSH 的 `22` 映射到主机 `2222`，避免与主机 SSH 端口冲突；Web 端口 `3000` 可保持默认或自行修改。初始化页面数据库默认 SQLite3；设置站点名称，并将 Gitea SSH 服务端口设置为 `2222`，然后立即安装。原帖指出第一个注册用户为管理员。

创建仓库可在页面右上角“+”菜单操作。迁移外部仓库时选择“迁移外部仓库”，填写来源仓库克隆地址并等待迁移完成。原帖演示先迁移仓库，再创建 `test` 仓库。

## cpolar 公网访问和 Git 克隆
启用飞牛 SSH 后连接 NAS 并切换 root：

```sh
ssh rk3566@192.168.50.212
sudo -i
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

`https://get.cpolar.sh` 按原帖明示规则移除了插入的“删”字。Web UI 示例为 `http://192.168.50.212:9200`。编辑或新建 HTTP 隧道，将目标本地端口设为 Gitea Web 端口 `3000`，原帖地区示例为 `China VIP`。随机域名可用于测试；长期使用需先预留固定二级子域名，再将隧道域名类型改为二级子域名。

原帖示例以 HTTPS 固定公网地址克隆仓库，命令中 `删` 字按原帖说明移除：

```sh
git clone https://gitea01.vip.cpolar.cn/admin/hp-prodesk-600-g4-efi.git
```

该命令依赖原帖示例的固定域名与仓库路径，实际使用应从自己的 Gitea 仓库页复制克隆地址。原帖报告 Windows 终端克隆成功。

## 来源
- 原帖：<https://www.coolapk.com/feed/72821369>
- Gitea 项目：<https://github.com/go-gitea/gitea>
- cpolar 安装脚本：<https://get.cpolar.sh>（按原帖明确规则删除插入字后恢复）
- 原帖示例仓库克隆地址：<https://gitea01.vip.cpolar.cn/admin/hp-prodesk-600-g4-efi.git>
