---
title: linux下配置java环境
date: 2018-01-30 08:01:00
tags: java
---
1、首先下载对应CentOS版本的jdk，我下载的是：jdk-8u161-linux-x64.tar.gz
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

2、下载该jdk到本地，通过ftp上传到服务器上根目录

3、新建一个jdk的安装目录，cd到/usr/local目录下， mkdir java，将jdk-8u161-linux-x64.tar.gz copy到此目录下：cp ~/jdk-8u161-linux-x64.tar.gz ./

4、解压缩：tar xvf jdk-8u161-linux-x64.tar.gz

5、修改host：vim /etc/profile ,在最后添加：
```
export JAVA_HOME=/usr/local/java/jdk1.8.0_161
export CLASSPATH=$JAVA_HOME/lib/
export PATH=$PATH:$JAVA_HOME/bin
export PATHJAVA_HOME CLASSPATH

```
:wq!保存并退出。

6、更新host文件：source /etc/profile

7、输入java -version  即可看到java版本号，说明已经安装成功了。