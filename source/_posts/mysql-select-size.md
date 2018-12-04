title: mysql容量查询
date: 2018-12-04 16:32:39
categories: mysql
tags: mysql
---

## 查看所有数据库容量大小
```sql
select 
table_schema as `database`,
sum(table_rows) as `records`,
sum(truncate(data_length/1024/1024, 2)) as 'dataLength(MB)',
sum(truncate(index_length/1024/1024, 2)) as 'indexLength(MB)'
from information_schema.tables
group by table_schema
order by sum(data_length) desc, sum(index_length) desc;
```

## 查看所有数据库各表容量大小
```sql
select 
table_schema as 'database',
table_name as 'table_name',
table_rows as 'records',
truncate(data_length/1024/1024, 2) as 'dataLength(MB)',
truncate(index_length/1024/1024, 2) as 'indexLength(MB)'
from information_schema.tables
order by data_length desc, index_length desc;
```

## 查看指定数据库容量大小
```sql
select 
table_schema as 'database',
sum(table_rows) as 'records',
sum(truncate(data_length/1024/1024, 2)) as 'dataLength(MB)',
sum(truncate(index_length/1024/1024, 2)) as 'indexLength(MB)'
from information_schema.tables
where table_schema='mysql';
--------------------- 
作者：傲雪星枫 
来源：CSDN 
原文：https://blog.csdn.net/fdipzone/article/details/80144166 
版权声明：本文为博主原创文章，转载请附上博文链接！
```
## 查看指定数据库各表容量大小
```sql
select 
table_schema as 'database',
table_name as 'table_name',
table_rows as 'records',
truncate(data_length/1024/1024, 2) as 'dataLength(MB)',
truncate(index_length/1024/1024, 2) as 'indexLength(MB)'
from information_schema.tables
where table_schema='mysql'
order by data_length desc, index_length desc;

```