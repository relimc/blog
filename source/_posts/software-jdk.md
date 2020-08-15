---
title: 安装软件：JDK
comments: true
date: 2020-05-28 17:20:25
categories: 
- Java
tags:
- 软件
- JDK
---

JDK 的英文全名是：Java SE Development Kit（Java Development Kit），中文意思是：Java 开发工具包。意思就是说，如果你想要编写并运行 Java 程序，你就必须得有这个包。

<!-- more -->

## 获取安装包

[官网](https://www.oracle.com/java/technologies/javase-downloads.html)提供了多种版本的下载入口。我选择的是 [Java SE 8u251](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html) 这个版本。这个版本有多种用于不同操作系统的安装包。在当前系统（CentOS7）下，我们可以下载 tar 包或者 rpm 包来安装 JDK。我在这里会演示两种包的安装过程，你可根据自己的需要选择其中一种包来安装即可。

我这里下载的包名如下：

+ jdk-8u251-linux-x64.tar.gz
+ jdk-8u251-linux-x64.rpm

如果出现需要账号登录才能下载的情况，可尝试使用下面两个别人分享的账号：

+ 账号：2696671285@qq.com ；密码：Oracle123
+ 账号：908344069@qq.com；密码：Java2019

如果账号失效了，请自行百度：oracle 账号。

在官网里，我们可以看到 Java SE 14 和 Java SE 11 等等的版本，每个版本下面都有相应的 JDK 下载。那么这些 Java SE 和 JDK 的关系是什么呢？

在这里得提几个概念：SE，EE 和 ME。

+ Java SE：Java Platform，Standard Edition ，Java 标准版本，用于开发在电脑上运行的桌面程序，如 Eclipse
+ Java EE：Java Platform，Enterprise Edition，Java 企业版本，用于开发网站，如 Tomcat
+ Java ME：Java Platform，Micro Edition，Java 微型版本，用于开发移动设备程序

它们的关系可以简单地理一下：

+ Java SE 是 Java EE 和 Java Me 的基础，其开发包是 JDK，即 Java SE Development Kit。
+ Jave EE 是在 Java SE 的基础上的一套规范,，其开发包是 SDK，即 Java EE Software Development Kit。
+ Java ME 是 Java SE 的简化版。

## 卸载系统自带的 JDK

部分 linux 发行版可能自带有 JDK，一般是开源版本的 JDK。我们在这里安装的是 Oracle 公司的 JDK。 这两种 JDK 的区别与关系可以自行搜索了解。`java -version`命令可以用来判断当前系统是否已经安装了 JDK。输入`java -version`后发现以下的提示信息：

```bash
openjdk version "1.8.0_222-ea"
OpenJDK Runtime Environment (build 1.8.0_222-ea-b03)
OpenJDK 64-Bit Server VM (build 25.222-b03, mixed mode)
```

可以看到，CentOS7 下已经安装了开源版本的 openjdk，其版本是 1.8.0_222。我这里将要安装的是 Oracle 版本的 JDK，其版本是 jdk-8u251，也就是 1.8.0_251。

如果系统已经存在了 JDK，我们需要卸载它。如果不存在的话，这节的内容可以跳过。

```bash
rpm -qa | grep java # 查询带有 java 字样的安装包
```

获得的信息如下：

```bash
java-1.7.0-openjdk-1.7.0.221-2.6.18.1.el7.x86_64
python-javapackages-3.4.1-11.el7.noarch
tzdata-java-2019b-1.el7.noarch
javapackages-tools-3.4.1-11.el7.noarch
java-1.8.0-openjdk-headless-1.8.0.222.b03-1.el7.x86_64
java-1.7.0-openjdk-headless-1.7.0.221-2.6.18.1.el7.x86_64
java-1.8.0-openjdk-1.8.0.222.b03-1.el7.x86_64
```

我们需要把**java-数字版本号**开头的包都删除掉：

```bash
rpm -e --nodeps java-1.7.0-openjdk-1.7.0.221-2.6.18.1.el7.x86_64
rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.222.b03-1.el7.x86_64
rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.221-2.6.18.1.el7.x86_64
rpm -e --nodeps java-1.8.0-openjdk-1.8.0.222.b03-1.el7.x86_64
```

现在在执行`java -version`发现命令已经失效了，说明当前系统下已经没有 JDK 了。

```bash
-bash: /usr/bin/java: No such file or directory
```

## 通过 tar 包安装 JDK

确认你的 tar 包所在的目录，通过浏览器下载的包一般在 `~/Downloads` 下。

将目录切换到 tar 包所在的目录，我这里是 `/root/Downloads/`。

```bash
cd /root/Downloads/ && ll
```

显示结果如下：

```bash
-rw-r--r--. 1 root root 179472367 May  8 18:26 jdk-8u251-linux-x64.rpm
-rw-r--r--. 1 root root 195132576 May  8 18:26 jdk-8u251-linux-x64.tar.gz
```

我这里两个包都下载好了。将 tar 包解压到 `/opt` 下。

```bash
tar -zxvf jdk-8u251-linux-x64.tar.gz -C /opt/
```

然后将目录切换指 `/opt`，查看刚才的 tar 包解压是否成功。

```bash
cd /opt && ll
```

显示结果如下：

```bash
drwxr-xr-x. 7 10143 10143 245 Mar 12 14:37 jdk1.8.0_251
```

进入 JDK 目录，可以查看 JDK 目录下有哪些内容。

```bash
cd jdk1.8.0_251/ && ll
```

显示结果如下：

```bash
drwxr-xr-x. 2 10143 10143     4096 Mar 12 14:33 bin
-r--r--r--. 1 10143 10143     3244 Mar 12 14:33 COPYRIGHT
drwxr-xr-x. 3 10143 10143      132 Mar 12 14:33 include
-rw-r--r--. 1 10143 10143  5217764 Mar 12 11:25 javafx-src.zip
drwxr-xr-x. 5 10143 10143      185 Mar 12 14:33 jre
drwxr-xr-x. 5 10143 10143      245 Mar 12 14:33 lib
-r--r--r--. 1 10143 10143       44 Mar 12 14:33 LICENSE
drwxr-xr-x. 4 10143 10143       47 Mar 12 14:33 man
-r--r--r--. 1 10143 10143      159 Mar 12 14:33 README.html
-rw-r--r--. 1 10143 10143      424 Mar 12 14:33 release
-rw-r--r--. 1 10143 10143 20923007 Mar 12 14:33 src.zip
-rw-r--r--. 1 10143 10143   117365 Mar 12 11:25 THIRDPARTYLICENSEREADME-JAVAFX.txt
-r--r--r--. 1 10143 10143   169571 Mar 12 14:33 THIRDPARTYLICENSEREADME.txt
```

至此，tar 包形式的 JDK 安装就完毕了。

我们可以通过执行`java -version`命令来判断是否安装成功。

来到 JDK 存放执行文件的目录，然后执行`./java -version`

```bash
cd /opt/jdk1.8.0_251/bin
./java -version
```

提示信息如下：

```bash
java version "1.8.0_251"
Java(TM) SE Runtime Environment (build 1.8.0_251-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.251-b08, mixed mode)
```

## 通过 rpm 包安装 JDK

前文提到，rpm 包在 `/root/Downloads` 下，我们切换到那个目录下。

```bash
cd /root/Downloads
```

我们可以查看 rpm 包下所有的目录和文件

```bash
rpm -qpl jdk-8u251-linux-x64.rpm
```

安装 rpm 的命令如下

```bash
rpm -ivh jdk-8u251-linux-x64.rpm
```

我们把目录切换到 `/usr/java` 并查看文件夹的内容

```bash
cd /usr/java && ll
```

获取的信息如下

```bash
lrwxrwxrwx. 1 root root  16 May  8 18:44 default -> /usr/java/latest
drwxr-xr-x. 8 root root 258 May  8 18:44 jdk1.8.0_251-amd64
lrwxrwxrwx. 1 root root  28 May  8 18:44 latest -> /usr/java/jdk1.8.0_251-amd64
```

至此，通过 rpm 包形式安装 JDK 就完毕了。

在终端输入`java -version`查看是否有提示信息：

```bash
java version "1.8.0_251"
Java(TM) SE Runtime Environment (build 1.8.0_251-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.251-b08, mixed mode)
```

## 配置环境变量

如果你是通过 rpm 包安装 JDK，那么这节的内容可以跳过。因为通过这种方式安装的 JDK，不需要手动配置环境，或者说，在安装的过程中，已经把环境变量配置好了。如果需要了解较详细的说明，可以参考这篇[Linux的RPM安装方式为什么不需要配置环境](https://blog.csdn.net/qq_31246691/article/details/79331355)。

你如果是通过 tar 包安装 JDK，就需要配置环境变量了。

配置环境变量是什么意思呢？

前面我们通过 tar 包安装好 JDK 后，到 JDK 的 bin 目录下执行了`./java -version`命令来判断 JDK 是否安装成功。`./java -version`的意思是说，在当前目录下查找并执行`java`这个可执行文件，其参数是`version`。那个命令是只能在 bin 目录下才能执行的，因为 java 这个文件在 bin 目录下。你可以在其它目录下看是否能执行那个命令。

如果我们想要在任意目录下执行`java -version`命令呢？那我们就需要配置环境变量了。

配置环境变量的意思可以简单地理解为，它可以让我们在任意目录下执行某个命令。

现在我们开始配置环境变量。使用 vi 编辑器打开配置环境变量的文件：

```bash
vi /etc/profile
```

在文件末尾添加两行代码：

```
export JAVA_HOME=/opt/jdk1.8.0_251
export PATH=$JAVA_HOME/bin:$PATH
```

注意：第一行中，JAVA_HOME=你的 JDK 安装目录，我的 JDK 目录是`/opt/jdk1.8.0_251`，我这里的 JAVA_HOME 就等于`/opt/jdk1.8.0_251`。第二行代码可以直接复制。

保存并关闭`/etc/profile`后，执行`source /etc/profile`命令，让刚才添加的两行代码生效。现在，我们就可以在任意目录下执行`java -version`命令了。