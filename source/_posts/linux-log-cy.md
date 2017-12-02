---
title: linux环境下查看日志常用命令
date: 2017-10-13 08:00:00
tags: linux
---

linux环境下查看日志必不可少，简单整理了一些常用的命令，若有需要可直接拿来使用，如果还有其他觉得不错的需要补充的，请在微信公众号测试架构师后 台回复给我，谢谢！

tail -f 87testing.log#默认查看最新10条日志记录并实时刷新

tail 87testing.log -n 100 #查看最新100条日志记录

tail -f 87testing.log -n 100 #查看最新100条日志记录并实时刷新

tail 87testing.log -n +100 #查看从第100行开始，后面的所有日志记录

head 87testing.log -n 100 #查看前面100行日志记录

cat -n 87testing.log |grep "关键词" #查看到关键词相关日志及行号

cat -n 87testing.log |grep "关键词" |more #分页显示，按空格键可翻页

cat 87testing.log | tail -n +200 | head -n 100 #从200开始，显示200行到299行的日志记录

sed -n '200,299p 87testing.log #从200开始，显示200行到299行的日志记录

sed -n '/2017-10-11 00:00:00/,/2017-10-11 01:23:23/p' 87testing.log #查看某一时间段内的日志记录（两个日期必须在日志中存在，不然会是失效）

tail -f 87testing.log |grep --color=auto -i 关键词 #实时日志记录中，将关键词设置高亮

tail -f 87testing.log |grep -v 关键词 #反向查找，查询实时日志中不包含关键词的行的日志记录

grep 关键词 -B2 -A1 87testing.log #查询日志文件中，关键词所在行及前2行后1行的日志记录