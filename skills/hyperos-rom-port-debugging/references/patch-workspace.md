# HyperOS 移植补丁工程参考

> 本文的工程结构、配置与执行语义整理自当前另行阅读的 [OnePlus15-Port-Script README](https://raw.githubusercontent.com/yangFenTuoZi/OnePlus15-Port-Script/main/README.md)。原帖 73340364 引用了该仓库，但这次 README 阅读不能证明原帖所述 V2 移植包已包含 README 中的全部当前补丁。这里记录的是 README 的说明，不是对代码或设备行为的测试证明。本任务没有下载二进制，也没有刷机。

## 补丁目录与选择

补丁位于 `DNA_hyper/port`，按复用范围和硬件依赖分为：

- `common/<补丁名>`：共享补丁、兼容处理和公共流程，仍需检查各补丁前提。
- `features/<补丁名>`：按目标设备能力显式启用或配置的硬件特性，机型参数由组合入口提供。
- `devices/<机型>/<补丁名>`：无法抽象复用的设备专属补丁，只适用于指定设备，不能跨机型使用。

补丁默认不会全部运行。先确认所需分区已解包到工程目录，再显式传入要执行的补丁。推荐使用完整分类路径；兼容旧用法时，也可传全局唯一的补丁名。

```bash
cd DNA_hyper/port

# 只列出补丁，不执行
bash port_main.sh
bash port_main.sh list

# 显式运行补丁，按参数顺序执行
bash port_main.sh common/fix_launcher common/fix_device_identity

# 组合入口示例：一加 15 流程
bash OP15_port.sh

# 指定其他工程目录
bash port_main.sh --project-dir /path/to/project common/fix_launcher
```

`OP15_port.sh` 可以作为一加 15 的组合流程入口直接运行；`apply.sh` 是由 `port_main.sh` 管理的补丁模块，不应独立直接运行。`port_main.sh` 只导入一次 `tools/tools.sh`，随后在彼此隔离的子 Shell 加载补丁，以避免补丁的严格模式、变量、trap 或 `exit` 互相影响。

硬件专属补丁必须与目标机型匹配。README 的示例包括 `features/fix_oplus_ltpo`、`features/fix_oplus_fingerprint_protocol` 和 `devices/oneplus15/fix_refresh_rate_switch`；示例不构成对其他机型适用性的证明。

## 工程目录来源与最终分区

README 将“原包”定义为小米移植原包，将“底包”定义为当前设备原厂包。目录来源和用途如下：

| 工程目录 | 来源 | 用途 |
| --- | --- | --- |
| `odm` | 底包 `odm` | 最终 `odm` 分区工作树；保留底包 ODM，不得整体替换成原包 ODM。 |
| `vendor` | 底包 `vendor` | 最终 `vendor` 分区工作树。 |
| `product` | 原包 `product` | 最终 `product` 分区工作树。 |
| `system` | 原包 `system` | 最终 `system` 分区工作树。 |
| `system_ext` | 原包 `system_ext` | 最终 `system_ext` 分区工作树。 |
| `mi_vendor` | 原包 `vendor` | 仅为原包资源取材目录；按清单映射到最终 `vendor`。 |
| `mi_odm` | 原包 `odm` | 仅为原包资源取材目录；按清单映射到最终 `odm`。 |
| `mi_ext` | 原包 `mi_ext` | 原包独立分区；对应模块按真实目标路径合并。 |

`mi_vendor`、`mi_odm` 是区分同名原包来源的取材目录，不是最终分区名称；不得据此生成或刷入同名分区。补丁会将 contexts 中的 `/mi_vendor`、`/mi_odm` 转换为 `/vendor`、`/odm`。设备最终分区名称须与真实设备分区一致。

补丁不会自动解包 `DNA_input/mi_*.img`。使用动态来源补丁前，须先通过 D.N.A 将对应镜像解包成同名目录。文件搬运补丁还要求当前配置目录有来源分区和目标分区的 contexts、fsconfig 清单；跨分区搬运时两种清单会一起转换。缺少任一来源权限条目时，会在复制文件前失败。

## 配置目录、元数据与工具路径

`tools/tools.sh` 统一处理配置目录及 contexts、fsconfig 文件名模板。工程根目录只识别以下两套格式；两者并存时优先选择 `DNA_config`：

| 配置目录 | contexts 模板 | fsconfig 模板 |
| --- | --- | --- |
| `DNA_config` | `{part}_contexts.txt` | `{part}_fsconfig.txt` |
| `config` | `{part}_file_contexts` | `{part}_fs_config` |

`{part}` 替换为 `product`、`system` 等分区名。复杂分区元数据由 Python 3 工具 `tools/partition_metadata.py` 处理。

特殊命令行工具由 `tools/toolchain.sh` 解析。复制 `local.properties.example` 为未提交的 `local.properties` 后，可显式填入 `apktool`、`zipalign`、`avbtool`、`ddk`、`ndk` 的绝对路径。README 未给出该文件的具体键名或完整内容，因此不应臆造配置项格式。

未显式配置时，查找规则为：

- `zipalign`：从 `ANDROID_SDK`、`ANDROID_SDK_ROOT`、`ANDROID_HOME` 的 `build-tools` 中查找。
- NDK：依次检查 `NDK_HOME`、`ANDROID_NDK_HOME`、`ANDROID_NDK_ROOT`，再检查上述 SDK 下的 `ndk/`。
- 其他特殊 CLI：只使用同名 PATH 命令。

不会扫描 Snap、固定用户目录或其他工程。配置示例：

```bash
# 显式指定 SKU 附加 prop 文件名
DEVICE_IDENTITY_PROP=nezha_5.9.9.prop bash port_main.sh common/fix_device_identity

# 显式覆盖原包设备显示名
DEVICE_DISPLAY_NAME='OnePlus 15' bash port_main.sh common/fix_device_identity

# 一加 15 显示配置示例
PORT_TARGET_DISPLAY_ID=4630946903293830803 bash port_main.sh \
  common/coloros_display common/fix_boot_brightness common/fix_boot_refresh_rate
```

补丁条目按路径覆盖目标条目。contexts 匹配会忽略正则转义差异；目标文件中的重复路径会在每次修改时去重。写回 contexts 时，有效条目的字段间空白统一为单个 ASCII 空格，以兼容无法识别 Tab 分隔符的手机版 D.N.A 旧解析器。

## 身份快照

首个补丁执行前，`init_port_env` 会从尚未修改的分区树识别底包与原包设备，并把同一份身份快照交给所有下游补丁。补丁不应自行重读可能已被修改的 ODM 身份，也不应写死原包设备代号。

底包身份读取优先级：

1. `odm/etc/<ro.separate.soft>/build.default.prop`。
2. 若无该文件，仅当 `odm/etc/*/build.default.prop` 唯一时使用该文件。
3. 回退依次为 `odm/build.prop`、`odm/etc/build.prop`、`vendor/build.prop`。

原包身份读取优先级：`mi_odm/etc/build.prop`、`mi_odm/build.prop`，然后是 `product` 与 `system` 的 `build.prop`。`mi_vendor` 只标记资源来源，不参与设备代号推断。

下游使用 `PORT_BASE_DEVICE_CODE`、`PORT_BASE_DEVICE_NAME`、`PORT_BASE_DEVICE_MODEL`、`PORT_BASE_DEVICE_MARKET_NAME`，以及对应的 `PORT_SOURCE_DEVICE_*` 变量。原包机型 XML 路径由 `PORT_SOURCE_DEVICE_FEATURE_FILE` 提供。

SKU 附加 prop 不属于身份识别结果，也不会按设备代号自动猜选。只有组合入口通过 `DEVICE_IDENTITY_PROP` 明确指定文件名，才会交给 `common/fix_device_identity`。若指定文件不存在，仅输出弱警告并忽略附加配置；`mi_odm/etc/build.prop` 中的基础身份写入仍会执行。只有显式设置 `DEVICE_DISPLAY_NAME` 才覆盖 `ro.product.odm.marketname`；未设置时沿用 `mi_odm` 的基础属性或附加 prop，其他原包身份与认证字段不受影响。

## 缺失项与失败语义

可选 prop 子步骤遇到属性来源文件、目标 `build.prop`、属性清单或预期属性条目缺失时，会输出 `WARN` 并跳过该 prop 子步骤；同一补丁的 APK、XML、文件迁移和 metadata 等非 prop 主流程仍继续。明确用于替换或修补既有文件的步骤，在目标文件缺失时也只警告并跳过该替换，混合补丁中的其他可执行步骤继续。

这类缺失处理不适用于本来就要新增的 Overlay、配置、服务文件、跨分区迁移目标、生成产物或 metadata；公共复制接口仍可创建这些文件。已存在文件若格式无效、属性重复、值冲突、文件类型错误、版本或校验不受支持、内容冲突，或使用不安全符号链接，仍会失败，不会静默忽略。

传入多个补丁时按参数顺序运行。某个补丁失败会记录失败并继续后续模块，最后以非零失败状态退出。README 所述的单补丁快照恢复只覆盖该补丁的快照，不能视为整个工程的全局事务回滚。

若多个补丁复用同一 APK/JAR，补丁可按范围 source `tools/apk_patcher.sh`，调用 `apk_patcher_open`、`apk_patcher_snapshot`、`apk_patcher_rollback`、`apk_patcher_record_entry`、`apk_patcher_finalize`。公共辅助管理归档事务；Smali、资源和 DEX 的具体选择仍由调用补丁负责。README 说明 `OP15_port.sh` 会为组合流程建立共享 Settings 会话，已登记 DEX 在流程末尾只回编译、对齐并原子替换一次；单个补丁失败时恢复该补丁快照并继续后续模块。未接入该接口的补丁行为不变。这些是 README 描述的设计语义，未经本任务代码测试验证。

## 来源

- 帖子： [Coolapk 73340364](https://www.coolapk.com/feed/73340364)（帖子引用了该仓库；其 V2 包是否包含当前 README 所列全部补丁，无法由此确认）。
- 本文工程细节： [OnePlus15-Port-Script README](https://raw.githubusercontent.com/yangFenTuoZi/OnePlus15-Port-Script/main/README.md)（本次另行阅读）。
