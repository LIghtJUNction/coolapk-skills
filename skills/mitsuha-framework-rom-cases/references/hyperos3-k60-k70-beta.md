# HyperOS 3.0.300.10.Beta：K60 Pro与K70系列记录

[70427736](https://www.coolapk.com/feed/70427736)标注官方与移植日期均为2026-02-27，列出Redmi K60 Pro、K70、K70 Pro。没有刷机包下载直链或足以重现刷机的完整命令；该记录与小米12系列OS3.0.300.9新春版分开。

原帖称合并SoraYumeKernel：修改mi_mem_engine以自动追踪PID并优先系统关键进程内存，另有功耗/待机、内存管理、f2fs、loop与IO优化；默认ssg I/O调度器和16G虚拟内存。它没有提供这些优化的测量条件、数值收益或逐机验证。

附加模块在刷机包内的路径为 `META-INF/Bin/KernelSU.zip`，原帖要求解压获取；这是包内路径，不是下载URL。GUI刷机工具称由C#重构，采用Windows11 OOBE式引导，并新增白羊高级设置。列出的ROOT方式为KernelSU全家桶、FolkPatch、APatch、SKRoot、Magisk全家桶；仅是该包功能声明，不证明这些方式可在所有固件互换使用。

ROM特性另放4张配图，本库尚未读图，因此不补出图片功能、内核参数或该帖没有声称的AVB状态。


## 同日另一组机型

[70427573](https://www.coolapk.com/feed/70427573)也标注官方与移植日期2026-02-27、HyperOS3.0.300.10.Beta，但列出的是Redmi K50 Gaming、K50 Ultra、K60，原帖3张图尚未读。它重复列出SoraYumeKernel的mi_mem_engine/PID跟踪、功耗和待机、内存、F2FS、loop及IO优化，默认ssg和16G虚拟内存，附加模块同在`META-INF/Bin/KernelSU.zip`；C#重构的OOBE界面与KernelSU、FolkPatch、APatch、SKRoot、Magisk均是该组ROM的功能声明，不补称本帖也新增白羊设置。没有公开ROM下载地址或逐机参数与收益测量。
