# Flexus X 上以 Git 与 Docker 部署 ASP.NET Core 8

## 原帖环境

原帖记录的云服务器为 Flexus X 实例，自定义规格 `4vCPUs | 12GiB`，镜像选择 Ubuntu（未给出具体发行版版本）。项目为 .NET 8 Web API，代码托管于 Gitee；示例默认项目目录和项目名为 `WebApplication1`。这些是作者报告的实验配置，不构成对其他规格或系统版本的适配验证。

## 多阶段 Dockerfile

原帖给出的 Dockerfile：

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WebApplication1/WebApplication1.csproj", "WebApplication1/"]
RUN dotnet restore "WebApplication1/WebApplication1.csproj"
COPY . .
WORKDIR "/src/WebApplication1"
RUN dotnet build "WebApplication1.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "WebApplication1.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication1.dll"]
```

这是 SDK 构建阶段与 ASP.NET Core 运行时阶段分离的多阶段构建示例。原帖提示可按解决方案名称及目标 .NET 版本调整。示例使用 `USER $APP_UID`；原文没有说明基础镜像中该变量的具体取值或运行时权限配置。

## Git 与 SSH 密钥

原帖列出的 Git 安装与身份配置命令：

```sh
sudo apt update
sudo apt install git
git config --global user.name "你的名字"
git config --global user.email "你的邮箱地址"
```

检查 SSH 目录：

```sh
ls -al ~/.ssh
```

作者报告当时目录存在但没有 `id_rsa` 和 `id_rsa.pub`，随后给出：

```sh
chmod 700 /root/.ssh
ssh-keygen -t rsa -b 4096 -C "你的邮箱地址"
cat /root/.ssh/id_rsa.pub
```

原帖说明 `ssh-keygen` 可按提示使用默认保存路径 `/root/.ssh/id_rsa`，私钥口令可选；再将公钥添加到 Gitee 的 SSH 公钥设置后克隆仓库。上述 `/root/.ssh` 路径针对 root 用户；若由普通用户执行，`~` 指向该用户目录，不能把两者混为一谈。原文未提供仓库地址或实际克隆命令。

## Docker 安装源配置

原帖先安装依赖：

```sh
sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release
```

原文列出官方与阿里云 GPG 地址：

- Docker 官方 GPG：`https://download.docker.com/linux/ubuntu/gpg`
- 阿里云镜像 GPG：`https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg`

对应命令中的 URL 在原帖正文被“查看链接”替代，按上下文恢复后的命令形式为：

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

原帖将第二条称为推荐使用的阿里源。两条命令都写入同一个 keyring 文件，原帖没有说明应择一执行还是如何处理重复写入，因此不要把两条视作必须连续执行的完整流程。

原帖列出 APT 源地址：

- Docker 官方源：`https://download.docker.com/linux/ubuntu`
- 阿里云镜像源：`https://mirrors.aliyun.com/docker-ce/linux/ubuntu`

对应源配置命令：

```sh
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

两条命令同样写入 `/etc/apt/sources.list.d/docker.list`，原帖未说明应选用其一；按原文，添加源后还需再次更新软件包列表。输入正文在“再次更新软件包列表”标题处结束，实际更新命令及后续安装、构建、运行命令未提供，不补造。

## 资源与证据边界

Docker 上游资源：`https://download.docker.com/linux/ubuntu/gpg`、`https://download.docker.com/linux/ubuntu`。阿里云镜像资源：`https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg`、`https://mirrors.aliyun.com/docker-ce/linux/ubuntu`。原帖还给出文章链接 `https://ruocheng.blog.csdn.net/article/details/141748572`，但没有证明其与本帖 Docker 步骤的具体对应关系。

来源：酷安文章《Flexus X实例快速发布部署-让你的项目飞起来~》，https://www.coolapk.com/feed/61714636 。文章含 5 张图片，图片未读取；正文在 APT 索引更新说明处截断。
