---
name: fedora-nabu-kde-vulkan-recovery
description: 排查 nabu 上 Fedora 43 KDE 更新后黑屏，按原帖安装 Vulkan 组件并区分包检查与图形会话验收。
---

# Fedora 43 KDE 黑屏且背光亮：检查 Vulkan 组件

## 适用范围

针对原帖记录的 Fedora 43 KDE（43.0-20260417-1049，nabu）更新后黑屏、背光仍亮、无法进入 KDE 桌面的情况。原帖作者判断与缺少 Vulkan 组件有关，但没有提供日志佐证；这不能视为已确认的普遍原因。

## 在已挂载的系统中安装组件

如果已通过安卓端工具挂载 Linux 分区，并能进入原帖所用的挂载路径，可运行：

```sh
chroot /data/mounted/linux
dnf install vulkan-loader mesa-vulkan-drivers
```

原帖没有提供分区挂载或设备节点操作命令，因此这里不补写这些步骤。

新装 Fedora KDE 或准备升级系统时，原帖建议先安装两项组件，再更新：

```sh
sudo dnf install vulkan-loader mesa-vulkan-drivers
sudo dnf upgrade
```

## 检查安装情况

```sh
rpm -qa | grep vulkan
ls -l /usr/lib64/libvulkan.so.1
```

原帖列出的正常包名还包括 `vulkan-tools`，但前述安装命令没有安装它，不能据此保证该包存在。`libvulkan.so.1` 文件存在只说明该路径下有库文件，不代表 KWin、Wayland 或 GPU 渲染已经正常工作。

## 来源

[酷安技术帖](https://www.coolapk.com/feed/72841871)
