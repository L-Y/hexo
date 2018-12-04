title: mysql主从复制实现
date: 2018-12-03 15:43:22
categories: mysql
tags: mysql
---
## 原理
- mysql要做到主从复制，其实依靠的是二进制日志，即：假设主服务器叫A，从服务器叫B；主从复制就是B跟着A学，A做什么，B就做什么。那么B怎么同步A的动作呢？现在A有一个日志功能，把自己所做的增删改查的动作全都记录在日志中，B只需要拿到这份日志，照着日志上面的动作施加到自己身上就可以了。这样就实现了主从复制。

## 实现步骤

1.步骤1 
- 打开主服务器(master)mysql配置文件 例：/etc/my.ini
- 在配置文件增加三行
```sql
### 将mysql二进制日志取名为mysql-bin
log-bin=mysql-bin  
### 二进制日志的格式，有三种：statement/row/mixed,具体分别不多做解释，这里使用mixed
binlog_format=mixed 
### 为服务器设置一个独一无二的id便于区分
server-id=100 
```
- 重启mysql
```sql
service mysqld restart
```
- 配置从服务器，与上面一样，不过 server-id要换成 200,这个值你随便设置 在1-2^23之间

2.步骤2
- 为从服务器设置一个可以同步复制主服务器日志的权限
- 命令
```sql
#### 先登陆mysql
mysql -u root -p ******
#### 分配权限 replication slave：分配复制权限 *.*:操作哪个数据库 slave:用户名 %：哪个主机 pass:密码
GRANT replication slave ON *.* TO 'slave'@'%' IDENTIFIED BY 'pass'; 
```

3.步骤3
- 查看主服务器BIN日志的信息
```sql
#### 配置完从服务器之前不要对主服务器进行任何操作，因为每次操作数据库时这两值会发生改变
show master status;
#### 记住`File` 与 Position的值，下面第四步mysql_log_file,与mysql_log_pos要使用.
```

4.步骤4
- 配置从服务器
```sql
#### 先看下从服务器状态
show slave status;
#### 如果存在则执行：
stop slave;
#### 设置
change master to master_host="192.168.1.100",master_user="uname",matser_password="pass",mysql_log_file="mysql-bin-0600",mysql_log_pos=876;
```
- 参数解释：
- `master_host` :设置要连接的主服务器的ip地址 
- `master_user` :设置要连接的主服务器的用户名 
- `matser_password` :设置要连接的主服务器的密码
- `mysql_log_file` :设置要连接的主服务器的bin日志的日志名称，即第3步得到的信息
- `mysql_log_pos` :设置要连接的主服务器的bin日志的记录位置，即第3步得到的信息，（这里注意，最后一项不需要加引号。否则配置失败

5.查看是否成功
```sql
show master status;
#### 成功会提示 Slave_IO_Running: yes | Slave_sql_Running yes
```

## mysql日志的三种模式
1.基于SQL语句的复制(statement-based replication,SBR) `binlog_format=statement`
- 每一条会修改数据的sql都会记录在binlog中。

2.基于行的复制(row-based replication,RBR) `binlog_format=row`
- 它不记录sql语句上下文相关信息，仅保存哪条记录被修改

3.混合模式复制(mixed-based replication,MBR) `binlog_format=mixed`
- 一般的语句修改使用statment格式保存binlog，如一些函数，statement无法完成主从复制的操作，则采用row格式保存binlog，MySQL会根据执行的每一条具体的sql语句来区分对待记录的日志形式，也就是在Statement和Row之间选择一种

4.binlog相关命令
```sql
#### 查看使用的模式
show variables like 'binlog_format'
#### 查看是否开启binlog
show variables like 'log_bin'
#### 获取binlog文件列表
show binary logs
#### 查看当前正在写入的binlog文件
show master status
#### 查看master上的binlog
show master logs
#### 只查看第一个binlog文件的内容
show binlog events
#### 查看指定binlog文件的内容
show binlog events in 'mysql-bin.000002'
####日志被刷新时，新生成一个日志文件
flush logs
```