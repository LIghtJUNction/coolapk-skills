# 用宝塔 Docker 部署 ThingsBoard

## 环境与安全组

原帖描述在华为云 Flexus X 实例上通过宝塔的 Docker 菜单创建容器，实例系统版本、Docker 版本和宝塔版本未说明。作者称为方便曾开放所有端口，并明确不建议生产环境这样做。部署时应依据实际服务需求限制安全组入站端口；这是原帖同时给出的安全提示。

## 创建数据与日志目录

原帖在宝塔终端中执行：

```bash
mkdir -p ~/.mytb-data && sudo chown -R 799:799 ~/.mytb-data
mkdir -p ~/.mytb-logs && sudo chown -R 799:799 ~/.mytb-logs
```

这会创建用户家目录下的 `.mytb-data`、`.mytb-logs`，并将目录所有者及所属组递归改为 UID/GID 799。实际路径取决于运行终端的用户；若通过 root 执行，`~` 通常展开为 `/root`。

## 容器配置

原帖给出的 Compose 配置：

```yaml
version: '3.0'
services:
  mytb:
    restart: always
    image: "thingsboard/tb-postgres"
    ports:
      - "8080:9090"
      - "1883:1883"
      - "7070:7070"
      - "5683-5688:5683-5688/udp"
    environment:
      TB_QUEUE_TYPE: in-memory
    volumes:
      - ~/.mytb-data:/data
      - ~/.mytb-logs:/var/log/thingsboard
```

配置使用 `thingsboard/tb-postgres`，容器重启策略为 `always`，队列类型为 `in-memory`。端口映射为主机 8080 到容器 9090、1883 到 1883、7070 到 7070，以及 UDP 主机端口段 5683–5688 映射到容器同段。数据和日志分别挂载至容器 `/data` 与 `/var/log/thingsboard`。

原帖称镜像拉取较慢，等待后容器创建成功；这是作者报告，未提供耗时或运行检查结果。

## 访问与登录信息缺口

原帖给出的访问地址为 `http://ip:8080`。登录说明中用户名位置显示为 `sysadmin@thingsboard.org`，密码显示为 `sysadmin`。用户名显然是被替换的占位内容，无法据此恢复真实默认账号；不能将该占位符作为可用登录名。由于原帖公开了默认密码文本，部署后应检查并按应用支持方式更改默认凭据。

## 资源与证据边界

华为云 Flexus X 产品页：<https://www.huaweicloud.com/product/flexus-x.html>

宝塔官网：<https://www.bt.cn/new/index.html>

原帖提及宝塔安装脚本，但正文地址被“查看链接”替代；输入外链提取为 `https://download.bt.cn/install/install_lts.sh;if`，包含疑似命令残片，不能当成已确认脚本直链。原帖促销页：<https://activity.huaweicloud.com/828_promotion/index.html>。这些链接分别是产品、面板官网、疑似脚本上下文及活动页，不能相互替代。

来源：酷安文章《华为云Flexus X实例部署开源物联网平台ThingsBoard》，https://www.coolapk.com/feed/61691422 。文章含 6 张图片，图片未读取；页面截图和作者报告的“负载 0%”未能核对。
