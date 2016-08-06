title: mysql 查询
date: 2015-03-21 14:09:42
categories: code
tags: [mysql]
---

## 查询当天数据
``` bash
select * from tab where FROM_UNIXTIME(fabutime, '%Y%m%d') = 20121217;
```
##  mysql TO_DAYS(date) 函数
### TO_DAYS(date)
给定一个日期date, 返回一个天数 (从年份0开始的天数 )。
 ``` bash
mysql> SELECT TO_DAYS(950501);
 
-> 728779
 ```
 
mysql查询今天、昨天、7天、近30天、本月、上一月 数据
今天
``` bash
select * from 表名 where to_days(时间字段名) = to_days(now());
``` 
昨天
``` bash
SELECT * FROM 表名 WHERE TO_DAYS( NOW( ) ) - TO_DAYS( 时间字段名) <= 1
```
7天
``` bash
SELECT * FROM 表名 where DATE_SUB(CURDATE(), INTERVAL 7 DAY) <= date(时间字段名)
```
近30天
``` bash
SELECT * FROM 表名 where DATE_SUB(CURDATE(), INTERVAL 30 DAY) <= date(时间字段名)
```
本月
``` bash
SELECT * FROM 表名 WHERE DATE_FORMAT( 时间字段名, '%Y%m' ) = DATE_FORMAT( CURDATE( ) , '%Y%m' )
```
上一月
``` bash
SELECT * FROM 表名 WHERE PERIOD_DIFF( date_format( now( ) , '%Y%m' ) , date_format( 时间字段名, '%Y%m' ) ) =1
 ```
#查询本季度数据
``` bash
select * from `ht_invoice_information` where QUARTER(create_date)=QUARTER(now());
```
#查询上季度数据
``` bash
select * from `ht_invoice_information` where QUARTER(create_date)=QUARTER(DATE_SUB(now(),interval 1 QUARTER));
```
#查询本年数据
``` bash
select * from `ht_invoice_information` where YEAR(create_date)=YEAR(NOW());
```
#查询上年数据
``` bash
select * from `ht_invoice_information` where year(create_date)=year(date_sub(now(),interval 1 year));
```