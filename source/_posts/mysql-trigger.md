title: mysql 触发器
date: 2015-12-11 22:05:31
categories: mysql
tags:
---

![实例1](/images/1.png)
![实例2](/images/2.png)
![实例3](/images/3.png)
![实例4](/images/4.png)
![实例5](/images/5.png)

说明：
	所需要触发的SQL语句都要以‘；’结束。end后面也需要有分界符，但不能再是'；'。所以创建触发器之前需要先修改分界符

	delimiter 分界符 上文以 $ 为例子

	对于insert，插入一行后出现了一个新行用new表示，在触发器中可以通过new.列名来引用插入行各个字段的值

	对于delete，删除一行后之前的那行不见了，用old表示，在触发器中可以通过old.列名来引用被删除行各个字段的值

	对于update，修改一行后旧的一样用old表示，新的一行用new表示，在触发器中可以通过new.列名来引用修改后行各个字段的值，old.列名来引用修改前各个字段的值。

	触发器after和before的区别：

		after是先完成增删改再触发

		before是在增删改之前将以判断 如 ：库存100 需要购买120 这时 就需要判断 只能购买100 不然库存就会变为 -20;例如:

```
	create trigger tg5 before insert on dingdan for each row 
	begin
	declare tmp int ; #保存库存
	select num into tmp from shop where id=new.id;
	if new.much>tmp #比较购买量 如果大于库存 则=粗存
	then set new.much=tmp;
	end if;
	update shop set num=num-tmp where id=new.id;
	end$
```bash
	更新多件商品 如下:
```
	create trigger more after insert on dingdan for each row
	begin
	update shop set num=num-new.much where id=new.id
	end$
```bash
