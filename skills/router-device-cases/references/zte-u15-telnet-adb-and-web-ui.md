# 中兴 U15 随身 Wi-Fi 开启 ADB 与增强 Web 后台替换

> 整理自酷安公开动态正文；图片、评论及外链未逐一核验，实际操作请以对应硬件版本与上游仓库为准。

- **中兴 U15 随身 Wi-Fi 开启 ADB 与高级后台替换**
  - [酷安原帖](https://www.coolapk.com/feed/75026977)（`imgs=5`，后台截图未读）
  - **前提条件**：设备已开启 Telnet 调试服务。
  - **Telnet 开启 ADB 实操步骤**：
    1. 终端执行 `telnet 192.168.0.1 5364` 连接设备；
    2. 登录输入用户名 `admin`，密码 `admin` 进入底层 shell；
    3. 查看当前参数 `nv show | grep -i adbd`（默认状态为 `adbd_enable=0`）；
    4. 依次执行配置命令开启并保存：
       ```sh
       nv set adbd_enable=1
       nv save
       reboot
       ```
    5. 若重启未生效，手动拔插电源冷重启。
  - **高级 Web 后台与备份资源**：
    - U15 优化后台下载：蓝奏云，密码 `d2pb`（新增 AT 命令终端、锁频段设置及 IMEI 修改功能）；
    - U15 官方原版后台备份下载：蓝奏云，密码 `hkte`；
    - 刷入说明：使用 UFI_TOOL 工具或直接传输替换系统的 web 目录生效。
