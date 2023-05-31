---
layout: post
title: 如何部署基于 Docker 的为知笔记
categories: [Docker]
description: 10分钟搭建你专属的为知笔记
keywords: Docker, Wiznote
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

仅用10分钟，就能搭建专属于你的为知笔记~

## 准备：

* 大陆地区服务器需要配置镜像源，创建或修改 /etc/docker/daemon.json 文件，修改为如下形式然后重启 Docker 服务

```json
{
    "registry-mirrors": [
        "http://hub-mirror.c.163.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://registry.docker-cn.com"
    ]
}
```

## 下载并启动：
* 请在终端中输入下面的命令：

```
cd ~
mkdir wizdata
```

* 在用户主目录建立一个 wizdata 的文件夹。为知笔记服务端会把所有的数据保存在这个目录里面。如果是正式使用，请注意定时备份该目录。
* 然后输入下列命令来启动服务

```
docker run --name wiz --restart=always -it -d -v  ~/wizdata:/wiz/storage -v  /etc/localtime:/etc/localtime -p 80:80 -p 9269:9269/udp  wiznote/wizserver
```

* xxxxxxxxxx1 1echo "/swapfile swap swap defaults 0 0" >> /etc/fstabbash
* 然后打开浏览器，在地址栏里面输入：`http://ip:80`，如果服务正常，则会出现下面的界面：


![ok](https://cdn2.wiz.cn/wp-content/new-uploads/be0563b0-da94-11e9-b6e3-2953c693f8f7.png)

![error](https://cdn2.wiz.cn/wp-content/new-uploads/c4772b70-da94-11e9-b6e3-2953c693f8f7.png)


* 通常表示为知笔记服务还没有启动起来，请继续等待并刷新浏览器。
* 如果您当前服务器/电脑的80端口已经被占用，则可以使用其他的端口，例如使用8080端口
* 将上面命令中的-p 80:80 修改为 -p 8080:80 即可。（前面代表当前服务器的端口，可以自行修改。后面的80端口不能修改）。
* **注意！**修改端口后，在浏览器里面，则需要输入相应的**端口号**，例如：`http://ip:8080`

## 修改启动参数，并重新启动服务，例如修改映射端口

```
docker stop wiz
docker rm wiz
docker run --name wiz --restart=always -it -d -v  ~/wizdata:/wiz/storage -v  /etc/localtime:/etc/localtime -p 80:80 -p 9269:9269/udp  wiznote/wizserver
```

* 其中最后一行，请自行修改为自己需要的命令行。

## 系统重新启动后，重新启动服务：

```
docker start wiz
```

## 更新服务命令行：

```
docker stop wiz
docker rm wiz
docker pull wiznote/wizserver:latest
docker run --name wiz --restart=always -it -d -v  ~/wizdata:/wiz/storage -v  /etc/localtime:/etc/localtime -p 80:80 -p 9269:9269/udp  wiznote/wizserver
```

* 其中最后一行，请自行修改为自己需要的命令行。

## F&Q（节选自[为知笔记服务端docker镜像使用说明](https://www.wiz.cn/zh-cn/docker)）：
### 如何配置https？
[/docker-https](https://www.wiz.cn/zh-cn/docker-https)

### 管理员账号是什么？
默认管理员账号：admin@wiz.cn，密码：123456。请在部署完成后，使用这个账号，登录网页版，然后修改管理员密码。其他用户，请自行注册。免费版本可以注册5个用户（不包含管理员账号）

### 为知笔记数据保存在哪里？
所有数据，都保存在我们前面建立的目录里面。请定时备份该目录，避免数据丢失。

### 重新启动服务器/电脑后，如何重新启动为知笔记服务？
在命令行中窗口/终端中，输入

```
docker start wiz
```

就可以重新启动为知笔记服务了。

### 可以使用客户端访问吗？
可以，您可以直接使用所有的官方客户端，然后在登录的时候，选择登录到企业私有服务器即可。注意：该功能仅限于客户端所在网络可以访问到您的企业私有服务器才可以。例如，手机客户端，在离开公司网络的环境下，通常无法访问私有部署的为知笔记。但是已经离线的数据，则可以正常访问。也可以在离线环境下新建/修改笔记，并在回到公司后进行同步。

### 可以禁止客户端访问吗？
可以禁用客户端访问，确保数据只能通过网页版访问。一旦离开公司网络，就无法访问任何数据。

### 为知笔记服务端有时间限制吗？
没有。在限定的用户数量下，您可以永久免费使用。如果想要更多用户使用，请联系我们购买使用许可。

### 如何升级为知笔记服务端？
我们会经常更新docker镜像。您只需要下载更新docker镜像，然后重新启动docker镜像即可升级为知笔记服务端。无需更多额外操作。

下面是更新镜像命令行：

```
docker stop wiz
docker rm wiz
docker pull wiznote/wizserver:latest
```

更新完成后，重新使用前面启动镜像的命令，就可以完成服务端升级。你也可以使用 Watchtower 来自动更新 WizNote 的镜像：[https://github.com/containrrr/watchtower](https://github.com/containrrr/watchtower)

### 使用一段时间后，如果想要将数据从本地硬盘迁移到NAS或者云存储里面可以吗？
可以。包括数据库，笔记数据内容等，都可以完整的进行迁移。

### 如何进行数据备份？
您可以自己备份用户数据目录，或者将数据保存在NAS/云存储里面。

### 服务启动后新建笔记时间不正确
因为docker镜像默认时区不正确。因此需要进入docker里面手工设置一下时区，命令如下：

```
docker exec -it wiz /bin/bash
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
exit
```

上面的命令，会把docker里面的时区设置为东八区（北京时间）。如果需要设置成其他的时区，请自行修改上面的命令。具体时区的名称，可以搜索linux时区名称获取。

如果是linux，则可以通过在命令行里面加入命令，来自动获取当前时区：

```
-v  /etc/localtime:/etc/localtime
```

完整命令行：实际使用是，请根据自己的情况调整其他参数，例如映射路径，端口映射等。

```
docker run --name wiz --restart=always -it -d -v  ~/wizdata:/wiz/storage -p 8088:80 -v  /etc/localtime:/etc/localtime wiznote/wizserver
```
