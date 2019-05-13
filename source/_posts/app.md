title: mysql 生成备份文件
date: 2019-05-13 11:50:26
tags: 备份文件、mysql
categories: mysql 
---

#脚本如下

```mysql
#! /bin/bash

#host
host="127.0.0.1"
# uname
user="root"
# pwd
userPWD="123456"
#port
port="3306"

# dbnames
dbNames=(dbName1 dbName2)

# now time
NOW=`date -d "now" +%Y%m%d%H`

# back path
back_path=/var/wwwroot/mysql/$NOW

# create new back data directory
mkdir $back_path
# Add authority to execute
chmod -R 777 $back_path

# When to Back up Data (one day ago)
whenDel=`date -d "-1 day" +%Y%m%d%H`

del_path=/var/wwwroot/mysql/$whenDel

# 
if [ -d $del_path ];
    then
        rm -rf $del_path
fi

# backup 
for dbName in ${dbNames[*]}
do
    dumpSqlFile=$dbName-$back_path.sql.gz
    mysqldump -h$host:$port -u$user -p$userPWD $dbName | gzip > $back_path/$dumpSqlFile
done
```