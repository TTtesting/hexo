---
title: PyCharm关联GitHub
tags:
  - PyCharm
  - python
id: 214
comment: false
categories:
  - 未分类
date: 2017-07-02 21:33:13
---

1、terminal中分别输入git及ssh，检查命令是否都有，如果没有自行安装。

2、在pycharm中设置github账号密码
Preferences--&gt;Version Control--&gt;GitHub,
需要填写：
Host:github.com
Login:xxxxx@gmail.com
Password：xxxxxx
点击test,弹出Success

3、在Github账号中添加本机Mac的ssh keys
在Terminal输入以下命令，生成ssh keys：
ssh-keygen -t ras -C xxxx@gmail.com

cd ~/.ssh

ls

ls之后可以看到一个文件id_ras.pub,把其中内容复制出来，登录github网站，settings---&gt;ssh keys新增key，Title随便写，key值为刚刚粘贴的内容。

4、PyCharm向GitHib更新代码
向GitHib创建新的项目，VCS--&gt;Import into Version Control--&gt;Share Project on GitHib

向GitHib更新提交代码，在PyCharm的工具栏有vcs绿色箭头向下、向上图标，一个update一个commit,或者快捷键command+k