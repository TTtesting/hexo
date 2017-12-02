---
title: get、post请求使用ab工具如何做并发测试？
date: 2017-09-20 23:13:47
tags:
---
<p style="line-height: 2em;">
    <span style="font-family: helvetica, arial, freesans, clean, sans-serif; font-size: 13.34px; background-color: rgb(248, 248, 248);">最近确实也有些忙，原来都是利用周末的时间写东西，现在周末的时间被占满，照顾小孩。只能忙中找空，写点东西。</span>
</p>
<p style="line-height: 2em;">
    <span style="font-family: helvetica, arial, freesans, clean, sans-serif; font-size: 13.34px; background-color: rgb(248, 248, 248);">最近正好使用ab性能测试工具测了一个项目，主要用来做并发测试，需求是控制水军用户的并发数，通过nginx配置控制并发量（针对异常ip--双ip），所以为了能够快速上线，就选择了ab工具。觉得ab工具多多少少还是可以快速的运用到工作当中，所以有必要写出来分享给需要的同行。主要介绍下使用ab工具并发发送get和post请求，比如日常工具中做并发测试可以使用（针对活动等相关库存的并发测试）</span>
</p>

`linux下安装ab性能测试工具`
```
yum -y install httpd-tools
```
安装完成后，执行：
```
ab -H 'X-Forwarded-For:2.2.2.2' -n 100 -c 30 http://m.1768.com/?act=index&st=login

```
>`备注`：<br>
<font color=#0A0A0A size=2>2.2.2.2为模拟的ip</font>

查看nginx访问日志：
```
m.1768.com 21.58.201.31 - - [18/Sep/2017:18:57:43 +0800] "GET /?act=index&st=login HTTP/1.0" 200 266 "-" "ApacheBench/2.3" 2.2.2.2,47.94.13.33
```
执行结果如下：
```
[root@iZ9q9Z /]# ab -H 'X-Forwarded-For:2.2.2.2' -n 100 -c 30 http://m.1768.com/?
act=index&st=login
[1] 4515
[root@iZ9otb9Z /]# This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 10test7-admin.stg3.1768.com (be patient).....done


Server Software:        nginx
Server Hostname:        m.1768.com
Server Port:            80

Document Path:          /?act=index    #请求的资源
Document Length:        0 bytes     #文档返回的长度，不包括相应的头

Concurrency Level:      30          #并发数
Time taken for tests:   0.609 seconds   #总请求时长
Complete requests:      100            #总请求数
Failed requests:        0           #失败的请求数
Write errors:           0          #错误
Non-2xx responses:      100
Total transferred:      45900 bytes   #总共传输数据量 
HTML transferred:       0 bytes
Requests per second:    164.18 [#/sec] (mean)   #平均每秒的请求数，重要指标：相当于LR中的每秒事务数，后面的括号中mean表示这是一个平均值
Time per request:       182.725 [ms] (mean)    #平均每个请求消耗的时间，重要指标：LR中的平均事务响应时间
Time per request:       6.091 [ms] (mean, across all concurrent requests)   #上面的请求除以并发数，即服务器平均请求响应时间 在并发量为1时 用户等待时间相同  
Transfer rate:          73.59 [Kbytes/sec] received   #平均每秒多少K，即传输速率

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:       28   32   1.7     32      36
Processing:    70  106  25.5    100     169
Waiting:       70  106  25.5    100     169
Total:        100  138  25.7    134     202

Percentage of the requests served within a certain time (ms)
  50%    134        #50%的请求都在134ms内完成
  66%    143
  75%    153
  80%    163
  90%    180
  95%    188
  98%    193
  99%    202
 100%    202 (longest request)
```

具体每个参数的含义可以参考如下地址，里面有很详细的介绍。
```
http://www.jb51.net/article/59469.htm
```
<!-- more -->
举例
-_*
`get请求`

```
ab -H 'X-Forwarded-For:2.2.2.2' -n 100 -c 10 -C "Cookie: appId=3793; track_u_3793=CS1111QXZ591F5C6066AACC1B57273B8; popbeforelogin=aToxOw%3D%3D; Hm_lvt_a52c9eb6cde4f51aa1212ed955bc723c=1504493440,1504503826,1504580247,1505092652; Hm_lpvt_a52c9eb6cde4f51aa1212ed955bc723c=1505121635; YOUXISID=88811fde2b2d2648ae4b4ddefa7da6f28530cb2a; is_visitor=0; point_games_flag_v9_game_circus_5252037600830=2037600830; exchangeReturnUrl=%2F%3Fact%3Dgame_collection; ucenter=newucenter; newucenter_nologout=1; tExchangeBackUrl=newucenter; PHPSESSID=5vqp5j4knn6mldjqt2fovc3ad5; track_u_3783=37yoo; track_u=37yoo; point_games_flag_v9_pinballwap_5252037600830=2037600830; gameHistoryRecord=a%3A3%3A%7Bi%3A0%3Bi%3A3697%3Bi%3A1%3Bi%3A3783%3Bi%3A2%3Bi%3A3793%3B%7D; tExchangeBacToGame=%2F%3Fact%3Dpinballwap; loginReturnUrl=%2F%3Fact%3Dpinballwap%26track_u%3D37yoo; Hm_lvt_be49684f9d9c2de3c96227f9e25c261b=1503630548,1505359432; Hm_lpvt_be49684f9d9c2de3c96227f9e25c261b=1505375090; Hm_lvt_70e5a3dba732ce8be082655aff6ff1e6=1503455932,1504839932,1505186995; Hm_lpvt_70e5a3dba732ce8be082655aff6ff1e6=1505375091" "http://m.1768.com/?act=pinballwap&act=pinballwap&st=play_once&amount=500&newer=1&rndnum=599719"
```
>`备注：`
>1.cookie内容一定要加个双引号；<br>
>2.请求url加双引号；


