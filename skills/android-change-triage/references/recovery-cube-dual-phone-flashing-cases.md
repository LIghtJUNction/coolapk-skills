# 保数据恢复、第三方 Recovery、免电脑线刷与临时提权实验记录

本记录整理自 5 篇社区刷机与恢复案例，均未在本库实机复现，操作前需核对对应机型与分区状态。

## 1. `/data/system/cube` 空壳保数据恢复（红魔 11 Pro+ 示例）[74874951](https://www.coolapk.com/feed/74874951)

1. 从当前机型救砖包/系统包中解压出完整 `super.img`（原帖示例约 `18G` 左右；理论亦支持解包单独刷入 `vendor.img`）。
2. 使用文件管理器检查 `/data/system/cube` 目录状态并尝试删除。
3. 通过支持分区刷写的第三方 Recovery（或对应工具）刷入完整 `super.img` 后重启，再检查 `/data/system/cube` 是否恢复内容。
4. 若仍为空或无法删除，在 Root 终端执行去除不可变属性并删除目录后重启：
   ```sh
   su
   chattr -R -i /data/system/cube
   rm -rf /data/system/cube
   ```

## 2. 小米 17 Pro 第三方 Recovery（`skyward-Pandora`）[74964400](https://www.coolapk.com/feed/74964400)

- 修复内容：修复 `data` 解密问题，修复刷写完整包后重启到 Recovery 分区自动合并更新。
- 测试边界：原帖仅在 HyperOS `4.0.6.0` 下测试通过；原帖提供密码 `972m`，仓库与下载直链未入库。

## 3. `Kernel Flasher` / `bootflasher` 与双手机免电脑线刷 [55160996](https://www.coolapk.com/feed/55160996)、[74102348](https://www.coolapk.com/feed/74102348)

- **Boot 提取与修补**：可从系统包提取、用 `bootflasher`（需 Root）提取，再用 Magisk / Alpha / APatch / Kernel Flasher（旧版刷 AK3 或原厂 boot，新版区分 LKM 与 GKI）修补；已修改内核切回时需先 `fastboot flash` 刷回原厂 `boot.img`。
- **免电脑双手机刷入**：准备双 C 口数据线，在主控手机使用免 Root 工具箱（如 Bugjaeger Premium `fastboot flash` 或甲壳虫 ADB，或 Termux）连接处于 Fastboot 模式的待刷手机刷写镜像。

## 4. `CVE-64560` 手机端临时提权载荷实验条件记录 [74088525](https://www.coolapk.com/feed/74088525)

- 原帖（标注 `#一加Ace6# #ColorOS17#`）将其描述为概率性本地提权载荷（非稳定方案，可能触发内核恐慌导致重启），并记录可能受影响的因素：锁屏（影响 ADB）、机身温度、高性能模式、无线调试；在 ColorOS 17 上若 Shizuku/ADB 授权受限，原帖测试将系统语言切为英文并在开发者选项开启 `Disable system optimization` 可缓解。
