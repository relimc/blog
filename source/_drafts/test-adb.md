---
title: 软件测试：安卓调试桥
categories:
  - Android
tags:
  - 调试
  - 猴子测试
comments: true
date: 2020-05-29 14:51:04
---


安卓调试桥（Android Debug Bridge，简写为 adb）是 Android SDK 中的一个工具。我们可以在电脑上使用这个工具来管理和控制安卓手机或者安卓模拟器。它在 Android SDK 目录下的 platform-tools 子目录里。

{% asset_img whereis_adb.jpg "安卓调试桥的位置" %}

<!-- more -->

## 准备事项

要获取 adb，我们只需要在电脑上安装 Android SDK 即可。

要使用 adb，我们只需要将 SDK 所在的目录加入环境变量即可。

Android SDK 的获取和安装，以及环境变量的配置，可参考这篇文章。

获取 adb 后，现在我们还差个安卓手机或者安卓模拟器。我这里是使用安卓模拟器的方式（不忍心把手机拿来测试）。网络上有多种模拟器可供选择，我这里选择的是网易的[MuMu模拟器](http://mumu.163.com/)。此外，你还需要一个准备一个 APP 来作为测试的对象。我这里下载的是谷歌浏览器（com.android.chrome_404411700.apk）。当然你也可以选择模拟器里的已经安装好的 APP 来作为测试对象。MuMu 模拟器安装好后的界面如下：

{% asset_img mumu.jpg "MuMu模拟器界面" %}

## 开始使用

在 cmd 命令提示符中，输入以下命令即可连接MuMu模拟器。

```powershell
 adb connect localhost:7555 # 连接模拟器
```

现在来查看是否连接成功：

```powershell
adb devices # 获取设备列表
```

提示信息如下：

```powershell
List of devices attached
127.0.0.1:7555  device
```

### 常用命令

上文我们已经接触了2个简单的命令，现在我们来学习一些常用的命令。

#### 查看日志 logcat

默认在终端显示日志信息

```powershell
adb logcat
```

将日志输出到指定文件

```powershell
adb logcat > d:logcat.log # 将日志重定向直 d 盘的 logcat.log 中
```

#### 安装卸载 install/uninstall

安装

```powershell
adb install F:\\tools\\com.android.chrome_404411700.apk
```

如果出现 `Success` 的提示，就表示安装成功了。

卸载时不能直接只用安装时的包名，我们得先获取安装包的包名。

获取谷歌浏览器的包名

```powershell
adb shell pm list package -3
```

获取信息如下：

```powershell
package:com.android.chrome
```

这里的 `com.android.chrome` 才是卸载时要用到的包名。

```powershell
adb uninstall com.android.chrome
```

如果出现 `Success` 的提示，就表示卸载成功了。

#### 传输文件 push/pull

通过 adb 工具可以实现电脑与安卓设备（模拟器）之间的文件传输。

把文件从电脑复制到安卓设备

```powershell
adb push F:\\tools\\com.android.chrome_404411700.apk /sdcard/
```

到 /sdcard/ 目录下查看是否传输成功

```powershell
adb shell # 进入安卓设备的 shell 终端

# 进入 /sdcard/ 目录并查看该目录否有安装包
cd /sdcard/ && ls -al | grep chrome 
```

获取信息如下：

```powershell
-rw-rw---- root     sdcard_rw 48667070 2020-05-28 19:45 com.android.chrome_40441
1700.apk
```

把文件从安卓设备复制到电脑

```powershell
# 把文件复制到电脑的 d 盘根目录
adb pull /sdcard/com.android.chrome_404411700.apk d:\\
```

#### adb shell

`adb shell` 表示进入安卓的 shell 终端，这就像我们通过 xshell 进入 Linux 的 shell 终端一样。

#### 截屏录屏

```powershell
adb shell screencap /sdcard/screen.png # 截屏
adb shell screenrecord # 录屏
```

#### 管理软件包 pm

查看所有已安装的软件包名

```powershell
adb shell pm list package
# 或者
adb shell # 进入安卓设备 shell 终端
pm list package # 列出所有软件包名
```

查看第三方软件，即用户自行安装的不是 Android 自带的软件

```powershell
adb shell pm list package -3
```

安装与卸载的用法跟上文一致

```powershell
adb shell pm install F:\\tools\\com.android.chrome_404411700.apk
adb shell pm uninstall com.android.chrome
```

#### 查看系统信息 dumpsys

查看 cpu 的使用情况

```powershell
adb shell dumpsys cpuinfo
```

查看磁盘的使用状态

```powershell
adb shell dumpsys diskstats
```

查看电池使用信息

```powershell
adb shell dumpsys batterystats
```

获取电量

```powershell
adb shell dumpsys battery
```

切换手机为非充电状态

```powershell
adb shell dumpsys battery set status 1 # status 1 表示非充电状态
```

切换手机为充电状态

```powershell
adb shell dumpsys battery set status 2 # status 2 表示充电状态
```

查看每个APP的内存使用和系统内存状态

```powershell
adb shell dumpsys meminfo
```

查看某个APP的内存使用和系统内存状态

```powershell
adb shell dumpsys meminfo com.android.chrome
```

## 猴子测试

### 简介

Monkey 意指猴子，顽皮淘气。所以 Monkey 测试，顾名思义也就像猴子一样在软件上乱敲按键，猴子什么都不懂，就爱捣乱。Monkey 原理也是类似，通过向系统发送伪随机的用户事件流（如按键输入、触摸屏输入、滑动Trackball、手势输入等操作），来对设备上的程序进行压力测试，检测程序多久的时间会发生异常。一般在功能测试完成后进行 Monkey 测试。

Monkey 程序由 Android 系统自带，使用 Java 语言写成，在 Android 文件系统中的存放路径是：`/system/framework/monkey.jar`。

monkey.jar 程序是由一个名为 monkey 的 shell 脚本来启动执行，shell 脚本在 Android 文件系统中的存放路径是：`/system/bin/monkey`。

我们可以通过在 cmd 窗口中执行： adb shell monkey ｛+命令参数｝来进行 Monkey 测试。

### 原理

在Monkey运行的时候，它生成事件，并把它们发给系统。同时，Monkey还对测试中的系统进行监测，对下列三种情况进行特殊处理（自动停止）：

1. 如果限定了Monkey运行在一个或几个特定的包上，那么它会监测试图转到其它包的操作，并对其进行阻止；

2. 如果应用程序崩溃或接收到任何失控异常，Monkey将停止并报错；

3. 如果应用程序产生了应用程序不响应(application not responding)的错误，Monkey将会停止并报错；

Monkey 运行在设备或模拟器上面，可以脱离 PC 运行（普遍做法是将 Monkey 作为一个向待测应用发送随机按键消息的测试工具。验证待测应用在这些随机性的输入面前是否会闪退或者崩溃）

{% asset_img monkey.png "Monkey 运行原理" %}

### 参数

{% asset_img monkey-params.png "Monkey 参数" %}

Event percentages（事件百分比）:

0：点击事件百分比，即参数 --pct-touch

1：滑动事件百分比，即参数 --pct-motion

2：缩放事件百分比，即参数 --pct-pinchzoom

3：轨迹球事件百分比，即参数 --pct-trackball

4：屏幕旋转事件百分比，即参数 --pct-rotation

5：基本导航事件百分比，即参数 --pct-nav

6：主要导航事件百分比，即参数 --pct-majornav

7：系统按键事件百分比，即参数 --pct-syskeys

8：Activity启动事件百分比，即参数 --pct-appswitch

9：键盘唤出隐藏事件百分比，即参数 --pct-flip

10：其他事件百分比，即参数 --pct-anyevent

Monkey参数的约束限制规范：

**-p：** 一个 -p 选项只能用于一个包，指定多个包，需要使用多个 -p 选项；

**-s：** 伪随机数生成器的 seed 值，如果用相同的seed值再次运行monkey，它将生成相同的事件序列，对9个事件分配相同的百分比；

**-v：** 命令行的每一个 -v 将增加反馈信息的级别：

+  Level 0 为一个 -v 的命令，除了启动的提示、测试完成和最终结果之外，提供较少的信息 ;
+ Level 1 为两个 -v 的命令，提供较为详细的测试信息，如逐个发送到 Activity 的事件 ；
+ Level 2 为三个 -v 的命令，提供更加详细的测试信息，如测试中被选中或未被选中的 Activity；

### 实战

常见命令组合：

1. `monkey -p com.package -v 500` ：简单的输出测试的信息；

2. `monkey -p com.package -v -v -v 500` ：以深度为三级输出测试信息；

3. `monkey -p com.package --port 端口号 -v` ：为测试分配一个专用的端口号，不过这个命令只能输出跳转的信息及有错误时输出信息；

4. `monkey -p com.package -s 数字 -v 500` ：为随机数的事件序列定一个值，若出现问题下次可以重复同样的系列进行排错；

5. `monkey -p com.package -v --throttle 3000 500` :为每一次执行一次有效的事件后休眠3000毫秒；

Monkey 参考命令：

```powershell
adb shell monkey -p com.tencent.XXX(替换包名) --throttle 500 --ignore-crashes --ignore-timeouts --ignore-security-exceptions --ignore-native-crashes -v -v -v 1000000 > d:\\monkeyScreenLog.log
```

## 专项测试

### 应用启动时间测试

从点击应用的启动图标开始创建出一个新的进程直到我们看到了界面的第一帧，这段时间就是应用的启动时间。
我们要测量的也就是这段时间，测量这段时间可以通过命令的方式进行测量，这种方法测量得最为精确，命令如下：

```powershell
adb shell am start -W -n app.activity # APP.activity 指被测 APP 的 activity
```

我们可以通过下面的命令来获得 APP 的 activity：

```powershell
adb shell dumpsys activity | find "mFocusedActivity"
```

上面的命令是获取已经启动的 APP 的 activity。先在模拟器上启动谷歌浏览器，这样我们才能获取 activity。启动浏览器后，执行上面的命令，获取的信息如下：

```powershell
mFocusedActivity: ActivityRecord{a85f429 u0 com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity t8}
```

其中 `com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity` 就是我们需要的 activity。现在我们来执行上面的启动程序并获取启动时间的命令。前面我们为了获取 activity，已经把谷歌浏览器启动了，我们得先把模拟器中的谷歌浏览器关闭，才能获得其启动时间。我们可通过命令来关闭浏览器：

```powershell
adb shell am force-stop com.android.chrome
```

关闭浏览器后，执行以下命令：

```powershell
adb shell am start -W -n com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity
```

获取的信息如下：

```powershell
Starting: Intent { cmp=com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity }
Status: ok
Activity: com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity
ThisTime: 3558
TotalTime: 3558
WaitTime: 3637
Complete
```

这里有三个时间，我们一般取 TotalTime 来作为应用的启动时间。现在我们只获取了一个启动时间，要想准确地获取程序的启动时间，需要获取多次时间来评估应用的启动时间。你可以手动执行多次来获取，也可以通过编写 Python 脚本来自动化获取。

