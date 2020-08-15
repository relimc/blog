---
title: 安装软件：Jmeter
comments: true
date: 2020-05-28 17:45:36
categories:
- Software Testing
tags:
- Jmeter
- 软件
- 接口测试
- 性能测试
- 自动化测试
---

> The **Apache JMeter™** application is open source software, a 100% pure Java application designed to load test functional behavior and measure performance. It was originally designed for testing Web Applications but has since expanded to other test functions.

JMeter 是一个 100% 由 Java 编写的开源软件，它可以用于功能测试（接口测试）和性能测试（压力测试）。它最初被设计用于Web应用测试，但后来扩展到其他测试领域。

<!-- more -->

## JMeter 下载安装

官方推荐大多数用户直接使用最新版的 JMeter。我们可以从[官网](http://jmeter.apache.org/download_jmeter.cgi)选择最新版下载。

它有 Binaries 和 Source 类型的不同安装包，我们选择 Binaries 类型的，至于怎么选择 zip 包或者 tgz 包，这其实影响不大。一般来说，你如果是在 Windows 下安装 JMeter，那下载 zip 包会好一点；你如果是在 Linux 下安装 JMeter，那下载哪个包都影响不大。我们选择哪种包，这取决于你电脑系统解压 JMeter 安装包是否方便。

我这里是在 Windows 7 下安装 JMeter，我下载的是 zip 包，因为 Windows 自带的资源管理器就可以解压 zip 包了。我如果下载了 tgz 包，我还得去下载相应的解压软件，这比较麻烦。

我下载的 zip 包的包名是：apache-jmeter-5.2.1.zip，解压出来的文件夹名是：apache-jmeter-5.2.1。在我的电脑下，其绝对路径是：F:\tools\apache-jmeter-5.2.1。其目录下有以下内容：

```
F:\tools\apache-jmeter-5.2.1 的目录

bin
docs
extras
lib
LICENSE
licenses
NOTICE
printable_docs
README.md
```

至此，JMeter 已经安装完毕了。

**JMeter 依赖 JDK 开发包。**JDK 版本最好是 JDK8 以上。

> JMeter is compatible with Java 8 or higher. 

要让 JMeter 跑起来，就必须安装好 JDK。在 Windows 下安装 JDK 的方法可以参考这篇文章：[Java 开发环境配置](https://www.runoob.com/java/java-environment-setup.html)

确认装好 JDK 后，我们可以双击  bin 目录下的 jmeter.bat 文件，让 JMeter 在电脑上跑起来。

我们也可以配置环境变量，通过`jmeter`命令让 JMeter 跑起来。

点击电脑左下角的 Windows 开始图标，在搜索框中输入**环境变量**，然后选择**编辑系统环境变量** -> **高级** -> **环境变量** -> **新建** ，如图所示：

{% asset_img env.jpg "配置环境变量" %}

最后，在**新建系统变量**中填写**变量名**和**变量值**：

+ 变量名：JMETER_HOME
+ 变量值：F:\tools\apache-jmeter-5.2.1

注意你的变量值应该和我的不一样。这里的变量值是 JMeter 安装目录的绝对路径。

点击确定保存好 JMETER_HOME 后，在**系统变量**里找到**Path**变量。

{% asset_img path.jpg "编辑 Path 变量" %}

选中**Path**后点击**编辑**按钮，在**编辑系统变量**中，在**变量值**输入框中，将鼠标光标移到行首，切记不要删除这行的任何内容。确认光标在最前面后，复制粘贴这个`%JMETER_HOME%\bin;`。最后点击确定，打开命令提示符，输入`jmeter` 应该就可以打开  JMeter 的图形界面。

## JMeter 图形界面

在打开的图形界面中，右击 **Test Plan**，鼠标移到 **Add** 处，会有一个子菜单。这个子菜单几乎包含了 JMeter 的所有功能。

{% asset_img jmeter-5.2.1.jpg "Jmeter 图形界面" %}