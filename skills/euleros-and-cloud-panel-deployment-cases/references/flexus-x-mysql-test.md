# Huawei Cloud EulerOS 上的 MySQL 安装与读写测试记录

## 系统识别与安装尝试

原帖使用 `lsb_release -a` 查看镜像，并报告系统为 Huawei Cloud EulerOS（HCE OS，基于 openEuler 构建）。作者先尝试：

```sh
dnf install mysql5-server mysql5-common mysql5
```

该命令在原帖中未成功，随后改用：

```sh
yum install mysql
```

作者报告后者安装效果符合预期，但具体安装包版本、仓库配置及安装输出图片未包含在可读文本中。

## 启动、登录和账号配置

```sh
systemctl start mysqld
systemctl status mysqld
mysql -u root -p
```

原帖报告初次连接时数据库未设置密码。随后在 MySQL 客户端执行：

```sql
use mysql;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
flush privileges;
exit;
```

`root` 是原文示例密码，不适合作为生产环境密码。原帖随后以 `mysql -u root -p` 重新连接并输入 `root`。

## 外网连接配置

原帖先在云安全组中开放 TCP 3306，再尝试使用 Navicat 连接；首次连接失败后，将本地 root 账号的 host 从 `localhost` 改为 `%`：

```sql
use mysql;
update user set host='%' where user='root' and host='localhost';
flush privileges;
exit;
```

作者报告之后 Navicat 连接成功。该操作会允许 root 从任意来源主机匹配登录，属于高风险示例；原帖没有描述来源 IP 限制、TLS、专用账号或防火墙规则。文章中的成功报告不构成安全配置建议。

## 建表与 Python 测试

原帖称使用一份 7767 条数据的 CSV，字段为 `label`（int，1 表示正向、0 表示负向）与 `review`（text，评论内容），文件名 `ChnSentiCorp_htl_all.csv`。建表 SQL：

```sql
CREATE TABLE `test_table` (
  `id` int NOT NULL AUTO_INCREMENT,
  `label` int NOT NULL,
  `review` TEXT NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

批量写入脚本原文如下：

```python
import mysql.connector
import time
import csv

# 数据库连接信息
config = {
  'user': 'root',
  'password': 'root',
  'host': 'ip 地址',
  'database': 'sql_rc_test',
  'raise_on_warnings': True
}

# 创建连接
creatConnector = mysql.connector.connect(**config)

# 获取游标
cursor = creatConnector.cursor()

# 开始时间
start_time = time.time()

file_path = 'ChnSentiCorp_htl_all.csv'
# 读取csv 数据, 并将数据插入到数据库
with open(file_path, mode='r', encoding='utf-8') as csv_file:
    reader = csv.DictReader(csv_file)
    rows = list(reader)  # 将CSV行转换为列表，以便批量插入

# 批量执行INSERT语句
query = "INSERT INTO test_table (label,review) VALUES (%s , %s)"
cursor.executemany(query, [(row['label'], row['review']) for row in rows])

# 提交事务
creatConnector.commit()

# 关闭游标
cursor.close()

# 关闭连接
creatConnector.close()

# 结束时间
end_time = time.time()

# 打印所用时间
print(f"总时间: {end_time - start_time} 秒")
```

此写法将整个 CSV 先读入内存，再用 `executemany()` 批量插入。原帖报告写入耗时结果，但数值只在未读取的图片中，故此处不补填。

读取测试代码：

```python
import mysql.connector
import time

# 数据库连接信息
config = {
  'user': 'root',
  'password': 'root',
  'host': 'ip 地址',
  'database': 'sql_rc_test',
  'raise_on_warnings': True
}

# 创建连接
cnx = mysql.connector.connect(**config)

# 获取游标
cursor = cnx.cursor()

# 开始时间
start_time = time.time()

# 假设我们有一个特定的ID，例如表中最大的ID
rand_id = 7766  # 替换为表中最大的ID
query = "SELECT * FROM test_table WHERE id = %s"
cursor.execute(query, (rand_id,))

# 结束时间
end_time = time.time()

# 总时间
total_time = end_time - start_time

# 打印查询结果（如果有的话）
result = cursor.fetchone()
if result:
    print(f"查询ID {rand_id} 的结果: {result}")

# 打印查询时间
print(f"查询ID {rand_id} 的总时间: {total_time} 秒")

# 关闭游标和连接
cursor.close()
cnx.close()
```

代码中的变量名为 `rand_id`，其值固定为 7766；SQL 执行和计时在 `fetchone()` 之前，因此计时不包括取回结果的耗时。原帖称性能表现突出，但没有提供可读的计时数值、对照机器配置、MySQL 版本或重复次数，不能形成可比较的性能结论。

## 来源与证据边界

来源：酷安 feed 61658695。文中指向的前文链接为 https://ruocheng.blog.csdn.net/article/details/141748572?spm=1001.2014.3001.5502。图片未读取，评论未提供；测试结果数值缺失。示例连接配置含占位主机名及弱密码，不能直接作为生产配置。

来源：[61658695](https://www.coolapk.com/feed/61658695)（图片与评论未读，未在本库实机复现）。

