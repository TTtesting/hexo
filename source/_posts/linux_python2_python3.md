---
title: linux下python2升级python3，python2和python3并存
date: 2018-01-24 18:35:13
tags: python
---
wget https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz

解压：tar -xzvf Python-3.6.4.tgz

cd Python-3.6.4

/usr/local 创建一个文件夹：mkdir /usr/local/python3

编辑安装：

make
make install

不要覆盖老版本，将老版本的名字修改下

mv /usr/bin/python /usr/bin/python_old

再建立新版本python的链接

ln -s /usr/local/python3/bin/python3 /usr/bin/python3


python2 get-pip.py
python3 get-pip.py 

建立软连接：
ln -s /usr/local/python2/bin/pip2 /usr/bin/pip2
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3

查看python2和python3版本，就是对应安装的版本了
python2 -V
python3 -V

yum命令就不能正常使用了，去修改vim /usr/bin/yum
将第一行#!/usr/bin/python修改为#!/usr/bin/python_old，保存即可。


pip源配置修改：
linux的文件在~/.pip/pip.conf
----------------------------------------
[global]
index-url=http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
----------------------------------------
可以修改为：
http://pypi.douban.com/simple 