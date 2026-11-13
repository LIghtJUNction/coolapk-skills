---
name: shizuku-accounts-hail-management
description: 按授权和启动方式使用 Shizuku、Accounts 与 Hail 管理可访问的系统账户与同步，处理重启和应用冻结的权限关系。
---

# 使用 Shizuku 配合 Accounts 管理系统账户

## 用途与条件

原帖将 Accounts 描述为依赖 Shizuku 授权的系统账户管理工具：可查看账户，并尝试查看同步项、管理同步或删除账户。原帖没有给出 Accounts 的包名、版本或下载地址；这些功能属于原帖报告，不能据此保证所有账户类型或所有设备都可访问、可修改。

Shizuku 为应用提供由 ADB 启动的服务能力，也可以在已 root 的设备上启动服务。它不是直接把应用 UID 改成 system。Accounts 的修改操作需要在 Shizuku 中授权；仅安装 Shizuku 并不等于已授权。

## 基本使用流程

1. 安装可信来源提供的 Accounts、Shizuku；若需要冻结应用，再单独安装 Hail（雹）。原帖没有提供这些应用的下载链接或版本信息。
2. 启动 Shizuku 服务。可按设备条件选择 ADB 或 root 启动方式，具体设置参考 Shizuku 官方指南。
3. 在 Shizuku 的授权界面中向 Accounts 授权。
4. 打开 Accounts 查看可访问的系统账户及其同步项。若账户显示或操作不可用，不能据此断定所有账户都支持管理。
5. 只有确认目标账户确为要处理的残留账户后，才执行删除或关闭同步等操作。删除系统账户可能影响同步，并可能要求相关应用重新登录。
6. 若还要用 Hail 冻结应用，需按 Hail 自身的权限流程另行授权，并确认设备上该冻结模式受支持。Accounts 的授权不会自动授予 Hail 权限。

## 重启与权限维护

手机重启后 Shizuku 服务会停止。原帖建议按非 root 方式在重启后重新启动服务；若设备实际使用 root 方式，应按 Shizuku 对应的启动方式操作。可按需允许 Shizuku 后台运行和通知，以减少服务被系统停止的情况，但不同系统的行为可能不同。

## 账户安全注意

- 先核实账户类型和所属应用，再决定是否删除；不要把清理残留账户理解为可以无差别删除系统账户。
- 原帖只列举 Google、厂商账号和应用同步账户等类型，没有提供账户身份或 token，也不能据此推断具体账户内容。
- 原帖称可先用 Accounts 清理，再用 Hail 冻结相关应用以减少账户再次生成；这不是保证所有应用都不会重建账户。

## 官方资料

Shizuku 设置指南：https://shizuku.rikka.app/guide/setup/

## 来源

- Shizuku 官方设置指南：https://shizuku.rikka.app/guide/setup/
- 原帖：[酷安技术帖](https://www.coolapk.com/feed/73662073)
