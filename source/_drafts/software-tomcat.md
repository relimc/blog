---
title: 安装软件：Tomcat
comments: true
date: 2020-05-28 17:31:49
categories:
- Java
tags:
- Tomcat
- 软件
- 服务器
---

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用[服务器](https://baike.baidu.com/item/服务器)，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。对于一个初学者来说，可以这样认为，当在一台机器上配置好Apache 服务器，可利用它响应[HTML](https://baike.baidu.com/item/HTML)（[标准通用标记语言](https://baike.baidu.com/item/标准通用标记语言/6805073)下的一个应用）页面的访问请求。实际上Tomcat是Apache 服务器的扩展，但运行时它是独立运行的，所以当你运行tomcat 时，它实际上作为一个与Apache 独立的进程单独运行的。

诀窍是，当配置正确时，Apache 为HTML页面服务，而Tomcat 实际上运行JSP 页面和Servlet。另外，Tomcat和[IIS](https://baike.baidu.com/item/IIS)等Web服务器一样，具有处理HTML页面的功能，另外它还是一个Servlet和JSP容器，独立的Servlet容器是Tomcat的默认模式。不过，Tomcat处理静态[HTML](https://baike.baidu.com/item/HTML)的能力不如Apache服务器。

> [百度百科：Tomcat](https://baike.baidu.com/item/tomcat/255751?fr=aladdin)

<!-- more -->

## 获取 Tomcat 包

进入[官网](http://tomcat.apache.org/)选择自己所需要的版本下载。在 CentOS 7 中，可选 tar 包版和 rpm 包版。

我这里下载的是 tar 包：apache-tomcat-linux-8.5.41.tar。

## 解压 Tomcat 包

我的 tar 包放在 `/opt/tomcat` 文件夹下，你在解压你的 tar 包时需要注意你的 tar 包在哪里。

```bash
cd /opt/tomcat
tar -xvf apache-tomcat-linux-8.5.41.tar
```

解压完成后，`/opt/tomcat` 目录情况如下：

```bash
drwxr-xr-x. 9 root root     220 Apr 13 18:05 apache-tomcat-8.5.41
-rw-r--r--. 1 root root 9699102 Apr 13 18:05 apache-tomcat-linux-8.5.41.tar.gz
```

## 启动 Tomcat 服务器

`cd apache-tomcat-8.5.41`进入到 apache-tomcat-8.5.41 文件下，`ls -al`查看目录情况如下：

```bash
drwxr-x---. 2 root root  4096 Apr 13 18:05 bin
-rw-r-----. 1 root root 19539 May  4  2019 BUILDING.txt
drwx------. 3 root root   254 Apr 13 18:06 conf
-rw-r-----. 1 root root  5407 May  4  2019 CONTRIBUTING.md
drwxr-x---. 2 root root  4096 Apr 13 18:05 lib
-rw-r-----. 1 root root 57092 May  4  2019 LICENSE
drwxr-x---. 2 root root   197 Apr 13 18:06 logs
-rw-r-----. 1 root root  1726 May  4  2019 NOTICE
-rw-r-----. 1 root root  3255 May  4  2019 README.md
-rw-r-----. 1 root root  7139 May  4  2019 RELEASE-NOTES
-rw-r-----. 1 root root 16262 May  4  2019 RUNNING.txt
drwxr-x---. 2 root root    30 Apr 13 18:05 temp
drwxr-x---. 7 root root    81 May  4  2019 webapps
drwxr-x---. 3 root root    22 Apr 13 18:06 work
```

看到有个 bin 文件夹了吗？Tomcat 的启动和关闭等操作脚本都在这里面。进入 bin 目录下，执行 `./startup.sh` 脚本即可启动 Tomcat。

```bash
cd bin
./startup.sh
```

如果你启动成功，会看到类似如下的提示信息：

```bash
Using CATALINA_BASE:   /opt/tomcat/apache-tomcat-8.5.41
Using CATALINA_HOME:   /opt/tomcat/apache-tomcat-8.5.41
Using CATALINA_TMPDIR: /opt/tomcat/apache-tomcat-8.5.41/temp
Using JRE_HOME:        /opt/java/jdk1.8.0_221
Using CLASSPATH:       /opt/tomcat/apache-tomcat-8.5.41/bin/bootstrap.jar:/opt/tomcat/apache-tomcat-8.5.41/bin/tomcat-juli.jar
Tomcat started.
```

最后，在网页下输入 ip 地址访问 Tomcat 服务器。查询 ip 的方法如下：

```bash
# 查询
ip addr 
# 或者
ifconfig
```

通过`ip addr`获取的信息如下：

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:b8:d6:2d brd ff:ff:ff:ff:ff:ff
    inet 192.168.216.132/24 brd 192.168.216.255 scope global noprefixroute dynamic ens33
       valid_lft 1554sec preferred_lft 1554sec
    inet6 fe80::a880:245e:bdbd:2828/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

这行 **inet 192.168.216.132/24 brd ...** 中的 **192.168.216.132** 就是我们需要的 ip 地址

通过`ifconfig`获取的信息如下：

```bash
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.216.132  netmask 255.255.255.0  broadcast 192.168.216.255
        inet6 fe80::a880:245e:bdbd:2828  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:b8:d6:2d  txqueuelen 1000  (Ethernet)
        RX packets 181720  bytes 222516300 (212.2 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 19621  bytes 4699418 (4.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 154  bytes 10028 (9.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 154  bytes 10028 (9.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

第二行 **inet 192.168.216.132  netmask ...** 中的 **192.168.216.132** 就是我们需要的 ip 地址。

Tomcat 安装好后其默认的端口号为 **8080**。在浏览器地址栏输入 ip 地址：192.168.216.132:8080。回车后即可访问 Tomcat 的欢迎页。

每台主机的IP地址都不一样，不要直接复制我的IP地址。比如，你获取的 ip 是192.168.216.144，你在浏览器就应该输入 192.168.216.144:8080。

注意：我的 CentOS 7 是安装在 Windows 7 下的虚拟机软件里。Tomcat 装在 CentOS 7 后开启服务，我是通过 Windows 7 的谷歌浏览器访问 Tomcat 服务器。这相当于远程访问 Tomcat 服务器。所以需要远程服务器的 ip 地址和端口号。

你如果是在本地安装 Tomcat——可以理解为浏览器和Tomcat在同一台电脑上，可直接尝试 localhost:8080 或者 127.0.0.1:8080 访问。

如果你无法通过网页访问 Tomcat 服务器，可以尝试下面的解决方法。

## 网页访问Tomcat服务器异常时的处理方法

### 防火墙未开放端口

防火墙默认只开启 22 端口，防火墙如果没有开启 8080 的端口，我们就无法访问 Tomcat 服务器。查看防火墙已经开启哪些端口的命令如下：

```bash
firewall-cmd --list-ports
```

获取信息如下：

```bash
8080/tcp 8989/tcp
```

我这里已经开放了 8080 端口。

如果你获取的信息里没有 8080 端口，可以这样来开放端口号：

```bash
firewall-cmd --add-port=8080/tcp --permanent
firewall-cmd --reload # 无需重启防火墙使修改的端口生效
```

这时，应该就可以访问 Tomcat 服务器了。

### 端口被其它服务占用

在端口冲突的情况下，并不影响 Tomcat 的启动。在 Tomcat 启动的情况下，如果端口号被占用的话，我们就无法通过**ip地址加端口号**访问到 Tomcat 服务器。如果 Tomcat 启动成功了，但是访问不了网页，那么可以考虑**是否出现了端口占用的情况**。

查看服务端口号的命令如下：

```bash
netstat -ntulp
```

获取的信息如下：

{% asset_img tomcat-netstat.jpg "Tomcat 默认端口是否被占用" %}

注意箭头指向的那行。现在 8080 端口是被 httpd 服务占用了。

解决这个问题有多种方法，最简单的方法就是把那个占用端口的服务关掉。如关掉 httpd 服务：

```bash
# 关闭 httpd 服务
systemctl stop httpd.service
# 然后，重启Tomcat
./shutdown.sh
./startup.sh
```

现在我们可以尝试通过网页访问 Tomcat 服务器。

当然这个方法并不是一个好的处理方式。我们可以通过修改 Tomcat 服务器的配置文件来修改端口号。

```bash
cd /opt/tomcat/apache-tomcat-8.5.41
cd conf
vi server.xml
```

在69行找到这样的一行**<Connector port="8080" protocol="HTTP/1.1"**，将 8080 改成 8989，得确认 8989 目前没有被其它服务占用。保存并退出后，重启 Tomcat 服务器。这时应该能够通过 192.168.216.132:8989 访问到 Tomcat 服务器了。