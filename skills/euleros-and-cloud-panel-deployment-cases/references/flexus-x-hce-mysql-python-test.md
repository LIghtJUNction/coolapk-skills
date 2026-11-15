# Huawei Cloud EulerOS 上安装 MySQL 与 Python 读写测试记录

本记录整理自在华为云 Flexus X（4 vCPUs、14 GiB 内存，默认 Huawei Cloud EulerOS 公共镜像）上安装 MySQL 并使用 Python `mysql.connector` 进行写入与查询测试的社区过程；原帖含 7 张图片（未读取），未在本库实机复现。

## 1. 安装与初始化 MySQL

原帖记录执行 `yum install mysql` 后发现缺少 `mysql-client`，随后安装与启动服务：

```bash
yum install mysql
systemctl start mysqld
systemctl status mysqld
mysql -u root -p
```

登录 MySQL 后调整 `root` 认证插件为 `mysql_native_password`（原帖示例密码为 `root`，实际部署应使用强密码）：

```sql
USE mysql;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
```

## 2. Python 读写测试脚本要点

原帖使用 `mysql.connector` 连接测试库 `mysql_insert_select_test`，对表 `test_table` 执行批量/单条写入与按 `id` 查询：

```python
import mysql.connector

config = {
    "user": "root",
    "password": "<your_password>",
    "host": "<your_ip>",
    "database": "mysql_insert_select_test",
    "raise_on_warnings": True,
}
cnx = mysql.connector.connect(**config)
# 写入测试语句：
# INSERT INTO test_table (userName, createDate, pwd, age, introduce) VALUES (%s, %s, %s, %s, %s)
# 读取测试语句：
# SELECT * FROM test_table WHERE id = %s
```

## 证据边界

- 原帖未在正文中给出完整的建库/建表 DDL、`mysql-connector-python` 安装命令及最终吞吐耗时数值（部分测试结果可能在 7 张未读图片中）。
- 来源：[Coolapk 帖子 61920723](https://www.coolapk.com/feed/61920723)。
