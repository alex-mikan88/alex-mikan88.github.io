---
layout: post
title: 自用服务器脚本
categories: [Note, VPS]
description:  为了省事，经常会用到一些脚本，这边整理的就是常用的一些脚本。
keywords: VPS
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---



## 升级 Packages

```bash
sudo -i # 切换到 root 用户
apt update -y  # 升级 packages
apt install wget curl sudo vim bash git -y  # 安装常用的软件（可选）
```

## 安装 Docker 环境

### 安装 Docker
```bash
wget -qO- get.docker.com | bash
docker -v  #查看 docker 版本
systemctl enable docker  # 设置开机自动启动
```

### 安装 Docker-compose
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version  #查看 docker-compose 版本
```

## 修改 Docker 配置
来自：[烧饼博客](https://u.sb/debian-install-docker/)

以下配置会增加一段自定义内网 IPv6 地址，开启容器的 IPv6 功能，以及限制日志文件大小，防止 Docker 日志塞满硬盘（泪的教训）：

```bash
cat > /etc/docker/daemon.json << EOF
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "20m",
        "max-file": "3"
    },
    "ipv6": true,
    "fixed-cidr-v6": "fd00:dead:beef:c0::/80",
    "experimental":true,
    "ip6tables":true
}
EOF
```

然后重启 Docker 服务：

```bash
systemctl restart docker
```
