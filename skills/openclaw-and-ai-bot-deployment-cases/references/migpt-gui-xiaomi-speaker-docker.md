# Docker 部署 MiGPT GUI 并接入小爱音箱

## 适用与证据范围
原帖以 Windows Docker 部署 `migpt-server` 为例，报告通过控制面板接入小米音箱、配置大模型与 TTS，并说明登录异常时在容器内写入配置文件。文章有 9 张图片，图片和部分外链未提供；页面字段名称、API 地址和音色清单无法据本文完整复原。实际运行结果为原帖报告。

## 部署服务
Windows 命令提示符执行：

```sh
docker run -d --name migpt-server -p 36592:36592 lmk123/migpt-server
docker ps
```

容器状态显示 `UP`、端口为 `36592` 时，原帖认为服务运行中。访问 `http://localhost:36592` 打开管理页面。

在控制面板填写音箱型号、小米账号 ID、账号密码和设备 ID。原帖举例设备名称需按米家 App 显示填写，包含空格也应一致。本文不复述任何实际账户凭据。

## 模型与 TTS
- 原帖以阿里云百炼 API 为模型服务示例，要求将 API 地址、API-Key 和模型标识填入面板。示例模型字符串为 `deepseek-v3.2`，原帖称当时为 DeepSeek V3.2；这是原帖发布时的信息，不代表当前可用性。
- TTS 引擎可从默认小爱音色切换到豆包。需配置对外地址（局域网部署填局域网地址，公网部署填公网地址）、APP ID、Access Token；随后用“测试语音配置”试听。原帖示例本机地址为 `http://192.168.50.149:36592`，仅为局域网示例。
- 原帖称默认试听音色为“灿灿”，并提及可用其 Gitee 页面试听其他音色，但没有给出该页面 URL。
- 人设可在面板自定义模板中覆盖后重新启动；对话唤醒词示例为“请、你、傻妞”。

## 修复原帖报告的小米登录问题
原帖称日志提示小米异地登录安全验证后，按以下方式在容器内写入 `.mi.json`。先停止占用日志的 `docker logs -f`（按 `Ctrl+C`），再进入容器：

```sh
docker exec -it migpt-server /bin/sh
mkdir -p /root/.migptgui/default
```

在容器内写文件：

```sh
cat > /root/.migptgui/default/.mi.json << 'EOF'
{
  "mina": {
    "pass": {
      "passToken": "你的小米账号Token"
    }
  },
  "miiot": {
    "pass": {
      "passToken": "你的小米账号Token"
    }
  }
}
EOF
```

`passToken` 是敏感账户凭据；此处仅保留字段结构，不含任何真实令牌。原帖还建议检查文件并退出容器：

```sh
ls -la /root/.migptgui/default/
exit
docker start migpt-server
docker logs -f migpt-server
```

原帖报告修复后服务启动并能对话。对话日志可能包含用户交互内容。

## cpolar 远程管理
原帖称先安装 cpolar 桌面端并以 `cpolar version` 检查；随后在浏览器访问 `http://127.0.0.1:9200` 登录。网站下载页和注册页未给出实际 URL。

默认隧道据原帖描述包括 `remoteDesktop`（TCP 3389）及 `website`（HTTP 8080）。编辑 `website` 指向 MiGPT GUI 服务端口，在线隧道可生成 HTTP 与 HTTPS 地址。原帖称随机域名约每 24 小时变化；固定二级子域名需付费套餐，在预留页保留名称后编辑隧道使用该子域名。其示例名称 `migpt`、地区 `China Top`，实际名称以账号可用项为准。

## 来源
- 原帖：https://www.coolapk.com/feed/69396354
- 缺失：cpolar 官网、模型 API 地址、火山引擎 TTS 音色页面及 Gitee 试听页的真实 URL；图片未提供。
