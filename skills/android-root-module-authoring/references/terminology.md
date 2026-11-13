# Root 与系统术语：保留可核对含义

来源：[术语原帖 59941224](https://www.coolapk.com/feed/59941224)。下表保留技术缩写，不把词形联想、读音和玩笑当成项目方命名说明。

| 缩写 | 展开与含义 |
| --- | --- |
| LKM | Loadable Kernel Module，可加载内核模块 |
| GKI | Generic Kernel Image，通用内核镜像 |
| KMI | Kernel Module Interface，内核模块接口 |

这里的内核模块与模块包中的 `customize.sh`、`system/`、WebUI 等不是同一层事物，不能因为同称“模块”就互换安装。名字含 GKI/KMI 也不等于已证明任意内核与模块兼容。

`/usr` 的目录用途应与个人主目录分开：它容纳程序、库和共享资料等，个人主目录通常位于 `/home`。参见 [Linux hier(7) 手册](https://man7.org/linux/man-pages/man7/hier.7.html)。原帖将“Unix System Resources”说成最早词源，但未提供出处，本文不采纳这一历史断言。

Magisk、LSPosed、Xposed、Android、Git 等名字的语音拆解和创始人故事没有在原帖给出依据，不整理成正式词源。可将这些当作非正式联想，但它们不能解释技术接口或替代文档。
