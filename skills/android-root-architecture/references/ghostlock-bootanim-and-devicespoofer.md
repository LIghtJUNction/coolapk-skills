# 免解 BL 临时 Root 矩阵、春秋检测 4.5.8 逆向、开机动画路径与 DeviceSpoofer

> 整理自酷安公开动态正文；图片、评论及外链未逐一核验，实际操作请以对应硬件版本与上游仓库为准。

- #### [酷安原帖](https://www.coolapk.com/feed/74722340)（imgs=0）
  - **免解 BL 锁临时 Root 兼容性速查**：
    - **通用方案**：Linux 内核版本 5.10.149 与 5.10.233 支持一键临时 root；SPL 2026-10-01 补丁安全修补程序级别依然适用 6.1 系列（OS4）及 6.6 系列（OS4）机型。
    - **vivo / iQOO**：
      - iQOO 11 / 11 Pro / 11s：内核 5.15.178，固件 15.2.12.x / 15.2.16.1。
      - iQOO 12 / 12 Pro：OriginOS 6 (16.2.20.3) 内核 6.1.145 (8月1日补丁)。
      - iQOO Neo7 (V2231A/天玑9000+)：内核 5.10.233；Neo7 竞速内核 5.10.233/246。
      - iQOO Neo8：固件 14.1.12.3 (5.10.198) / 15.2.18.1 (5.10.233)；Neo8 Pro (16.3.12.0) 内核 5.15.197。
      - iQOO Neo9：OriginOS 4/5/6 覆盖 14.0.17.x / 15.1.14.7 (5.15.178) / 16.2.13.2。
      - iQOO 13：OriginOS 5 (15.0.24.18) 内核 6.6.57；6.6.127 内核需降级。
      - 明确支持临时 Root 机型：iQOO Z8、Z9T 长续航版、Z9T+、vivo Y300、vivo Y500、红魔 Pad。
    - **小米 / 红米**：
      - 小米 13 / 13U：内核 5.15.178 / 5.15.194。
      - 小米 15 (4.0.0.8/19)、小米 15 Pro (4.0.14/15/17)。
      - 红米 K80 Pro、红米 K80 至尊、红米 Turbo 4、红米 Turbo 5 Max 均支持临时 root。
      - 红米 K90 Ultra (OS3.309) 内核 6.6.118 (9月补丁)；K100 Pro 内核 6.12.69。
    - **OPPO / 一加 / 摩托罗拉 / 三星**：
      - 一加 Ace 2：固件 16.0.5.700，内核 5.10.236。
      - 摩托 Razr 60 Ultra (6.6.118)、Edge 60 Pro、华硕 Zenfone 12、ROG 9、Nothing Phone 3a 支持临时 root。
      - 三星 Galaxy S25 系列全系（S931x, S936x, S937x, S938x, Z Fold 7）内核 6.6.98；A56 5G (6.6.102) 与 A36 5G (6.6.46) 明确支持临时 root。

