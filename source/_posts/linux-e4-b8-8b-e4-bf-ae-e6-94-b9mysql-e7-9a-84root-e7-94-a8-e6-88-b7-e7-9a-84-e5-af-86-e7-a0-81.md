---
title: Linux下修改Mysql的root用户的密码
tags:
  - mysql
id: 238
comment: false
categories:
  - 未分类
date: 2017-07-22 09:55:50
---

<span class="s1">首先，你必须要有操作系统的root权限了。要是连系统的root权限都没有的话，先考虑root系统再走下面的步骤。</span>

<span class="s1">**mysqld_safe --skip-grant-tables &amp;**</span>

<span class="s1">&amp;，表示在后台运行，不再后台运行的话，就再打开一个终端咯。</span>

<span class="s1">**mysql**</span>

<span class="s1">mysql&gt; use mysql;</span>

<span class="s1">mysql&gt; UPDATE user SET password=password("test123") WHERE user='root';</span>

<span class="s1">mysql&gt; flush privileges;</span>

<span class="s1">mysql&gt; exit;</span>