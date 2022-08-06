# MySQL高级部分：

## 一、视图

### 1.视图

**作用**：快速查询，隐藏某些信息，保护敏感信息；将一个复杂的查询语句的结果保存，类似于函数，再次查询时只需要查询该视图即可
**创建**：create view (名字) as (查询语句)，视图查询方法与表相同

### 2.显示视图

desc 视图名：显示视图结构
show tables：一般给视图起名时加前缀用于区分表和视图
show create view (视图名)；：查看创建视图时的语句
select * form 视图名：与表查询方式相同

### 3.更新和删除视图

alter  view （名) as （查询语句）
drop view (名)

### 4.视图算法

temptable 临时表算法：如果查询视图时用子查询，结果会有出入，修改视图算法为temptable即可
merge：合并算法



## 二、事务

### 1.事务的提出

**引用自菜鸟教程：MySQL事务主要用于处理操作量大，复杂度高的数据。比如说，在人员管理系统中，你删除一个人员，你既需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱，文章等等，这样，这些数据库操作语句就构成一个事务**

### 2.transaction

开始事务 `start transaction`

更新操作 `update transaction set balance=balance-10 where id=1;`

rollback：在提交以前可回滚

commit：提交

### 3.rollback to 回滚点

设置回滚点：`savepoint name`

回滚操作：`rollback to name` 恢复到name之前的数据保存情况

### 4.ACID

atomicity			原子性		事务的所有SQL操作作为原子工作单元执行，要么全部执行，要么全部不执行

consistency		一致性		事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100

isolation			隔离性			如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离

durability			持久性		即事务完成后，对数据库数据的修改被持久化存储

### 5.注意事项

事务只能在引擎使用innodb时才能使用



## 三、索引

index：用于实现数据的快速检索

创建删除的操作方法与表相同



## 四、存储过程

用来模块化设计，可以用来增删改查，用事务，当作函数来理解

### 1.delimiter

设置语句结束符，设计好存储过程后再还原即可

### 2.procedure

创建存储过程：`create procedure proc()`  

调用存储过程：`call proc()`



## 五、有趣的函数

### 1.number

rand()：生成随机数		ceil()：向上取整		floor()：向下取整		 round(数，保留位数)：四舍五入		truncate( 数字, 位数)：截取指定位数

order by rand()：随机排序

### 2.string

ucase()：小转大		lcase()：大转小		left(' ',位数)：左截取		right()：右截取		substirng('',起始位置,截取长度)：指定截取

concat('', '')：将两个字符串合并		可用于查询时设置条件，组合查询：select concat(字段，‘|’，字段) from table

### 3.others

select now()：查看当前时间

```mysql
mysql> select year(now()) year, month(now()) month, day(now()) day;
+------+-------+------+
| year | month | day  |
+------+-------+------+
| 2022 |     7 |   28 |
+------+-------+------+
```



加密函数：

```mysql
mysql> select sha('fdsfsd');
+------------------------------------------+
| sha('fdsfsd')                            |
+------------------------------------------+
| a3f60445f2031b5cd83534130eeba64cf4a0887b |
+------------------------------------------+
```