- #### [酷安原帖](https://www.coolapk.com/feed/74717557)（imgs=4 未读）
  - **提权方案与工具链**：中兴天玑 7300 机型（星云 AIOS 1.0 系统）存在 Linux 内核漏洞，可直接安装 GitHub 开源的 `ghostlock-app` (https://github.com/YuKongA/ghostlock-app/tree/main) 进行免电脑临时提权。
  - **Root 管理器与模块持久化流程**：
    - 配合 resukisu 或其他 KernelSU 分支使用。
    - 打入模块后必须在管理器中选择“软重启”（Soft Reboot），硬重启关机后漏洞提权失效需重新执行 ghostlock。
    - 实测 Zygisk 可稳定持久加载；vector 模块软重启偶发失效，但在 Scene 中再次触发软重启即可恢复挂载；成功打入 uperf 调度模块缓解发热卡顿。

- #### [酷安原帖](https://www.coolapk.com/feed/74576675)（imgs=1 未读）
  - **春秋检测 (DuckDetector) 4.5.8 逆向细节**：底层基于 ollvm + vmp 虚拟机保护，包含控制流平坦化、双向插桩与 CPU 时钟周期完整性校验。
  - **玩机工具包名黑名单（精准 68 项）**：
    - 重点检测项包含：`io.github.a13e300.ksuwebui`、`me.simpleHook`、`org.lsposed.lspatch`、`org.lsposed.manager`、`org.lsposed.lspd`、`com.tsng.hidemyapplist`、`org.frknkrc44.hma_oss`、`com.tsng.pzyhrx.hma`、`com.rifsxd.ksunext`、`com.omarea.vtools`、`com.termux`、`com.topjohnwu.magisk`、`io.github.vvb2060.magisk`、`me.weishu.kernelsu`、`com.sukisu.ultra`、`me.bmax.apatch`、`bin.*mt.*plus`、`com.reveny.nativecheck` 等。
  - **OEM Verified Bootkey 信任根（共 51 枚）**：
    - 覆盖 Google Pixel 1-9/Fold、小米 10-14/HyperOS、华为 Mate/P 系列、荣耀 Magic、三星 Knox Vault、OPPO/一加/真我统一 AVB 2.0 PKMD、vivo、魅族、索尼、华硕等主流官方证书。
    - **直接封杀的黑名单公钥**：AOSP 通用自签证书、TWRP 官方自签证书、OrangeFox 自签证书、GrapheneOS 官方签名证书、LineageOS 官方签名证书。

- #### [酷安原帖](https://www.coolapk.com/feed/74167834)（imgs=0）
  - **Android 通用开机动画加载优先级 (1-10 级)**：
    1. `/data/local/bootanimation.zip`（最高优先级）
    2. `/product/media/bootanimation.zip`
    3. `/my_product/media/bootanimation.zip`
    4. `/oem/media/bootanimation.zip`
    5. `/system/customize/resource/`
    6. `/system/media/bootanimation.zip`（通用后备路径）
    7. `/apex/com.android.bootanimation/etc/`
    8. `/vendor/media/bootanimation.zip`
    9. `/system/product/media/bootanimation.zip`
    10. `/system/system_ext/media/bootanimation.zip`
  - **各大定制系统特殊路径与音视频文件规范**：
    - **联发科 MTK 三阶段启动**：第一屏为 `logo.bin` 分区；第二屏为 `/system/media/images/boot_logo`；第三屏为常规 `bootanimation.zip`。
    - **三星**：传统 QMG 格式，开机动画为 `/system/media/bootsamsung.qmg` 与循环 `bootsamsungloop.qmg`；关机为 `shutdown.qmg`；第一屏位于 `up_param` 分区。
    - **欧加系 (OPPO/一加/realme)**：核心位于 `/my_product/media/bootanimation.zip`，迈凯伦定制位于 `/mnt/vendor/op1/bootanimation/`，关机动画支持 `rbootanimation.zip`，视频启动为 `/data/local/bootvideo.ts`。
    - **小米/红米**：支持视频动画 `/data/local/bootvideo.ts` 与 `/data/local/misc/boot_advideo.ts`；开机音频位于 `/system/media/bootaudio.mp3` 或 `/data/system/theme/boots/`；支持系统属性 `ro.third.bootvideo.path`。
    - **vivo / iQOO**：专属视频动画配置 `CONFIG/POWERONOFF.mp4`，位于 `/system/etc/bootvideo/boot.mp4` 或 `/data/local/bootvideo.ts`。
    - **HarmonyOS NEXT**：位于 `/sys_prod/etc/bootanimation/`，包含 `cust_bootpic.zip`、`cust_bootsound.wav`、`cust_bootvideo.mp4` 与 `bootanimation_custom_config.json`。

- #### [酷安原帖](https://www.coolapk.com/feed/74164979)（imgs=0）
  - **开源项目与架构**：Magisk-DeviceSpoofer (https://github.com/L0NE-6/Magisk-DeviceSpoofer)，纯本地 Shell 脚本，无联网无遥测，机型数据同步自 KHwang9883/MobileModels（CC BY-NC-SA 4.0），涵盖 26 个品牌 3548 个型号。
  - **工作机制与防回写设计**：
    - 属性修改：通过 resetprop 批量覆盖 `ro.product.*`（涵盖 generic 及 system、system_ext、vendor、product、odm、bootimage、vendor_dlkm、odm_dlkm 分区属性）。
    - 阶段控制：`post-fs-data.sh` 在 Zygote 启动前写入，保证系统服务及 `Build.*` 读取新值；`service.sh` 在开机后进行二次校验写入，阻止 OEM 定制服务回写还原。
  - **全系统兼容分支与验证命令**：
    - Android 6-17：通过 Magisk / KernelSU / APatch 模块本地刷入。
    - Android 2.2-5.x：CWM/TWRP 刷入同一 zip 自动检测并直接修改 `/system/build.prop`（自动备份为 `build.prop.xj_bak`）。
    - Android 1.x-5.x：提供 `legacy/xj_apply.sh` 与 `xj_restore.sh` 手动执行。
    - 生效验证命令：`getprop ro.product.model` 及 `getprop ro.product.marketname`。
