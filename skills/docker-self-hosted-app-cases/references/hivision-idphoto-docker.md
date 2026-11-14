# 在 Flexus X 上运行 HivisionIDPhotos Docker 镜像

## 原帖实验配置

作者报告使用华为云 Flexus X 实例，规格为 4 核 12G。文章没有说明操作系统版本、Docker 版本、镜像运行参数或 CPU 性能模式。原帖对云产品的性能、可用性及成本描述属于产品介绍主张，没有独立验证数据。

## Docker 命令

原帖给出的命令：

```sh
docker pull linzeyi/hivision_idphotos:v1
docker tag linzeyi/hivision_idphotos:v1 hivision_idphotos
docker run -d -p 7860:7860 hivision_idphotos
```

含义是拉取 `linzeyi/hivision_idphotos:v1`，将本地镜像另标记为 `hivision_idphotos`，再以后台模式运行容器，并将主机 7860 端口映射到容器 7860 端口。原帖提示容器启动后通过浏览器访问服务器 IP 的 7860 端口；正文写作 `http:ip+7860`，格式并非完整 URL，不能据此推断实际公网地址或协议配置。云安全组也需允许所需入站访问，原帖未写出具体规则。

## 原帖报告的使用体验

作者称上传照片并点击“开始制作”后，可生成所选证件照尺寸，并报告图片约 3 秒内生成。文中没有给出图片分辨率、张数、并发量、算法参数或重复测量，因此该速度仅代表原帖体验，不是性能保证。文章称可选择多种证件照类型，但具体类型只在未读取的图片中展示。

## 项目资源

项目仓库：<https://github.com/Zeyi-Lin/HivisionIDPhotos>。镜像名与标签按原帖记录；仓库当前内容及该标签的对应关系未在本条材料中核验。

来源：酷安文章《华为云Flexus X实例部署安装HivisionIDPhoto一个轻量级的AI证件照制作算法》，https://www.coolapk.com/feed/61710260 。文章含 4 张图片，图片未读取，证件照类型列表及“其他配置参数”无法从可见正文恢复。
