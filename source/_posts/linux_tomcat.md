---
title: linux下安装配置tomcat环境
date: 2018-01-30 09:00:00
tags: tomcat
---
1、首先下载对应CentOS版本的Tomcat，我下载的是：apache-tomcat-9.0.4
https://tomcat.apache.org/download-90.cgi

2、下载到本地，通过ftp上传到服务器上根目录

3、cp到/usr/local目录下，解压缩：tar xvf apache-tomcat-9.0.4.tar.gz ,重命名：mv apache-tomcat-9.0.4 tomcat

4、vi /etc/profile.d/tomcat.sh
新增：
```
CATALINA_BASE=/usr/local/tomcat
PATH=$CATALINA_BASE/bin:$PATH
export PATH CATALINA_BASE

```
:wq!保存并退出。

执行：
. /etc/profile.d/tomcat.sh

5、配置tomcat配置server.xml：
vim /usr/local/tomcat/conf/server.xml

找到
<Connector port="80" protocol="HTTP/1.1"      //默认端口为8080，改为80            connectionTimeout="20000"            redirectPort="8443" />

由于我80端口被占用了，我改成了81

6、创建测试页面：

mkdir -pv /usr/local/tomcat/webapps/test/WEB-INF/{classes,lib}

7、启动测试：

catalina.sh start 说明tomcat 已经成功安装。