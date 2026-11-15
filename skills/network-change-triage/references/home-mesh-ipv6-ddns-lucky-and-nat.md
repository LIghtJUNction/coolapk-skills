# 家庭 Mesh/IPv6/NAS 外网访问、Lucky 守护、NAT 测试与建站加速

本参考整理酷安社区中关于原生 IPv6 访问 NAS、Android Root 下 Lucky 进程守护、家庭宽带与 Mesh 规划、RFC3489 vs RFC5780 NAT 类型测试差异、星空组网 6.0.6、博客静态加速与深澜校园网研究边界的记录。

---

## 1. 无公网 IPv4 下的原生 IPv6 外网访问 NAS 与 Android Lucky 守护

- **原生 IPv6 外网访问 NAS**（https://www.coolapk.com/feed/69655704）：光猫改桥接后，在路由器中开启 IPv6 并设为 `Native` 模式、关闭防火墙；在 NAS 网络页确认获取以 `24` 等公网前缀开头（而非 `fe80` 链路本地）的 IPv6 地址后，配合 DDNS 即可实现高速外网直连。
- **Android Root 下 `lucky_2.26.2` 守护脚本**（https://www.coolapk.com/feed/71412265）：将 `lucky_2.26.2_Linux_arm64_wanji.tar.gz` 解压至 `/data/lucky/`（含 `conf`/`tmp`），在 `/data/adb/service.d/lucky.sh`（`755` 权限）中配置 `sleep 30` 等待系统就绪，每 30 秒通过 `/system/bin/pidof lucky` 检查并在缺失时以 `TMPDIR=/data/lucky/tmp /data/lucky/lucky -c /data/lucky/conf/` 拉起。

---

## 2. 家庭宽带规划、Mesh 组网与路由器替换实践

- **家庭宽带与全屋 Wi-Fi 规划**（https://www.coolapk.com/feed/69274393）：不建议盲目办理高资费 FTTR（设备多为 AX/BE3000 级别且隐形光纤后期个人难维护），建议每个房间预留网口；5GHz Wi-Fi 良好体验在 2 堵墙内、2.4GHz 在 3 堵墙内，与其高价买单台旗舰不如多台路由器有线/Mesh 覆盖。
- **家庭网络改造实例**（https://www.coolapk.com/feed/73348785、https://www.coolapk.com/feed/73042382、https://www.coolapk.com/feed/72518414）：120㎡ 承重墙隔离场景通过 `TP-LINK XDR5480 + XDR3030` Mesh 解决小房间信号衰减；红米 AX3000 电源/硬件故障（亮几十秒后全灭滋滋响）后替换为小米 BE6500。

---

## 3. NAT 类型测试（RFC3489 vs RFC5780）、星空组网 6.0.6 与建站/校园网研究

- **RFC3489 vs RFC5780 NAT 测试差异**（https://www.coolapk.com/feed/67056980）：《网络综合查询器》严格区分两套标准，因 RFC3489 已较老旧，常见 `RFC5780` 测得 `NAT3` 而 `RFC3489` 测为 `NAT4` 的差异；多网卡环境下受限于纯易语言无法指定 IPv6 UDP 出口网卡，需在「设置端口 → 默认参照」手动选择对应当前外网 IP 的网卡 IP。
- **星空组网 `6.0.6`**（https://www.coolapk.com/feed/72586095）：针对无公网 IP 场景提供免端口映射的远程组网与 SMB/影音直连优化。
- **Gridea + GitHub 私有库 + Netlify + Cloudflare 博客方案**（https://www.coolapk.com/feed/73412867）：将静态源码放于 GitHub 私有库，由 Netlify 拉取构建并配合 Cloudflare 仅 DNS（`@` 根域名 / `www` 子域名）指向 Netlify，解决 `github.io` 国内访问慢与样式异常问题。
- **深澜 Web 2022 校园网研究边界**（https://www.coolapk.com/feed/73434229）：仅记录校园网单设备认证机制研究背景与参考资料，涉及具体绕过操作需遵守校园网管理规范。
