# 原评论的 Docker 配置示例

原帖作者表示，照 AI 教程以及通过宝塔 Docker 安装 DroidSpaces 时遇到问题，但没有提供错误信息、主机系统或 Docker 版本。

原评论使用下面的整份覆盖命令。只作原建议记录，不是通用修复指令。原 HTML 将镜像 URL 后的 JSON 引号并入超链接，且正文显示“查看链接”；这里根据原始 href 恢复 URL 与闭合引号，属于明确的排版修正，未验证安装效果：

```sh
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json > /dev/null <<'EOF'
{
  "registry-mirrors": ["https://docker.1panel.live"],
  "dns": ["8.8.8.8", "114.114.114.114"],
  "iptables": false,
  "ip6tables": false,
  "bridge": "none",
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
EOF
```

评论者还建议配置好镜像源。该建议没有原作者确认或成功反馈；其他用户提到容器能启动，但安装软件或青龙仍有问题。因此不能据此认定这套配置能解决所有安装故障。配置中的网络、iptables、IPv6、bridge 设置会改变 Docker 行为，原帖没有说明适用发行版、网络拓扑或故障日志。

评论提到的镜像站：<https://docker.1panel.live>

原帖：<https://www.coolapk.com/feed/71848689>

评论区仅覆盖已存储的热门/最新评论页，并非完整评论历史。