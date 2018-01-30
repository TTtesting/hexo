---
title: windows下mysql数据库安装及配置
date: 2018-01-30 10:00:00
tags: mysql
---
1、首先下载对应windows系统的数据库免安装包：mysql-5.7.21-winx64.zip
下载地址：https://dev.mysql.com/downloads/file/?id=474496

2、下载完成，解压到合适的目录中，比如我放在D盘mysql目录下D:\mysql

3、在D:\mysql\mysql-5.7.21-winx64目录下新建一个空的data文件夹，再在D:\mysql\mysql-5.7.21-winx64目录下创建一个文件mysql.ini,mysql.ini文件内的内容为：
```
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8 
[mysqld]
#设置3306端口
port = 3306 
# 设置mysql的安装目录
basedir=D:/mysql/mysql-5.7.21-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:/mysql/mysql-5.7.21-winx64/data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB

```
4、cmd命令进入到D:/mysql/mysql-5.7.21-winx64/bin目录下，执行：mysqld --initialize-insecure

5、再输入mysqld -install，会显示Service successfully installed.

6、最后输入net start mysql 启动mysql服务（或者服务列表中找到mysql手动启动或停止服务）
D:\mysql\mysql-5.7.21-winx64\bin>net start mysql
MySQL 服务正在启动 .
MySQL 服务已经启动成功。

7、mysql设置密码：mysqladmin -u root password 123456（密码设置为123456）

8、再输入：mysql -u root -p，此时会提示Enter password：，输入密码即可进入mysql

9、查看mysql里面的数据库有哪些：show databases; 
```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+

```