---
title: Python Django之路与您同行
tags:
  - django
  - python
id: 203
comment: false
categories:
  - 未分类
date: 2017-06-26 22:14:51
---

最近折腾了一个自己的个人博客：87testing.com，这样可以记录自己的学习、工作和生活。主要在这里写下自己的学习笔记、软件测试思考及读书感悟等，后续可能会系统的介绍一些python、django、移动端自动化测试、接口自动化测试、性能测试等。欢迎您的光临！

要想深入测试，必须了解功能逻辑，对数据流及网站架构比较清楚，这点也说过多次，真的很重要，必须要体现在工作当中，养成习惯，绝对不要对自己测试过的功能模块其中的技术实现不清楚。这样测试路会不好走！！！

-----------------------------华丽的分割线----------------------------

今天分享的内容为django，适合初学者，同样也慢慢对网站能够有些了解。以下内容是在mac下操作的(其他系统大同小异)，使用的python自带版本2.7。

*   **mac下安装pip**
在终端Terminal里面输入sudo easy_install pip，回车，就开始进行下载安装，网络好的话几秒钟就安装好。

安装成功后，终端Terminal中直接pip就出出现pip命令的说明信息，说明pip安装成功可以正常使用了。

*   **安装Django**
终端Terminal中输入sudo pip install django，等待安装完成，如图：

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![](http://www.87testing.com/assets/blogimg/Django001.png)

使用命令python -m django --version或者pip show django查看django版本信息，如图：

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![](http://www.87testing.com/assets/blogimg/Django002.png)

*   **使用django创建一个项目，项目名称ceshijiagoushi**
django-admin startproject ceshijiagoushi

新建好项目以后，使用PyCharm打开，也可以看到目录

新建好后的目录说明：

**ceshijiagoushi:** 项目的容器。

**manage.py: **一个实用的命令行工具，可让你以各种方式与该 Django 项目进行交互。

**ceshijiagoushi/init.py: **一个空文件，告诉 Python 该目录是一个 Python 包。

**ceshijiagoushi/settings.py: **该 Django 项目的设置/配置。

**ceshijiagoushi/urls.py: **该 Django 项目的 URL 声明; 一份由 Django 驱动的网站”目录”。

**ceshijiagoushi/wsgi.py: **一个 WSGI 兼容的 Web 服务器的入口，以便运行你的项目。

*   **在ceshijiagoushi项目的容器中创建一个testing应用**
python manage.py startapp testing

应用创建完成后，运行项目：

python manage.py runserver

运行成功并正常启动，如图：

![](http://www.87testing.com/assets/blogimg/Django003.png)

![](http://www.87testing.com/assets/blogimg/Django004.png)![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

有可能会遇到的问题是8000端口被占用，可以将端口号改为8001，那么在重新启动时指定端口号：

python manage.py runserver 127.0.0.1:8001

**备注：**

后续可能还会遇到8001端口被占用的提示，那么可以使用命令：

lsof -i:8001查出端口被哪个程序占用，

然后把端口对应的进程关掉：kill -9 pid(对应的进程号)

*   **如果要在此项目的web页面上打印一句话出来，"Hello，ceshijiagoushi！"**
接下来需要配置几个地方：

1.将testing应用添加到项目中，ceshijiagoushi/settings.py中新增testing：

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![](http://www.87testing.com/assets/blogimg/setting.png)

2.ceshijiagoushi/urls.py，导入testing应用views文件、新增index/路径配置：

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![](http://www.87testing.com/assets/blogimg/urls.png)

3.定义index函数，并通过HttpResponse类向浏览器返回字符串"Hello ceshijiagoushi!"

![](http://www.87testing.com/assets/blogimg/views.png)

以上都修改完成后，启动项目，浏览器访问127.0.0.1:8001/index/ （在这里我启用了8001的端口）就看到打印结果了：

![](http://www.87testing.com/assets/blogimg/printhello.png)

通过上面我们已经知道怎么在页面打印出字符串："Hello ceshijiagoushi!"

*   **将字符串替换成html页面**
接下来将字符串替换成html页面，这样就可以不断的对页面进行优化了。

首先，在testing目录中创建templates目录，然后在templates目录中创建index.html文件。修改下html文件，如图：

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![](http://www.87testing.com/assets/blogimg/templateshtml.png)

其次，views.py文件index函数中的HttpResponse类修改为render函数，request请求对象改成index.html

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

再次访问http://127.0.0.1:8001/index/，页面就展示的index.html页面的内容了。

**用户访问到显示html页面django处理得过程如下：**

1\. 用户通过浏览器请求一个页面

2.请求到达Request Middlewares，中间件对request做一些预处理或者直接response请求

3.URLConf通过urls.py文件和请求的URL找到相应的View

4.View Middlewares被访问，它同样可以对request做一些处理或者直接返回response

5.调用View中的函数

6.View中的方法调用Django的render函数请求index.html，最终返回index.html中的内容。