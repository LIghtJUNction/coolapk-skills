# vivo OTA Tracker、Aerial TV 桌面、妙享远控过检与实用工具案例

> 整理自酷安公开动态正文；图片、评论及外链未逐一核验，实际操作请以对应硬件版本与上游仓库为准。

- **vivo OTA Tracker Plus 免电脑固件查询与 Payload 提取工具**
  - [酷安原帖](https://www.coolapk.com/feed/75044898)（`imgs=2`，界面未读）
  - **工具特性**：内置 14 个系列、395+ 款机型（支持代号手动输入），加解密直接在本地 JNI 库（`libvivoseckey.so`）完成，免 Root 直连 vivo 官方服务器抓取正式版/尝鲜/公测/内测通道全量包直链；可在手机端直接解析 Payload 并多选提取 boot、dtbo 等镜像分区。
  - **降级步骤与安全事项**：
    1. 查询并下载对应机型区域的完整包 zip（通常 >7GB）到手机存储，不解压；
    2. 进入系统更新 → 选择「本地安装 / 本地升级」验证并刷入；
    3. 提醒降级可能清空数据，且必须自行承担防回滚（anti-rollback）熔断风险。

- **PuddingWalls（小布丁壁纸）聚合架构与代理链路**
  - [酷安原帖](https://www.coolapk.com/feed/75040006)（`imgs=7`，界面未读，解压密码 `3mwe`）
  - **版本与权限**：APK 体积约 6.65MB，支持 Android 8.0+。图片存储路径为 `Pictures/PuddingWalls`，利用 Android 10+ 分区存储（Scoped Storage）无需外部存储读写权限。
  - **多源代理与中转链路**：
    - 聚合 Pixel 官方（206 张）、苹果官方 iOS 11~18（105 张）、Pixel 社区（369 张）、必应每日、Konachan、Wallhaven、Picsum 七大免 Key 图源；
    - 内置图库走 jsDelivr CDN 直连；缩略图及苹果 HEIC 格式图片经由 Photon 服务转码生成 540px 预览；
    - 壁纸站点 Wallhaven 与 Bing/Konachan 走国内 SaintIC 反向代理中转，完全断网时自动退回本地内置 78 张精选图。
  - **轮换机制约束**：系统 WorkManager/Alarm 限制自动轮换最短周期为 15 分钟；MIUI/ColorOS/EMUI 必须手动开启后台自启动并移除省电策略优化。

- **音乐播放器「山水有清音」v2.4 更新记录**
  - [酷安原帖](https://www.coolapk.com/feed/75035536)（`imgs=9`，界面改版未读）
  - **核心技术特性**：
    - 支持 Android 14+ USB 独占 bit-perfect 音频直通，绕过系统音频混音器直接输出原始 PCM 音频流至外接 DAC 解码器，拔出外设自动释放焦点；
    - 系统通知与胶囊岛全面适配：双向同步播放进度，适配小米超级岛/妙享背屏、vivo 原子岛、OPPO 流体云与荣耀灵动胶囊；
    - 播放与音效增强：新增 1~10 秒可调淡入淡出（默认 3 秒）；重构 8 套具备频响曲线标注的音效预设，支持自由调节音效强度与原声对比；网络歌词支持本地持久化缓存。

---

- **HosGit v2.7.6 原生鸿蒙 GitHub 客户端发布**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/75021954)（`imgs=2`，图片未读取）
  - **功能架构与特性**：
    - **原生技术栈**：纯 ArkTS + ArkUI 原生开发（非套壳网页），底栏采用鸿蒙系统 HdsTabs 悬浮玻璃胶囊设计，适配手机、折叠屏与平板分栏布局。
    - **自研 Markdown 渲染引擎**：支持语法高亮、引用、折叠块、状态徽章；自适应表格列宽与超宽横滑；支持锚点跳转，长文档限制2500行；代码页逐级浏览与图片多源降级。
    - **客户端功能**：提供 Trending 热榜、仓库/用户多条件筛选、自定义 Star 分组列表与别名、Issue/PR 筛选与评论、Releases 附件应用内断点续传下载（自带镜像加速源测速）、系统文件保存及离线 6 位 2FA 验证码功能。
    - **直达链接**：华为应用市场直达。

- **HeyTap 浏览器自定义搜索引擎与净化 Xposed 模块**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/75014475)（`imgs=3`，图片未读取）
  - **架构与模块特性**：
    - 针对 OPPO / OnePlus / Realme 设备自带的欢太浏览器（`com.heytap.browser`），基于 LSPosed 框架与 ColorOS 17（COUI 2.0）设计规范开发。
    - **功能特性**：支持自定义搜索引擎增删、排序与导入导出；离线错误页净化；通知渠道静音；深度广告拦截与页面对齐。
    - **工程优化**：开启 R8 代码混淆、仅保留 arm64 ABI 并压缩资源，将安装包体积由 60.3MB 缩减至 6.07MB。
    - **开源仓库**：GitHub `yooma1/HeytapBrowserSearchEngineManagerXposedModule`。

- **腕上导航 v0.4.6 小米手环10 Pro / 9 Pro 与高德联动更新**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/75004275)（`imgs=0`）
  - **双端架构与更新机制**：
    - 手机端 0.3.0（`NAV-PHONE-030.apk`）与手环端 0.4.6（`NAV-BAND-046.rpk`，基于快应用框架）。手环端使用 AstroBox 工具安装，安装后断开 AstroBox 并恢复小米运动健康 App 蓝牙连接。
    - **振动机制**：解析高德地图通知，在转向指令触发及临近路口时各执行两组振动；默认临近距离为30米，手机端开启“骑行临近提醒”后调整为100米；支持“轻柔”（两次短振）与“明显”（两次长振）两档。
    - **联动启闭控制**：手机端增加独立控制开关，支持打开手机 App 自动唤起手环端；支持退出手机 App 联动退出手环（锁屏不触发，切到高德算切出，需保持退出联动关闭以允许后台持续导航）。
    - **真机测试状态**：REDMI K80 至尊版（K80U）与小米手环 10 Pro 已通过基础测试；9 Pro 未做回归测试；演示原型已部署于 `wrist-navigation-seven.vercel.app`。

