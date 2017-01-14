title: mysql绿色版的安装配置
date: 2014-10-29 09:12:25
tags: mysql
categories: 精华转载
---
本文出自于：http://johnnyhg.javaeye.com/blog/245544
一、下载MySQL
http://www.mysql.org/downloads
我下载的是mysql-noinstall-5.0.67-win32.zip
二、安装过程
 1、解压缩 mysql-noinstall-5.0.67-win32.zip 到一个C盘，重新命名为 MySQL5 。
 假定MYSQL_HOME=C:/ MySQL5
 2、编辑mysql的运行配置文件my.ini，如果没有，可以拿my-medium.ini复制然后更名成 my.ini 
 <!--more-->
```
# Example MySQL config file for medium systems.
#
# This is for a system with little memory (32M - 64M) where MySQL plays
# an important part, or systems up to 128M where MySQL is used together with
# other programs (such as a web server)
#
# You can copy this file to
# /etc/my.cnf to set global options,
# mysql-data-dir/my.cnf to set server-specific options (in this
# installation this directory is C:/mysql/data) or
# ~/.my.cnf to set user-specific options.
#
# In this file, you can use all long options that a program supports.
# If you want to know which options a program supports, run the program
# with the "--help" option.

# The following options will be passed to all MySQL clients
[client]
#password	= your_password
port		= 3306
socket		= /tmp/mysql.sock

# Here follows entries for some specific programs

# The MySQL server
[mysqld]
basedir = "C:/MySQL5"
datadir = "C:/MySQL5/data"
default-character-set=utf8

port		= 3306
socket		= /tmp/mysql.sock
skip-locking
key_buffer = 16M
max_allowed_packet = 1M
table_cache = 64
sort_buffer_size = 512K
net_buffer_length = 8K
read_buffer_size = 256K
read_rnd_buffer_size = 512K
myisam_sort_buffer_size = 8M

# Don't listen on a TCP/IP port at all. This can be a security enhancement,
# if all processes that need to connect to mysqld run on the same host.
# All interaction with mysqld must be made via Unix sockets or named pipes.
# Note that using this option without enabling named pipes on Windows
# (via the "enable-named-pipe" option) will render mysqld useless!
# 
#skip-networking

# Disable Federated by default
skip-federated

# Replication Master Server (default)
# binary logging is required for replication
log-bin=mysql-bin

# required unique id between 1 and 2^32 - 1
# defaults to 1 if master-host is not set
# but will not function as a master if omitted
server-id	= 1

# Replication Slave (comment out master section to use this)
#
# To configure this host as a replication slave, you can choose between
# two methods :
#
# 1) Use the CHANGE MASTER TO command (fully described in our manual) -
#    the syntax is:
#
#    CHANGE MASTER TO MASTER_HOST=<host>, MASTER_PORT=<port>,
#    MASTER_USER=<user>, MASTER_PASSWORD=<password> ;
#
#    where you replace <host>, <user>, <password> by quoted strings and
#    <port> by the master's port number (3306 by default).
#
#    Example:
#
#    CHANGE MASTER TO MASTER_HOST='125.564.12.1', MASTER_PORT=3306,
#    MASTER_USER='joe', MASTER_PASSWORD='secret';
#
# OR
#
# 2) Set the variables below. However, in case you choose this method, then
#    start replication for the first time (even unsuccessfully, for example
#    if you mistyped the password in master-password and the slave fails to
#    connect), the slave will create a master.info file, and any later
#    change in this file to the variables' values below will be ignored and
#    overridden by the content of the master.info file, unless you shutdown
#    the slave server, delete master.info and restart the slaver server.
#    For that reason, you may want to leave the lines below untouched
#    (commented) and instead use CHANGE MASTER TO (see above)
#
# required unique id between 2 and 2^32 - 1
# (and different from the master)
# defaults to 2 if master-host is set
# but will not function as a slave if omitted
#server-id       = 2
#
# The replication master for this slave - required
#master-host     =   <hostname>
#
# The username the slave will use for authentication when connecting
# to the master - required
#master-user     =   <username>
#
# The password the slave will authenticate with when connecting to
# the master - required
#master-password =   <password>
#
# The port the master is listening on.
# optional - defaults to 3306
#master-port     =  <port>
#
# binary logging - not required for slaves, but recommended
#log-bin=mysql-bin

# Point the following paths to different dedicated disks
#tmpdir		= /tmp/		
#log-update 	= /path-to-dedicated-directory/hostname

# Uncomment the following if you are using BDB tables
#bdb_cache_size = 4M
#bdb_max_lock = 10000

# Uncomment the following if you are using InnoDB tables
#innodb_data_home_dir = C:/mysql/data/
#innodb_data_file_path = ibdata1:10M:autoextend
#innodb_log_group_home_dir = C:/mysql/data/
#innodb_log_arch_dir = C:/mysql/data/
# You can set .._buffer_pool_size up to 50 - 80 %
# of RAM but beware of setting memory usage too high
#innodb_buffer_pool_size = 16M
#innodb_additional_mem_pool_size = 2M
# Set .._log_file_size to 25 % of buffer pool size
#innodb_log_file_size = 5M
#innodb_log_buffer_size = 8M
#innodb_flush_log_at_trx_commit = 1
#innodb_lock_wait_timeout = 50

[mysqldump]
quick
max_allowed_packet = 16M

[mysql]
no-auto-rehash
# Remove the next comment character if you are not familiar with SQL
#safe-updates

[isamchk]
key_buffer = 20M
sort_buffer_size = 20M
read_buffer = 2M
write_buffer = 2M

[myisamchk]
key_buffer = 20M
sort_buffer_size = 20M
read_buffer = 2M
write_buffer = 2M

[mysqlhotcopy]
interactive-timeout
```
[mysqld]
设置mysql的安装目录
`basedir=$MYSQL_HOME`
设置mysql数据库的数据的存放目录，必须是data，或者是//xxx/data
`datadir=$MYSQL_HOME/data`
设置mysql服务器的字符集
`default-character-set=utf8`
 
