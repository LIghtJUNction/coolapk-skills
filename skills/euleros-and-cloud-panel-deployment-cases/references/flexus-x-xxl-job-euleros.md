# Huawei Cloud EulerOS 上部署 XXL-JOB 的实验记录

## 环境与平台机制

原帖将 Flexus X 描述为可按需配置资源、可选性能模式的云服务器，并在 Huawei Cloud EulerOS 环境中尝试部署 XXL-JOB。XXL-JOB 采用中心化结构：调度中心负责任务管理和触发调度，执行器负责执行任务；支持固定速率和 Cron 等调度模式。

原文没有给出实例的具体规格、EulerOS 版本、XXL-JOB 版本、仓库地址或部署端口，因此这些信息无法据此复现。文中称优惠券获取了配置额度，属于作者的使用背景，不是性能测试结论。

## 调度中心依赖

原帖列出的环境要求为 JDK 1.8+、Maven 3+、MySQL 5.7+。安装 OpenJDK 的步骤：

```sh
yum -y list java*
yum install -y java-1.8.0-openjdk.x86_64
java -version
```

先查询 yum 源中可用的 Java 包，再安装并用 `java -version` 查看版本。原帖未提供 Maven 安装步骤。

## MySQL 安装与配置记录

原帖报告，直接运行 `yum install mysql` 时遇到缺少 client 和 lib 的问题，随后改为手动下载 MySQL Community Server 的 RPM 组件并上传服务器。原帖指向 MySQL Community Server 下载页，但未提供可恢复的具体 URL 或 MySQL 版本。所选平台是 Huawei Cloud EulerOS，原帖建议选择相应 Linux 发行版的软件包。

RPM 安装顺序为 common、libs、client、server：

```sh
rpm -ivh 包名
rpm -qa | grep mysql
systemctl status mysqld
systemctl start mysqld
```

安装后用 `rpm -qa | grep mysql` 检查组件；`mysqld` 初始可能未启动，使用 `systemctl start mysqld` 启动并查看 `systemctl status mysqld` 状态。

原帖先称已在云服务器安全组放行 TCP 3306，随后进行数据库密码及远程访问配置。其密码初始化步骤记录如下：

1. 查询日志中的初始密码：`cat /var/log/mysqld.log | grep password`。
2. 原帖选择在 `/etc/my.cnf` 添加 `skip-grant-tables`，重启后免密码登录；登录后切换至 `mysql` 库并查询：

   ```sql
   select user, host, authentication_string from user;
   ```
3. 原帖给出的密码修改语句为：

   ```sql
   update user set authentication_string=password('<new_password>') where user='用户';
   flush privileges;
   ```

   `<new_password>` 是占位写法；原帖说明实际密码填在引号内，不包含尖括号。完成后删除 `skip-grant-tables` 配置并重启：

   ```sh
   systemctl restart mysqld
   ```
4. 原帖报告远程登录时又被要求修改密码，于是重复修改并刷新权限；若提示密码策略过严，原帖使用：

   ```sql
   set global validate_password_policy=0;
   set global validate_password_length=1;
   ```
5. 为允许远程连接，原帖通过 SQL 将 root 的 host 改为 `%`，再刷新权限：

   ```sql
   use mysql;
   update user set host='%' where user='root';
   flush privileges;
   ```

原帖中的 MySQL SQL 和验证策略参数应视为该次实验记录；文中未写明 MySQL 的精确版本，不能据此保证这些语句适用于其他版本。`skip-grant-tables`、弱密码策略、对外开放 3306，以及允许 root 从任意主机连接，都会显著降低数据库访问保护。原帖把安全组开放与数据库远程访问配置作为两项操作，二者都需要满足外部连接条件。

## XXL-JOB 部署步骤

原帖描述的流程是：

1. 下载对应版本的源码，解压后导入 IDEA。
2. 将源码中的 SQL 初始化文件导入数据库。原帖列出这些表及用途：
   - `xxl_job_lock`：任务调度锁表。
   - `xxl_job_group`：执行器信息。
   - `xxl_job_info`：调度任务扩展信息，如任务分组、任务名、机器地址、执行器、执行入参和报警邮件等。
   - `xxl_job_log`：调度及执行历史信息。
   - `xxl_job_logglue`：GLUE 更新历史，用于支持版本回溯。
   - `xxl_job_registry`：在线执行器和调度中心地址注册信息。
   - `xxl_job_user`：系统用户。
3. 配置 `xxl-job-admin` 调度中心的 MySQL 连接；如需邮件通知，再配置相应邮件参数。
4. 在 IDEA 中使用 Maven 打包成 JAR，上传服务器并以守护进程方式运行。原帖未给出守护进程命令、配置文件路径、JAR 参数、调度中心访问地址或登录凭据。
5. 使用官方示例项目构建并启动执行器，在调度中心配置执行器，新增任务、填写 Cron 和必要设置，选择立即执行或启动任务，再查看日志。

## 实验结果与证据边界

作者报告单次执行和按 Cron 执行均在日志列表中触发，执行器日志也有打印。该结果是原帖的实验报告；未提供部署版本、任务配置截图内容或可独立复核的日志。作者还报告 yum 安装路径因缺少 client/lib 不顺，手动 RPM 安装后完成；调度中心曾因未配置访问端口而无法访问，补充端口设置后解决。具体端口值没有提供。原帖未附可恢复的项目下载链接；图片内容未读取。

来源：[61925025](https://www.coolapk.com/feed/61925025)（图片与评论未读，未在本库实机复现）。