- **通用工具箱：安卓/鸿蒙手表免电脑无线 ADB 管理工具**
  - **来源**：[酷安原帖](https://www.coolapk.com/feed/74963459)（`imgs=1`，图片未读取）
  - **分包架构与通道支持**：
    - 安装器 1.0.0 (v24, `com.watch.choice`)、手机端 3.2.1 (`com.watch.keepalive`)、手表独立端 2.0.0 (`com.watch.keepalive.wear`)及融合安装包。
    - 通道：欧加真系手表支持专用直连；常规安卓手表走通用 ADB（无线扫码/5555端口/网段扫描）；鸿蒙手表走 HDC 协议分支。
    - **核心功能**：
      - 系统与应用管理：免电脑停用/冻结/解冻/卸载预装应用，提取安装包，修改 DPI、屏幕亮度、动画缩放；
      - 微信手表版保活：一键部署保活脚本与权限，提供温和/标准/加强三档保活强度（长效建议标准档，加强档频繁拉起可能引发微信崩溃）；
      - 提权辅助：内置一键激活 Shizuku，支持一键拉起冰箱、绿色守护、AppOps。
    - **物理限制说明**：蓝牙通道单条消息受限 <= 100KB，传文件必须开启 WiFi；手表黑屏被系统休眠时蓝牙指令会失败，需点亮屏幕后操作。

---

- **GyroBoost 6.3 陀螺仪采样率加速工具功能与 HAL 限制说明**
  - [酷安原帖](https://www.coolapk.com/feed/74906764)（`imgs=4` 未读）
  - 工具特性：GyroBoost 是一款面向安卓设备的陀螺仪采样率提升辅助工具，支持 Root 守护与免 Root 双模式。
  - 6.3 版本更新：
    1. 悬浮窗全新设计：新增刻度环、ChatGPT 图标、Claude 图标三种无底色背景的透明悬浮窗，支持 0.5x~1.2x 尺寸调节及 40%~100% 透明度调节；
    2. 支持针对 100Hz、200Hz 与 MAX 档位分别定制全局 RGB 颜色；
    3. 修复免 Root 模式下传感器正常工作但主页误显“未加速”的状态读取错误；
    4. 采样率机制说明：100Hz/200Hz/MAX 为请求档位，实际采样速度受硬件传感器与系统底层 HAL 限制；使用时需开启悬浮窗权限，并将系统电池优化设为“无限制”常驻保活。

- **WooBox NX 努比亚/红魔系统增强 LSPosed 模块**
  - [酷安原帖](https://www.coolapk.com/feed/74881063)（`imgs=2` 未读）
  - 环境与适配：基于 MyOS 14 / RedMagicOS 9（Android 14）的系统增强 LSPosed 模块，二次开发自 WooBox For RedmagicOS。
  - 核心功能特性：
    1. 屏下前摄“挖孔”定制：支持将前摄区域屏幕改为弯月、图标、剪影、纯黑或显示电量，亮色模式可用并可按应用生效；
    2. 状态栏深度调优：双击锁屏、时钟显秒、电池百分比/内外位置/充电图标开关、双排网速（字号/宽度/小数位/低速隐藏）、独立隐藏 WiFi/数据/蓝牙图标；
    3. 交互与动效修复：下拉面板行列数自由配置（竖屏/横屏/编辑模式分离）、统一通知中心与快捷设置模糊变暗动效、修复快速上滑卡片突变消失为正常淡出；
    4. 系统限制解除：全局小窗解除应用与数量限制、飞行模式保留蓝牙与 WiFi、允许截图增强、移除启动限制与系统遥测、自定义 WiFi 国家码与 MAC 地址、细化闹钟/媒体/通知/铃声/通话独立音量阶数。

- **小米妙享桌面远控 Root 设备“设备不安全”逆向与 LSPosed 拦截方案**
  - [酷安原帖](https://www.coolapk.com/feed/74875447)（`imgs=2` 未读）
  - 逆向分析场景：小米妙享桌面远程控制，未 Root 手机 A 控制已 Root 手机 B 时，主控端 A 弹窗误报“设备不安全”并强制断开连接。
  - 底层机制逆向：
    1. 并非被控端 B 自检上报，而是 A 发起 `IMiDeviceStatusService.getDeviceStatus`，向 B 发送 `event 16 GET_REMOTE_STATUS` 并启动 5 秒定时器 `LR0/e`；
    2. 被控端 B 的 MiTrustService 调用 `mrm.getStatusByCert()` 读取底层 boot 状态，回传带 mrm 签名的 `event 17 RETURN_REMOTE_STATUS` JSON 数据；
    3. 主控端 A 调用 `LR0/h.c` 进行校验，解锁 BL 必中 `unlock_l1 != unlock_l2` 规则直接判定为 1（危险），触发 `relay/x.a` 关闭远控镜像并由 `LG/b` 弹窗；因签名数据来自 TEE，Magisk 隐藏环境完全无效。
    4. MiTrustService 同时承载 `IMiRemoteAuth` 远控认证，强行停用或冻结会导致远控密码校验失败。
  - 解决方案（B 端拦截）：
    - 在被控端 B 安装 LSPosed 模块「远控反root风控-B模块.apk」，作用域勾选 `com.xiaomi.trustservice`；
    - Hook 将 `R0.h.a` 置空，阻断 B 端回传状态事件；
    - 主控端 A 触发 5 秒 fail-open 超时机制，按 `status=4`（安全）放行远控，同时保留 `IMiRemoteAuth` 正常认证。
    - 验证方式：B 端 `logcat -s R2LSP` 验证命中置空，A 端 5 秒后顺利建立远控不弹窗。附百度网盘下载链接（提取码 `17ff`）。

- **Aerial 开源 Apple TV 风格 Android TV 桌面部署实操**
  - [酷安原帖](https://www.coolapk.com/feed/74869799)（`imgs=7` 未读）
  - 项目架构：开源 Android TV 桌面 Aerial（MIT 许可，GitHub: `pengchujin/aerial-tv`，官网 `aerial.qust.me`），复刻 Apple TV 极简无广告界面与航拍屏保（支持 4K/杜比视界）。无需 Root，不精简或停用原厂系统应用。完整版支持 Android 5.0+，老设备提供 4.0–4.4 Lite 测试版。
  - 功能：底部玻璃底板常用栏与全量应用网格；手机扫码在移动端浏览器进行应用拖拽排序、重命名与换图标；支持指定开机启动特定 App 或信号源；右上角控制中心聚合信号源与网络设置。
  - 小米电视/盒子 ADB 安装步骤（以小米盒子 4 Android 6.0.1 为例）：
    1. 电视开启 ADB 调试（关于页面连按“产品型号”开启开发者模式 -> 账号与安全 -> 打开 ADB 调试）；
    2. 同 Wi-Fi 下在终端执行一键安装：
       ```sh
       curl -fsSL https://aerial.qust.me/install.sh | KEEP_ADB=1 TV=电视IP bash
       ```
    3. 按遥控器主页键直接进入 Aerial。
  - 守护与局限：小米系统休眠待机时会强制杀死第三方后台，Aerial 通过注册无障碍服务检测前台窗口作为常驻守护，待机唤醒后 30–60 秒内自动拉回前台；固件在桌面启动前播出的原生开机广告无法拦截；暂不支持 LG、三星及华为鸿蒙系统。

- **格窗工作台 (Panes) Android 15+ 多任务侧边栏 LSPosed 模块**
  - [酷安原帖](https://www.coolapk.com/feed/74753891)（`imgs=8` 未读）
  - 模块与功能：格窗工作台（Panes，包名 `io.github.yishisanren.panes`），基于 NeXtep 二次开发的 Android 15+ 多任务桌面侧边工作台 LSPosed 模块。在不更换桌面与主题的前提下，提供“一大四小”窗口同屏运行交互（支持主副窗实时交换、上滑关闭小窗、长按暂停应用省电、横屏视频置顶播放、预设工作场景、多平台同屏比价与扫一扫聚合面板）。
  - 安装前提与冲突清理：需 Android 15+、Root 与 LSPosed 环境；若此前安装过 NeXtep（`io.github.lujinxin.nextep`），必须先在 LSPosed 中停用并彻底卸载，两者不可共存。
  - LSPosed 作用域精确勾选指南：
    - 小米澎湃 OS：SystemUI、系统框架、系统桌面、智能助理；
    - OPPO / 一加 / realme（ColorOS 16/17）：SystemUI、系统框架、桌面、应用分身服务、负一屏；
    - 类原生 ROM：SystemUI、系统框架。
  - 资源：附夸克网盘下载链接（提取码 `X5gm`）及飞书反馈表。基础功能免费，部分进阶功能含 1 天试用与 6.6 元买断。
