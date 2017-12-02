---
title: ADB命令实战
id: 54
categories:
  - 未分类
date: 2017-05-21 11:11:30
tags:
---

大家能点击进来，说明还是对ADB有所了解或听说过的，可能也会比较熟练的掌握了这些命令，下面描述如有不对的地方，欢迎指正和交流学习，请多指教！

一、简述

ADB的全称为Android DebugBridge，就是起到调试桥的作用。通过ADB我们可以在Eclipse中方面通过DDMS来调试Android程序，说白了就是debug工具。

二、准备工作

试着搭建下安卓开发环境，网上也有较详细的步骤，这里就不再介绍，具体安卓开发环境搭建可参考：

http://www.cnblogs.com/zoupeiyang/p/4034517.html

三、实践

能看到这里说明ADB对我们实际的工作中还是有作用的，在测试工作当中，尤其接触app native测试的同学，看到Android研发的同学都是用命令的方式给手机安装、卸载应用程序，看起来很高大上的样子，其实，我们也可以的。接下来给大家整理下平常工作中较常用的命令：

1、手机连接安卓设备，在终端命令行中输入adb devices，查看当前连接的设备。

2、 如果要卸载某应用程序，就使用adb uninstall 包名。

3、如果要安装某应用程序，就是用adb install D:&#92;xxxx.apk(apk具体路径)。

4、查看apk包的packageName、versionCode、applicationLabel、launcherActivity、permission等各种详细信息： aapt dump badging apk(apk具体路径)。

5、查看应用CUP占用情况：adb shell top -m 5。

6、Native/Dalvik的Heap 信息：它分别给出的是JNI层和Java层的内存分配情况，如果发现这个值一直增长，则代表程序可能出现了内存泄漏：

adb shell dumpsys meminfo 包名。

7、Monkey测试举例：

adb shell monkey -p 包名-s 20161008 --pct-trackball 10 --pct-nav 10 --pct-majornav 10 --pct-flip 10--pct-appswitch 10 --pct-motion 10 --pct-touch 10 --ignore-crashes--ignore-timeouts --throttle 300 -v -v 1000000 &gt;D:\a.log

具体每个参数的含义可参考：http://blog.csdn.net/dadoneo/article/details/7916649

8、如果中途想关闭Monkey测试？可以使用adb shell ps命令找到com.android.commands.monkey的pid，然后adb shell kill pid（具体的值）就可以结束该测试。