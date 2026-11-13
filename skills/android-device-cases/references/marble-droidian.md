# marble：Droidian 的适配与功能核对

## 身份与范围

[项目适配仓库](https://github.com/droidian-marble/adaptation-xiaomi-marble)将 `xiaomi-marble` 对应到 POCO F5 / Redmi Note 12 Turbo。按代号匹配资料后，仍需核对具体硬件、当前固件和准备使用的制品。

[社区文字](https://www.coolapk.com/feed/74340899)报告使用 `5.10.236` 内核、HyperOS 3 起始环境和指定 DFE NEO 包，并涉及清数据、Recovery、sideload 与 boot 镜像处理。依赖的 Recovery 选项在文字中不全，不能把这份简述拼成完整刷机脚本。

## 功能验收

2026-11-13 查阅的项目 README 将指纹、自动亮度和双 SIM 列为不可用，其中双 SIM 注明已禁用。社区对相机、网络、NFC 等功能的概括不是逐项验收；应采用项目当前功能表，分别核对所需项目。

社区还报告需要重启 ofono 与 ModemManager 才能使用单卡。缺少日志时不能认定两个服务的重启顺序就是所有设备的通用修复。

## 待完成

尚未下载并校验镜像、执行安装或测试功能；图片未读。安装前需要补齐项目当前说明、原始系统恢复材料、完整的包版本和文件校验。记录中的项目状态是阅读时快照，后续使用要重新核对。
