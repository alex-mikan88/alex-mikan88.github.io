---
layout: post
title: 自用服务器脚本——Swap 分区篇
categories: [VPS, Linux]
description: 为了省事，经常会用到一些脚本，这边整理的就是常用的一些脚本。
keywords: VPS
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

买到新机器之后，为了省事，经常会用到一些服务器脚本，这边整理的就是蜜柑自己常用的一些服务器脚本。

_PS：本文同时适用于 Debian 10 / 11 以及 Ubuntu_

## 升级 Packages

```bash
sudo -i # 切换到 root 用户
apt update -y  # 升级 packages
apt install wget curl sudo vim bash git -y  # 安装常用的软件（可选）
```

## 检查系统是否已经有 Swap 分区

```bash
swapon -s
```

或者

```bash
free -m
```

如果没有返回结果或者 `free -m` 中 `Swap` 一列数值是 `0`，则表示系统没有 Swap 分区。

## 通过 fallocate 命令创建交换文件

我们通过使用 `fallocate` 命令，创建一个 1GB 大小的 Swap 分区：

```bash
fallocate -l 1G /swapfile
```

然后设置这个文件的权限：

```bash
chmod 600 /swapfile
```

## 将新文件标记为交换空间

```bash
mkswap /swapfile
```

## 启用交换文件

```bash
swapon /swapfile
```

## 持久化

```bash
echo "/swapfile swap swap defaults 0 0" >> /etc/fstab
```
