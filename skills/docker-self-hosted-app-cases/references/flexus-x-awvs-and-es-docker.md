# Flexus X 上部署 AWVS 与 Elasticsearch 7.16.2 容器记录

本记录整理自两篇基于华为云 Flexus X 的 Docker 部署记录，均未在本库实机复现。

## 1. AWVS（Acunetix）容器化部署 [61917434](https://www.coolapk.com/feed/61917434)

- 参考镜像与项目：[secfa/awvs (Docker Hub)](https://hub.docker.com/r/secfa/awvs)、[Acunetix 官网](https://www.acunetix.com/)。
- 拉取并启动容器（需添加 `CAP_LINUX_IMMUTABLE` 能力并将容器 `3443` 端口映射到宿主机 `13443`）：

```bash
docker pull secfa/awvs:latest
docker run -itd --name awvs-01 -p 13443:3443 --cap-add CAP_LINUX_IMMUTABLE secfa/awvs:latest
docker ps
```

- 在云主机安全组放行 `13443` 端口后，通过 `http://<主机IP>:13443`（或按镜像实际协议）访问；原帖含 4 张图片未读取，未记录初始登录凭据与扫描验证结果。

## 2. CentOS/YUM 环境安装 Docker CE 与单节点 Elasticsearch 7.16.2 [61916101](https://www.coolapk.com/feed/61916101)

1. 安装依赖并配置华为云 CentOS Docker CE 源（[docker-ce.repo](https://repo.huaweicloud.com/docker-ce/linux/centos/docker-ce.repo)）：

```bash
sudo yum update -y
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
wget https://repo.huaweicloud.com/docker-ce/linux/centos/docker-ce.repo -P /usr/local/docker
yum-config-manager --add-repo /usr/local/docker/docker-ce.repo
yum install -y docker-ce docker-ce-cli containerd.io
docker --version
sudo systemctl status docker
```

2. 拉取并启动单节点 Elasticsearch `7.16.2`：

```bash
docker pull elasticsearch:7.16.2
docker run --name elasticsearch -p 9200:9200 -e "discovery.type=single-node" -d elasticsearch:7.16.2
curl -X GET "localhost:9200/"
```

- 原帖含 5 张图片未读取；未记录生产环境 JVM 堆内存限制、持久化卷挂载或安全认证配置。
