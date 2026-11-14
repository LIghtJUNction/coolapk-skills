---
name: redis-6-configuration-case
description: 整理 Flexus X 预装 Redis 加速镜像的使用信息、配置文件位置、密码设置与外部访问配置；保留文章报告的环境和验证边界。
---

# 华为云 Flexus X 实例 Redis 镜像与配置

## 镜像与实例信息

原帖报告购买了一台 **4 核、12 GB** 的 Flexus X 实例，并选择预装 Redis 加速镜像。登录服务器后运行 `redis-cli` 可进入 Redis 命令行；文章称镜像预装了经过性能优化的 **Redis 6.2**，并提及 X-Turbo 加速和智能应用调优。以上是原帖描述，未提供独立性能基准或测试数据。

Redis 配置目录为 `/etc/redis`，配置文件为 `redis.conf`。

## 修改密码

原帖称默认配置为空密码。编辑 `redis.conf`，找到 `requirepass`；若该行以 `#` 注释，去掉注释并设置密码。文章示例：

```conf
requirepass 123456
```

配置生效需要重启 Redis。原帖给出的命令为：

```sh
redis-cli shutdown
sudo systemctl restart redis
```

修改后使用新密码连接本机 Redis：

```sh
redis-cli -h 127.0.0.1 -p 6379 -a 123456
```

原帖描述，改密后空密码连接会提示无权限，并以新密码连接后进行基础 `set` 操作；未提供完整命令或执行输出。

## 配置外部访问

原帖建议在 `redis.conf` 中设置：

```conf
bind 0.0.0.0
protected-mode no
```

修改后重启 Redis。原帖没有给出安全组、防火墙规则或限制来源 IP 的配置，因此这些内容不能从该示例推定。将 Redis 监听所有网络接口并关闭保护模式会扩大可访问范围，部署时应结合网络访问控制。

## 来源与证据边界

来源：[酷安原帖](https://www.coolapk.com/feed/61924805)。正文包含 5 张图片，但图片内容未提供；本文仅整理可见文字。实例性能、镜像行为和命令执行结果均为原帖报告，未独立验证。