[client]
设置mysql客户端的字符集
`default-character-set=gbk`

3、安装mysql服务
从MS-DOS窗口进入目录E:/myserver/mysql-5.0.37-win32/bin，运行如下命令：
`mysqld --install mysql --defaults-file=C:/MySQL5/my.ini`

4、启动mysql数据库
还在上面的命令窗口里面，输入命令：`net start mysql`
这样就启动了mysql 服务。

5、停止服务
执行 `net stop mysql` 即可

6、删除服务
执行`mysqld --remove mysql` 即可

7、修改密码
以上安装完毕之后，默认的root用户密码为空的。为了安全起见，需要设置一个root用户的密码。(修改完成后记得重启服务才能使密码生效)

```
C:/>cd MySQL5

C:/MySQL5>cd bin

C:/MySQL5/bin>mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or /g.
Your MySQL connection id is 3
Server version: 5.0.67-community-log MySQL Community Edition (GPL)

Type 'help;' or '/h' for help. Type '/c' to clear the buffer.

mysql> use mysql
Database changed
mysql> update User set Password=PASSWORD('admin') where User='root';
Query OK, 2 rows affected (0.08 sec)
Rows matched: 2  Changed: 2  Warnings: 0

```

PS：配置my.ini的时候可以略有不同。我的安装路径为D:\Server\mysql，附上配置文件如下：
```
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html
# *** DO NOT EDIT THIS FILE. It's a template which will be copied to the
# *** default location during install, and will be replaced if you
# *** upgrade to a newer version of MySQL.

[mysqld]

# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
innodb_buffer_pool_size = 128M

# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
log_bin

# These are commonly set, remove the # and set as required.
basedir = D:/Server/mysql
datadir = D:/Server/mysql/data
port = 3306
server_id = 10


# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
join_buffer_size = 128M
sort_buffer_size = 2M
read_rnd_buffer_size = 2M 

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 

```
