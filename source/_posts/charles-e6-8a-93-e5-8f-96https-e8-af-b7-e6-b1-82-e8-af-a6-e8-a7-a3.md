---
title: Charles抓取https请求详解
tags:
  - charles
  - https
id: 171
categories:
  - 未分类
date: 2017-06-11 17:16:35
---

现在基本大部分网站都使用了https，所以要想抓到https的请求，首要任务是先有工具：charles、fiddler，先介绍下charles针对https请求的抓取方法，此方法兼容windows和mac用户（mac用户方法类似，如果有必要，请关注微信公众号：测试架构师，留言给我。后续我会专门再做整理）。

1、 windows下安装charles，看到此文章的用户相信都已经安装了charles，如果还真的没安装，麻烦就自行搜索，进行安装了，打开charles（我使用的charles版本是3.11.4），再继续浏览。

![](http://www.87testing.com/assets/blogimg/charles001.png)

2、 ok，charles已准备完成，接下来就要配置charles证书：

![](http://www.87testing.com/assets/blogimg/charles002.jpg)

之后会弹出安装证书：

![](http://www.87testing.com/assets/blogimg/charles003.jpg)

点击安装，一路下一步，直到提示“导入成功”

![](http://www.87testing.com/assets/blogimg/charles004.jpg)

此时证书还是不被信任的，让不信任变成信任：打开IE浏览器—&gt;工具—&gt;Internet选项—&gt;内容—&gt;证书—&gt;把中级证书颁发机构中的charles证书导出来—&gt;再把导出来的证书导入到受信任的根证书颁发机构中。这样就ok了。

![](http://www.87testing.com/assets/blogimg/charles005.jpg)

![](http://www.87testing.com/assets/blogimg/charles006.jpg)

3、 在移动设备上配置手机代理并安装证书

在手机上设置代理：设置—&gt;无线网络，设置服务器ip和端口号：

![](http://www.87testing.com/assets/blogimg/charles007.jpg)

然后，手机安装证书：

![](http://www.87testing.com/assets/blogimg/charles008.jpg)

会弹出一个提示框，如下：

![](http://www.87testing.com/assets/blogimg/charles009.jpg)

然后手机浏览器输入如上地址：[http://charlesproxy.com/getssl](http://charlesproxy.com/getssl)会弹出如下页面：

![](http://www.87testing.com/assets/blogimg/charles010.png)

点击安装即可，安装完成后就变成已验证，如下：

![](http://www.87testing.com/assets/blogimg/charles011.png)

下面以访问百度wap站点为例：

![](http://www.87testing.com/assets/blogimg/charles012.jpg)

上图看到，访问百度wap站点还是看不到https的请求数据，下面还需要再继续配置：

![](http://www.87testing.com/assets/blogimg/charles013.jpg)

点击SSL Proxying Settings，弹出下面的框，输入Host填写要抓取的ip或域名，port填写443即可。

![](http://www.87testing.com/assets/blogimg/charles014.jpg)

设置完成后，重新方位百度wap站点就可以抓取到https请求了

![](http://www.87testing.com/assets/blogimg/charles015.jpg)

如果以上还没解决，请关注我的微信公众号：测试架构师，后台留言找到我！