# 开源 Magisk 模块开发器与模块开发简明指南

项目仓库：[RootManage-Module-Model](https://github.com/LIghtJUNction/RootManage-Module-Model)

## 使用 GitHub Actions 发布和更新模块

1. 注册或登录 GitHub 账号，并 fork 项目仓库。
2. 点击 **Actions**，运行 `init` 工作流。
3. 在 `MyModule` 文件夹中编写模块。切换仓库分支可以查看作者制作的模块实例。
4. 更新 `changelog`，记录所做的改动。
5. 运行 `release` 工作流进行打包发布。

如果仓库中有超过 100 MB 的大文件，需要自行下载并使用 Git LFS 上传。作者提到，仓库包含大文件时现在也能正常打包。

## 模块安装脚本

指南建议不要使用 `install.sh`，改用 `customize.sh`。运行 `init` 工作流时，`module.prop` 已经设置好。模块根目录还需放置 `META-INF` 文件夹；如果不打算支持 Magisk 用户，可以不放。

### 检查设备架构

建议在 `customize.sh` 开头检查架构。原帖示例：

```sh
# 检查设备架构
case "$ARCH" in
    "arm")
        ui_print "设备架构为 ARM 32位"
        abort "不支持32位设备架构: $ARCH"
        ;;
    "arm64")
        ui_print "设备架构为 ARM 64位"
        ;;
    "x86")
        ui_print "设备架构为 x86 32位"
        abort "不支持32位设备架构: $ARCH"
        ;;
    "x64")
        ui_print "设备架构为 x86 64位"
        ;;
    *)
        abort "不支持的设备架构: $ARCH"
        ;;
esac
```

`abort` 会中断安装流程、报错并退出。示例只接受 `arm64` 和 `x64`，对 `arm`、`x86` 及其他值中止安装。

### 检查 Root 管理器

原帖示例根据 `$KSU`、`$APATCH` 判断管理器，并在 Magisk 分支重命名脚本：

```sh
if [ "$KSU" = "true" ]; then
  ui_print "- kernelSU version: $KSU_VER ($KSU_VER_CODE)"
  touch $MODPATH/ksu
  echo $KSU_VER > $MODPATH/ksu
# 意思是如果是在模块根目录新建ksu文件并把kernelsu的版本号写入文件
elif [ "$APATCH" = "true" ]; then
  APATCH_VER=$(cat "/data/adb/ap/version")
  ui_print "- APatch version: $APATCH_VER"
  ui_print "- KERNEL_VERSION: $KERNEL_VERSION"
  ui_print "- KERNELPATCH_VERSION: $KERNELPATCH_VERSION"
  touch $MODPATH/apatch
  echo $APATCH_VER > $MODPATH/apatch
else
  ui_print "- Magisk version: $MAGISK_VER ($MAGISK_VER_CODE)"
  mv $MODPATH/boot-completed.sh $MODPATH/service.sh
  touch $MODPATH/magisk
  echo $MAGISK_VER > $MODPATH/magisk
fi
```

原帖将 `$MODPATH` 解释为 `/data/adb/modules/$moduleid/`，也就是模块目录。示例会在模块目录创建 `ksu`、`apatch` 或 `magisk` 文件，并写入相应管理器版本；Magisk 分支还把 `boot-completed.sh` 改名为 `service.sh`。代码按原帖保留，未补充缺省文件处理或路径引用。

安装时可以设置目录和文件权限：

```sh
set_perm_recursive $MODPATH 0 0 0755 0755
```

原帖建议随后重启手机。文中称 KernelSU 新增了 `boot-completed.sh` 脚本类型，该脚本会在开机完成后运行；安装时会按 Root 管理器重命名，在 Magisk 下改为 `service.sh`。

原帖还给出如下等待系统启动完成的片段：

```sh
if [ -f $MODDIR/magisk ]; then
    echo "检测到 Magisk，等待系统开机完毕..."
    if [ -f $MODDIR/boot_completed.sh ]; then
        mv $MODDIR/boot_completed.sh $MODDIR/service.sh
    fi
    # 等待系统开机完毕
    while [ "$(getprop sys.boot_completed)" != "1" ]; do
        sleep 1
    done
fi
```

原帖未解释 `$MODDIR` 与前文 `$MODPATH` 的关系，也未说明 `boot-completed.sh` 与此处 `boot_completed.sh`（下划线写法）的对应关系；这里按原文保留，不推断或更正。等待之后的逻辑由模块作者自行编写。

指南总结：针对不同管理器的调整放在 `customize.sh` 中，对 `$MODPATH` 下的文件进行操作；目录权限可在安装时通过 `set_perm_recursive` 设置。更多细节请参考 KernelSU 官方文档：https://kernelsu.org/zh_CN/guide/module.html 。

## overlayfs 快速介绍

模块可以把文件放在模块根目录的对应系统路径下，例如：

```text
模块根目录/system/bin/某个可执行二进制文件
```

原帖举例：下载静态编译的 Linux 版 `yq` 二进制文件，重命名为 `yq` 并放入模块。刷入模块并重启后，可在终端使用 `yq` 命令。原帖将此描述为利用 overlayfs 让该文件等效地出现在只读文件系统中；其他 `/system/` 下的目录也可采用类似方式。将应用转为系统应用的示例思路，是在对应目录叠加应用安装包文件。具体实现可参考相关模块；原帖没有提供该类模块的链接。

## webroot 文件夹

原帖称 webroot 是 KernelSU 首先推出的功能，Magisk 可借助其他工具扩展，APatch 也应当支持。`webroot` 文件夹内必须包含 `index.html`。原帖给出的最简示例为：

```html
<!DOCTYPE html>
<script>
    document.location = 'http://127.0.0.1:1234'
</script>
</html>
```

该页面将浏览器跳转到 `http://127.0.0.1:1234`，原帖说明目标可以是本地地址或在线地址，也可以在 `webroot` 中放置完整前端项目，再由 KernelSU 打开。

## module.prop

模块必须有 `module.prop` 文件，否则无法标识为模块。原帖称项目的 `init` 工作流已设置该文件。

## 原帖排版说明

原帖中的 shell 片段以换行符排版，已用代码块呈现；`webroot` 示例也按 HTML 代码块呈现。原文的版本文件命名、变量名和脚本名存在差异，以上均按原文保留，不能据此确定哪一种拼写应当统一。


## 协调核查：旧示例的适用边界

- 原链接目前经 GitHub 重定向到 [MemDeco-WG/RootModuleMaker](https://github.com/MemDeco-WG/RootModuleMaker)，公开 API 确认该仓库已归档。上文 `init`、`MyModule`、`release` 是原帖的历史流程，不代表当前默认分支仍能照做。本次未运行任何工作流。
- 安装器的 `$MODPATH` 是本次安装目标路径，不应强行替换成固定的已安装模块目录。运行时 `$MODDIR` 也不能凭变量名假定存在；示例没有初始化它。
- `boot-completed.sh` 与 `boot_completed.sh` 写法不同。原文的安装重命名与后续运行时检查也未形成完整一致流程，不能默默统一后声称已验证。
- `else` 分支假定未知环境就是 Magisk，没有独立确认。`set_perm_recursive ... 0755 0755` 则把普通文件也设为可执行；这是示例行为，不是所有模块的统一权限要求。
- 系统目录映射不等于总使用同一种挂载后端；当前 KernelSU 系统文件修改依赖 metamodule，Magisk 的 magic mount 机制不同。见 [KernelSU 官方模块指南](https://kernelsu.org/guide/module.html)与 [Magisk 开发指南](https://topjohnwu.github.io/Magisk/guides.html)。
- WebUI 跳转到 localhost 不会启动对应服务，也没有提供服务、鉴权或桥接代码。原文“APatch 应当支持”是推测，不是已验证的兼容矩阵。

仅整理已读正文；图片、评论与完整仓库实现未读取。Shell 片段只做本地语法与架构分支检查，没有在手机安装模块。
