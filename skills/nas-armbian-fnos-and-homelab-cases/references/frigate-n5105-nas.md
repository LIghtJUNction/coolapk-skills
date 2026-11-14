# 绿联 NAS 上用 Frigate 与 N5105 核显做本地监控识别

## 适用范围与证据边界
原帖以绿联 NAS、N5105 Intel 核显和普通 RTSP 摄像头为例，报告通过 OpenVINO 在本地进行目标检测，并使用 Frigate 管理事件录像。部署结果是原帖报告，未独立复测。图片未读取；原帖分享出的初始密码属于账户凭据，不在知识正文复述。评论未提供。

## Frigate 与硬件路径
Frigate 是本地视频监控/NVR 系统，可接入 IP 摄像头 RTSP 流，进行目标检测、事件录像和回放管理。原帖方案在 N5105 的 Intel 核显上通过 OpenVINO 做本地 AI 推理；没有给出检测帧率、分辨率、摄像头数量或性能基准，不能据此推定负载能力。

项目：<https://github.com/blakeblackshear/frigate>

## 摄像头网络与 RTSP
先让摄像头 IP 固定：可在摄像头管理页面手动配置静态 IP，或在路由器中绑定摄像头 MAC 与固定 IP。原帖使用的摄像机不支持自身设置静态 IP，因此采用路由器 IP-MAC 绑定。

原帖给出的 RTSP URL 结构示例为：

```text
rtsp://admin:admin123@192.168.50.127/live/ch00_0
```

这是包含示例账号口令的示意格式，不应照搬其中凭据。原帖建议用 PotPlayer 等支持流播放的软件先测试 RTSP 地址能否播放，再用于 Frigate 配置。

## 一键部署
在绿联 NAS 安装 Docker 并开启 SSH。原帖从 NAS 终端执行以下命令（URL 中插入的“删”按原帖说明去除）：

```sh
curl -fsSL https://gitee.com/jun-wan/script/raw/master/frigate_deploy/deploy-frigate-dx4600.sh -o /tmp/deploy-frigate-dx4600.sh && chmod +x /tmp/deploy-frigate-dx4600.sh && /tmp/deploy-frigate-dx4600.sh
```

脚本选择 `1` 首次部署，经过 Docker 环境和 Intel 核显检查后选择镜像源与存储位置，填写摄像头名称和 RTSP 地址。原帖称脚本会自动生成配置、拉取并启动服务，状态 `UP` 表示容器运行中。原帖示例 Web UI 为 `https://192.168.50.99:8971`；cpolar 本机控制台示例为 `http://192.168.50.99:9200/`。

首次登录后原帖建议更改初始密码。Frigate 界面可查看实时画面、事件录像、回放、快照、遮罩/区域、画面变化和系统状态。若 NAS 负载偏高，原帖建议降低检测分辨率或帧率，或启用子码流；没有提供具体参数值。

## cpolar 隧道
原帖在 NAS SSH 终端执行：

```sh
sudo curl https://get.cpolar.sh | sh
sudo systemctl status cpolar
```

安装地址按原帖明确规则移除了插入的“删”字。状态 `active (running)` 表示服务在线。登录 cpolar Web UI 后，编辑 `website` 隧道或新建 HTTP 隧道，目标地址填写 Frigate 实际 Web 访问地址，原帖特别注明需带 `https` 协议，地区示例为 `China Top`。在线隧道列表可查看生成的 HTTP/HTTPS 公网地址。

固定二级子域名需先在 cpolar 后台预留，再将隧道域名类型改为二级子域名并填入名称；原帖说明随机免费域名约每 24 小时变化，固定子域名需要付费套餐。原帖没有给出可复用的公网域名。

## 来源
- 原帖：<https://www.coolapk.com/feed/72157440>
- Frigate 项目：<https://github.com/blakeblackshear/frigate>
- 部署脚本：<https://gitee.com/jun-wan/script/raw/master/frigate_deploy/deploy-frigate-dx4600.sh>（按原帖明确规则删除插入字后恢复）
- cpolar 安装脚本：<https://get.cpolar.sh>（按原帖明确规则删除插入字后恢复）
