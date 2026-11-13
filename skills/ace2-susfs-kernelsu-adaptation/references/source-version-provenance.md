# 内核版本号与源码变更证据

[74647660](https://www.coolapk.com/feed/74647660)讨论了只修改 Makefile 版本号、合并上游、引入功能以及AI辅助开发的区别。此处只提炼版本核对方法，不将原帖对其他项目的评价当作已核实事件。

[Linux 顶层 Makefile](https://github.com/torvalds/linux/blob/master/Makefile)中有 `VERSION`、`PATCHLEVEL`、`SUBLEVEL`、`EXTRAVERSION`。修改这些字段并不能证明已合入对应上游代码。判断版本升级时，应分别记录上游基线、实际修复提交或等效回移、设备补丁及最终构建配置；是否由AI生成或CI编译同样不能替代这些证据。

[Linux stable规则](https://www.kernel.org/doc/html/latest/process/stable-kernel-rules.html)要求修复或等效修复已有于主线，并要求正确且经过测试；回移的补丁也可能为适配老系列而调整。因而提交哈希不同不能单独证明修复缺失，版本字符串相同也不能单独证明修复齐全。

整理时将以下三类结论分开：源码是否包含相关改动，配置是否启用该路径，实际运行制品是否表现出预期行为。仅见susfs或FAS名字、功能宣传或更新日志，不足以证明适配完整、性能提升或所有机型稳定。原帖未提供具体可核对仓库，不能据此补造仓库或移植步骤。
