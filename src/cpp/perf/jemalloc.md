# jemalloc

percona使用jemalloc
最新的percona已经开关支持jemalloc进行内存分析了：

Jemalloc Memory Allocation Profiling - Percona Server for MySQL

Jemalloc memory profiling integration by efirs · Pull Request #1496 · percona/percona-server (github.com)

步骤：

1.重新编译jemalloc

2.修改mysqld配置文件，jemalloc_profiling=1

3.重启mysqld： LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2 MALLOC_CONF="prof:true" mysqld &

4.导出profile文件

mysql> show variables like '%jemalloc%';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| jemalloc_detected  | ON    |
| jemalloc_profiling | ON    |
+--------------------+-------+
2 rows in set (0.00 sec)
​
mysql> flush memory profile;
Query OK, 0 rows affected (0.01 sec)
​
[root@/ #]ls /tmp/jeprof_mysqld*
/tmp/jeprof_mysqld.1.0.170013202213
[root@/ #]jeprof --dot /usr/sbin/mysqld /tmp/jeprof_mysqld.1.0.170013202213 > /tmp/jeprof1.dot
[root@/ #]dot -Tpng /tmp/jeprof1.dot > /tmp/jeprof1.png


## 下面是老的记录，mysql的mysqld是静态编译的jemalloc。

mysql使用jemalloc
1.编译jemalloc支持profile

./configure --enable-prof --enable-stats --enable-debug --enable-fill

make -j4

2.设置环境变量，执行malloc_conf

export MALLOC_CONF=prof:true,prof_final:true,prof_leak:true,lg_prof_interval:24

3.指定mysqld使用新编译的jemalloc动态库（mysql需要重新编译内核）

export LD_PRELOAD=/home/www/MySQL-8.0/mysql/depend/jemalloc/jemalloc-5.2.1/lib/libjemalloc.so

4.手动启动mysql

./mysqld --defaults-file=/home/www/pro-runmysql/phi/etc/my_3603.cnf --basedir=/home/www/pro-runmysql/phi --datadir=/home/www/pro-runmysql/phi/var/3603/dbdata_raw/data --plugin-dir=/home/www/pro-runmysql/phi/lib/plugin --log-error=/home/www/pro-runmysql/phi/log/3603/dblogs/mysqld.err --open-files-limit=100000 --pid-file=/home/www/pro-runmysql/phi/var/3603/prod/mysql.pid --socket=/home/www/pro-runmysql/phi/var/3603/prod/mysql.sock --port=3603

5.在目录下会生成profile文件



6.jeprof分析内存使用

编译jemalloc会生成jeprof分析程序

/home/www/MySQL-8.0/mysql/depend/jemalloc/jemalloc-5.2.1/bin/jeprof mysqld jeprof.17213.9.i9.heap



7.如果觉得不够直观，可以生成图片

/home/www/MySQL-8.0/mysql/depend/jemalloc/jemalloc-5.2.1/bin/jeprof --show_bytes --pdf mysqld jeprof.17213.20.i20.heap > app-profiling.pdf
