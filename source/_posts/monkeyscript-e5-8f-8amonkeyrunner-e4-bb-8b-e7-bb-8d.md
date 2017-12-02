---
title: MonkeyScript及Monkeyrunner介绍
tags:
  - monkey
  - monkeyrunner
  - monkeyscript
id: 198
categories:
  - 未分类
date: 2017-06-18 21:02:57
---

获取app包名：
adb logcat |grep START

执行Monkey脚本的命令：
adb shell monkey -f &lt;scriptfile&gt; &lt;event-count&gt;

1、Dispatch Trackball命令（轨迹球事件）

Dispatch Trackball(long downtime,long eventide,<span style="color: #ff0000;">int action,float x,float y</span>,float pressure,float size,int metastate,float xprecision,float yprecision,int device,int edgeflags)
action 0代表按下，1代表弹起，x和y代表的坐标点

2、DispatchPointer命令（点击事件）

DispatchPointer(long downtime,long eventide,<span style="color: #ff0000;">int action,float x,float y</span>,float pressure,float size,int metastate,float xprecision,float yprecision,int device,int edgeflags)
action 0代表按下，1代表弹起，x和y代表的坐标点

3、DispatchString(String text)(输入字符串事件)

4、LaunchActivity命令（启动应用）

LaunchActivity(package,Activity)

5、UserWait命令（等待事件）

UserWait(1000)

6、DispatchPress命令（按下键值）

DispatchPress(int keycode) #keycode 66 回车键

举例：
1、启动app
2、点击输入框
3、输入查询词
4、点击搜索按钮
5、等待结果的出现
6、点击clear按钮
#search.script，使用命令adb shell monkey -f search.script 2(注意脚本在电脑上，monkey在手机上跑，需要将脚本push到手机上：adb push serch.script /data/local/tmp;另外，在AndroidManifest.xml中的activity加入android:exported="true",最后，可使用sdk自带工具uiautomatorviewer获取坐标点)

`typ=user`
`count=10`
`speed=1.0`
`start data &gt;&gt;`

`LaunchActivity(com.android.chrome,com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity)`
`UserWait(2000)`
`DispatchPointer(10,10,0,100,100,1,1,-1,1,1,0,0)`
`DispatchPointer(10,10,1,100,100,1,1,-1,1,1,0,0)`
`DispatchString(test)`
`UserWait(1000)`
`DispatchPress(66)`
`UserWait(1000)`
`DispatchPointer(10,10,0,300,100,1,1,-1,1,1,0,0)`
`DispatchPointer(10,10,1,300,100,1,1,-1,1,1,0,0)`
`UserWait(6000)`
`DispatchPointer(10,10,0,400,100,1,1,-1,1,1,0,0)`
`DispatchPointer(10,10,1,400,100,1,1,-1,1,1,0,0)`

**MonkeyRunner:**
1、MonkeyRunner API -alert(警示框)
void alert(string message,string title,string okTitle)

#举例：demo.py 执行使用monkeyrunner demo.py
`#!usr/bin/python`
`#_*_ UTF-8 _*_`
`from com.android.monkeyrunner import MonkeyRunner`
`MonkeyRunner.alert('Hello TT frends','This is title','OK')`

2、MonkeyRunner API -waitForConnection(等待设备连接，有多个device id，需要指明具体哪个设备)
WaitForConnection(float timeout,string deviceid)

3、MonkeyDevice API - drag(拖动)
drag(tuple start,tuple end,float duration,integer steps)

4、MonkeyDevice API - press(按键)
press(string keycode,dictionary type)
keycode名，Down、UP、DOWN_AND_UP

5、MonkeyDevice API -startActivity(启动应用)
startActivity(package+'/'+activity)

6、MonkeyDevice API -touch(点击)
touch(integer x,integer y,integer type)
x坐标值，y坐标值，
type：DOWN,UP,DOWN_AND_UP

7、MonkeyDevice API - type(输入)
type(string message)

8、MonkeyDevice API - takeSnapshot(截屏)
MonkeyImage takeSnapshot()

9、MonkeyImage API - sameAs(图像对比)
boolean sameAs(MonkeyImage other,float percent)

10、MonkeyImage API - writetoFile(保存图像文件)
void writeToFile(string path,string format)

#举例：实现在搜索框中输入查询词，并截图

`#!/usr/bin/python`
`#_*_ UTF-8 _*_`
`from com.android.monkeyrunner import MonkeyRunner,MonkeyDevice,MonkeyImage`
`#连接设备`
`device = MonkeyRunner.waitForConnection(3,"emulator-5554")`
`#启动app`
`device.startActivity("com.android.chrome/com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity")`
`MonkeyRunner.sleep(1)`
`#点击搜索框`
`device.touch(100,100,"DOWN_AND_UP")`
`MonkeyRunner.sleep(1)`
`#输入查询词`
`device.type('test')`
`MonkeyRunner.sleep(1)`
`#点击回车键`
`device.press('KEYCODE_ENTER','DOWN_AND_UP')`
`MonkeyRunner.sleep(1)`
`#点击搜索按钮`
`device.touch(400,100,"DOWN_AND_UP")`
`MonkeyRunner.sleep(5)`
`#截图`
`image = device.takeSnapshot()`
`image.writeToFile('./test.png','png')`
`#点击清除按钮`
`device.touch(300,100,"DOWN_AND_UP")`
`MonkeyRunner.sleep(3)`