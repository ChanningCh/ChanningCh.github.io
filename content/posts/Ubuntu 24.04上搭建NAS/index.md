---
title: "Ubuntu 24.04上搭建NAS"
date: 2024-12-30T10:14:16+08:00
description: "Ubuntu 24.04上搭建NAS"
tags: ["Ubuntu", "NAS"]
categories: ["Tools"]
comment: true
draft: false
---
## 准备

首先, 需要准备一台主机安装Linux, 我使用的Linux发行版是Ubuntu最新的:

- [Ubuntu 24.04.1 LTS (Noble Numbat)](https://releases.ubuntu.com/noble/)

当然, Windows用户在WSL 2里装也是可以的。

## 配置

### 配置net-tools

先打开terminal查看IP地址

```bash
ifconfig
```

```bash
# 打印示例
inet 192.168.11.11  netmask 255.255.240.0  broadcast 192.168.11.11
        inet6 fe80::abc:defff:f123:4567  prefixlen 64  scopeid 0x20<link>
```

如果报错, 需要先安装

```bash
 sudo apt install net-tools
```

{{< alert icon="tag" >}}
Tips: `apt`和 `apt-get`都可以用, 两者用法也基本一致, 但从版本上来讲, 在**Debian 8**之后的版本(以及基于Debian的发行版, 例如**Ubuntu**), `apt`基本可以平替 `apt-get`
{{< /alert >}}

### 安装SSH

Ubuntu默认安装了SSH Client, 但是没有安装server, 直接执行:

```
sudo apt install openssh-server
```

装好后, 可以更改 `/etc/ssh/sshd_config` 来配置SSH的执行默认启动方式, 有兴趣可以输入以下命令查看manual:

```
man sshd_config
```

例如, 这里我可以更改登录时显示的页面, 在 `/etc/ssh/sshd_config` 中添加一行:

```
Banner /etc/ssh/example.net
```

在example.net中输入你想要显示的标语, 例如:

```
# example.net
       _.-- ,.--.
     .'   .'    /
     | @       |'..--------._
    /      \._/              '.
   /  .-.-                     \
  (  /    \                     \
   \\      '.                  | #
    \\       \   -.           /
     :\       |    )._____.'   \
      "       |   /  \  |  \    )
              |   |   |  |  |  /
```

修改完后, 可以先看看配置是否有错误:

```
sudo sshd -t -f /etc/ssh/sshd_config
```

没有问题的话, 我们就可以启动了

`sudo /etc/init.d/ssh start`

或者, 如果启动过了, 可以执行restart

`sudo service ssh restart`

现在, 你就可以使用 `ssh user@ip` 的方式远程登录了, 我这里用的是 `MobaXterm`

![1735551269605](image/1735551269605.png)

#### 免密登录

#### root用户登录

#### IPV6登录

如果我的远程服务器和我的设备不在同一网段, 那么我们可以使用IPV6来进行登录, 可以参考我的另一篇博客: [使用IPV6登录SSH]({{< ref "/posts/使用IPV6连接SSH" >}} "IPV6")

### docker安装
