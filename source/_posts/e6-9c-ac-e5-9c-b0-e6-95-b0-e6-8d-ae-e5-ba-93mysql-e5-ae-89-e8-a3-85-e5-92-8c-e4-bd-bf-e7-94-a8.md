---
title: 本地数据库mysql安装和使用
id: 216
comment: false
categories:
  - 未分类
date: 2017-07-02 22:21:42
tags:
---

mac本地数据库mysql的安装：
mysql官方下载链接：https://dev.mysql.com/downloads/mysql/
下载完并安装成功后，会有个默认的数据库账号密码：
root@localhost: waTADIuz0y=w

安装好mysql服务后，打开『系统偏好设置』，点击MySQL图标,在弹出的框中，点击Start MySQL Server,进行启动(如果启动成功，按钮会变成Stop MySQL Server)

在Terminal终端登录MySQL，输入：
TTZhangdeMacBook-Pro:~ admin$ PATH="$PATH":/usr/local/mysql/bin
TTZhangdeMacBook-Pro:~ admin$ mysql -u root -p
此时，会提示输入密码，输入刚刚默认给出的密码即可

展示当前数据库下所有的库：
mysql&gt; show databases;
此时，可能会有报错提示：ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
遇到这种情况，只要输入如下命令重新设置即可解决：
mysql&gt; SET PASSWORD = PASSWORD('123456');

重新执行：show databases; 即可展示出当前数据库下所有的库：

mysql&gt; show databases;
+--------------------+
| Database |
+--------------------+
| information_schema |
| mysql |
| performance_schema |
| sys |
+--------------------+
4 rows in set (0.00 sec)

进入某一个库中，如进入sys这个库
mysql&gt; use sys;
Database changed

查看sys库下所有的表：
mysql&gt; show tables;
+-----------------------------------------------+
| Tables_in_sys |
+-----------------------------------------------+
| host_summary |
| host_summary_by_file_io |
| host_summary_by_file_io_type |
| host_summary_by_stages |
| host_summary_by_statement_latency |
| host_summary_by_statement_type |
| innodb_buffer_stats_by_schema |
| innodb_buffer_stats_by_table |
| innodb_lock_waits |
| io_by_thread_by_latency |
| io_global_by_file_by_bytes |
| io_global_by_file_by_latency |
| io_global_by_wait_by_bytes |
| io_global_by_wait_by_latency |
| latest_file_io |
| memory_by_host_by_current_bytes |
| memory_by_thread_by_current_bytes |
| memory_by_user_by_current_bytes |
| memory_global_by_current_bytes |
| memory_global_total |
| metrics |
| processlist |
| ps_check_lost_instrumentation |
| schema_auto_increment_columns |
| schema_index_statistics |
| schema_object_overview |
| schema_redundant_indexes |
| schema_table_lock_waits |
| schema_table_statistics |
| schema_table_statistics_with_buffer |
| schema_tables_with_full_table_scans |
| schema_unused_indexes |
| session |
| session_ssl_status |
| statement_analysis |
| statements_with_errors_or_warnings |
| statements_with_full_table_scans |
| statements_with_runtimes_in_95th_percentile |
| statements_with_sorting |
| statements_with_temp_tables |
| sys_config |
| user_summary |
| user_summary_by_file_io |
| user_summary_by_file_io_type |
| user_summary_by_stages |
| user_summary_by_statement_latency |
| user_summary_by_statement_type |
| version |
| wait_classes_global_by_avg_latency |
| wait_classes_global_by_latency |
| waits_by_host_by_latency |
| waits_by_user_by_latency |
| waits_global_by_latency |
| x$host_summary |
| x$host_summary_by_file_io |
| x$host_summary_by_file_io_type |
| x$host_summary_by_stages |
| x$host_summary_by_statement_latency |
| x$host_summary_by_statement_type |
| x$innodb_buffer_stats_by_schema |
| x$innodb_buffer_stats_by_table |
| x$innodb_lock_waits |
| x$io_by_thread_by_latency |
| x$io_global_by_file_by_bytes |
| x$io_global_by_file_by_latency |
| x$io_global_by_wait_by_bytes |
| x$io_global_by_wait_by_latency |
| x$latest_file_io |
| x$memory_by_host_by_current_bytes |
| x$memory_by_thread_by_current_bytes |
| x$memory_by_user_by_current_bytes |
| x$memory_global_by_current_bytes |
| x$memory_global_total |
| x$processlist |
| x$ps_digest_95th_percentile_by_avg_us |
| x$ps_digest_avg_latency_distribution |
| x$ps_schema_table_statistics_io |
| x$schema_flattened_keys |
| x$schema_index_statistics |
| x$schema_table_lock_waits |
| x$schema_table_statistics |
| x$schema_table_statistics_with_buffer |
| x$schema_tables_with_full_table_scans |
| x$session |
| x$statement_analysis |
| x$statements_with_errors_or_warnings |
| x$statements_with_full_table_scans |
| x$statements_with_runtimes_in_95th_percentile |
| x$statements_with_sorting |
| x$statements_with_temp_tables |
| x$user_summary |
| x$user_summary_by_file_io |
| x$user_summary_by_file_io_type |
| x$user_summary_by_stages |
| x$user_summary_by_statement_latency |
| x$user_summary_by_statement_type |
| x$wait_classes_global_by_avg_latency |
| x$wait_classes_global_by_latency |
| x$waits_by_host_by_latency |
| x$waits_by_user_by_latency |
| x$waits_global_by_latency |
+-----------------------------------------------+
101 rows in set (0.00 sec)

可以查看其中表中的数据：
mysql&gt; select * from host_summary;
+-----------+------------+-------------------+-----------------------+-------------+----------+-----------------+---------------------+-------------------+--------------+----------------+------------------------+
| host | statements | statement_latency | statement_avg_latency | table_scans | file_ios | file_io_latency | current_connections | total_connections | unique_users | current_memory | total_memory_allocated |
+-----------+------------+-------------------+-----------------------+-------------+----------+-----------------+---------------------+-------------------+--------------+----------------+------------------------+
| localhost | 115 | 49.54 ms | 430.78 us | 6 | 54 | 2.73 ms | 1 | 1 | 1 | 0 bytes | 0 bytes |
+-----------+------------+-------------------+-----------------------+-------------+----------+-----------------+---------------------+-------------------+--------------+----------------+------------------------+
1 row in set (0.03 sec)

如果需要查看MySQL端口号：
mysql&gt; show global variables like 'port';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| port | 3306 |
+---------------+-------+
1 row in set (0.01 sec)

mysql&gt; CREATE DATABASE guest CHARACTER SET utf8;
Query OK, 1 row affected (0.00 sec)

安装pymysql：
sudo pip install pymysql

给电脑安装一个好用的查看数据库的插件吧，由于我是在mac上使用，所以使用插件：Valentina Studio ,windows上可使用Navicat

安装PyMySQL：
TTZhangdeMacBook-Pro:~ admin$ git clone https://github.com/PyMySQL/PyMySQL
TTZhangdeMacBook-Pro:~ admin$ cd PyMySQL/
TTZhangdeMacBook-Pro:PyMySQL admin$ python3 setup.py install