-_*
`post请求`

```
ab -H 'X-Forwarded-For:2.2.2.2' -n 1000 -c 100 -C "Cookie: appId=3793; track_u_3793=CS1111QXZ591F5C6066AACC1B57273B8; popbeforelogin=aToxOw%3D%3D; track_u_3783=37yoo; YOUXISID=14fe43e78bd2e7e41934ac8e9fc809ed881af5fd; is_visitor=0; exchangeReturnUrl=%2F%3Fact%3Dgame_collection; track_u=37yoo; ucenter=newucenter; newucenter_nologout=1; tExchangeBackUrl=newucenter; point_games_flag_v9_game_circus_5252037600830=2037600830; PHPSESSID=5vqp5j4knn6mldjqt2fovc3ad5; gameHistoryRecord=a%3A3%3A%7Bi%3A0%3Bi%3A3783%3Bi%3A1%3Bi%3A3697%3Bi%3A2%3Bi%3A3793%3B%7D; tExchangeBacToGame=%2F%3Fact%3Dgame_circus; loginReturnUrl=%2F%3Fact%3Dgame_circus; Hm_lvt_be49684f9d9c2de3c96227f9e25c261b=1503630548,1505359432; Hm_lpvt_be49684f9d9c2de3c96227f9e25c261b=1505469850; Hm_lvt_70e5a3dba732ce8be082655aff6ff1e6=1503455932,1504839932,1505186995; Hm_lpvt_70e5a3dba732ce8be082655aff6ff1e6=1505469852" -p /root/circus.txt -T application/x-www-form-urlencoded "http://m.1768.com/?index.php"
```
`circus.txt文件中保存的参数格式：`
```
act=game_circus&st=start&amount=1300&isNewUser=0&autoBetFlag=0
```
执行结果如下：

```
[root@iz2zfd6z ~]# ab -H 'X-Forwarded-For:2.2.2.2' -n 1000 -c 100 -C "Cookie: appId=3793; track_u_3793=CS1111QXZ591F5C6066AACC1B57273B8; popbeforelogin=aToxOw%3D%3D; track_u_3783=37yoo; YOUXISID=14fe43e78bd2e7e41934ac8e9fc809ed881af5fd; is_visitor=0; exchangeReturnUrl=%2F%3Fact%3Dgame_collection; track_u=37yoo; ucenter=newucenter; newucenter_nologout=1; tExchangeBackUrl=newucenter; point_games_flag_v9_game_circus_5252037600830=2037600830; PHPSESSID=5vqp5j4knn6mldjqt2fovc3ad5; gameHistoryRecord=a%3A3%3A%7Bi%3A0%3Bi%3A3783%3Bi%3A1%3Bi%3A3697%3Bi%3A2%3Bi%3A3793%3B%7D; tExchangeBacToGame=%2F%3Fact%3Dgame_circus; loginReturnUrl=%2F%3Fact%3Dgame_circus; Hm_lvt_be49684f9d9c2de3c96227f9e25c261b=1503630548,1505359432; Hm_lpvt_be49684f9d9c2de3c96227f9e25c261b=1505469850; Hm_lvt_70e5a3dba732ce8be082655aff6ff1e6=1503455932,1504839932,1505186995; Hm_lpvt_70e5a3dba732ce8be082655aff6ff1e6=1505469852" -p /root/circus.txt -T application/x-www-form-urlencoded "http://m.1768.com/?index.php"
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking m.1768.com (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        nginx
Server Hostname:        m.1768.com
Server Port:            80

Document Path:          /?index.php
Document Length:        267 bytes

Concurrency Level:      100
Time taken for tests:   13.657 seconds
Complete requests:      1000
Failed requests:        993
   (Connect: 0, Receive: 0, Length: 993, Exceptions: 0)
Write errors:           0
Total transferred:      365377 bytes
Total body sent:        1098000
HTML transferred:       102377 bytes
Requests per second:    73.22 [#/sec] (mean)
Time per request:       1365.722 [ms] (mean)
Time per request:       13.657 [ms] (mean, across all concurrent requests)
Transfer rate:          26.13 [Kbytes/sec] received
                        78.51 kb/s sent
                        104.64 kb/s total

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:       25   45 125.7     29    1037
Processing:   115  911 1246.9    637    8654
Waiting:      115  911 1246.9    637    8654
Total:        146  957 1248.8    675    8683

Percentage of the requests served within a certain time (ms)
  50%    675
  66%    834
  75%   1028
  80%   1117
  90%   1476
  95%   3128
  98%   7552
  99%   7636
 100%   8683 (longest request)
```
 #####至于用作性能分析建议还是使用LR，有兴趣也可以对性能结果进行分析。

