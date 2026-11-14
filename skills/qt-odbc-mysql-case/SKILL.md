---
name: qt-odbc-mysql-case
description: 整理原帖的 Windows ODBC 数据源配置、Qt QODBC 示例、ANSI/Unicode 区别及 Linux 驱动安装记录。
---

# Windows 与 Qt 使用 ODBC 连接 MySQL

[来源文章](https://www.coolapk.com/feed/61925246)记录了以下历史操作与配置（图片未读，未在本库实机复现）。

原帖以在本机建立 ODBC 数据源并由 Qt 访问 MySQL 为例，另记录在 CentOS 上安装 MySQL ODBC 驱动、再配置云端数据库数据源的步骤。文中没有提供完整 Qt 工程、驱动版本或可复现的连接环境。

## Windows 创建 ODBC 数据源

1. 从 MySQL 官方 Connector/ODBC 下载页选择 64 位安装包并安装。原帖没有提供该下载页的 URL 或具体版本。
2. 在 Windows 控制台进入 `C:\Windows\SysWOW64` 并启动 ODBC 数据源管理程序：

```bat
cd \Windows\SysWOW64
odbcad32.exe
```

3. 添加 ODBC ANSI 驱动并创建用户数据源。原帖特别指出 **Data Source Name**（DSN）后续要填入 Qt 的 `setDatabaseName()`。
4. 在 ODBC 管理器中使用 Test 测试连接。原帖没有给出测试结果截图内容。

## Qt QODBC 连接示例

```cpp
connect(ui->pushButton_2,&QPushButton::clicked,this,[=](){
    // 使用ODBC驱动
    QSqlDatabase db = QSqlDatabase::addDatabase("QODBC");
    db.setHostName("127.0.0.1"); // 云数据库的主机地址
    db.setPort(3306); // 端口
    db.setDatabaseName("mydatasource"); // 数据库名称
    db.setUserName("root"); // 用户名
    db.setPassword("123"); // 密码
    bool ok = db.open();
    qDebug() << ok;
    if (!ok) {
        qDebug() << "Database error occurred:" << db.lastError().text();
    }
    qDebug() << QSqlDatabase::drivers();
});
```

示例将 DSN 写为 `mydatasource`，用户名为 `root`，密码为示例字符串 `123`，端口为 3306。`setHostName("127.0.0.1")` 的注释却写“云数据库的主机地址”，二者含义不一致；如连接远端数据库，应按实际服务器地址配置，原帖没有给出可直接使用的云端地址。不要将示例密码当成真实凭据。

## ANSI 与 Unicode 驱动说明

原帖的概括如下：

- ANSI ODBC 使用系统默认字符编码，函数名通常以 `A` 结尾（如 `SQLConnectA`），字符串参数使用 `char`；适用于只需 ASCII 或相应单字节/双字节字符集的应用，也可能用于兼容旧系统。
- Unicode ODBC 使用宽字符编码（原帖举 UTF-16），函数通常以 `W` 结尾（如 `SQLConnectW`），字符串使用 `wchar_t`；适合多语言和多字符集需求。

实际编码行为还取决于驱动、系统和应用的构建配置；原帖未给出这些条件。

## Linux/CentOS 安装驱动

原帖给出的安装命令：

```sh
sudo yum install mysql-connector-odbc
```

若遇到仓库 GPG 检查报错，原帖记录了两种尝试：编辑 `/etc/yum.repos.d/epel.repo` 将 `check` 改为 `0`，但作者称未生效；以及运行：

```sh
sudo yum install mysql-connector-odbc --nogpgcheck
```

作者称加入 `--nogpgcheck` 后可安装成功。关闭 GPG 检查会跳过包签名校验，应谨慎使用；该记录没有提供仓库配置内容、错误日志或安装包版本。

安装后，原帖的流程是先在 ODBC 连接管理器中建立指向华为云 MySQL 的连接，再由 Qt 通过 ODBC 使用该数据源。输入材料没有提供 Linux 连接管理器的具体配置命令、云数据库地址或连接测试结果。

## 证据边界

本文依据单篇原帖整理。原帖图片未读取，故安装界面、ODBC 测试界面及未以文本提供的设置均不作补充；评论未提供。代码中的用户名与密码是原帖示例值，不应视为实际账户信息。
