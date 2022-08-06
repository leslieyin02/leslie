# MySQL基础部分

## 一、开场吹比

CRUD 

层次模型 网状模型 关系型 

出现箭头时输入分号回车，表示这行语句结束 创建data文件：mysqld --initialize-insecure --user=root（在文件目录下）

## 二、安装连接以及配置

### 1.数据库显示和讲解

net stop/start mysql57 （关闭或启动） 

information_schema：提供对数据库元数据的访问（有关mysql服务器的信息，数据库或表的名称，列的数据类型或访问权限） 

mysql：存储用户信息 

performance_schema：存储服务运行过程中的状态信息（执行的语句，花费的时间，占用的内存） sys：存储系统文件

### 2.创建数据库

create database student

检查是否存在数据库，若不存在则创建数据库（反之不创建，但通过命令）：create database if not exists student

### 3.删除数据库：

drop database student

检查是否存在数据库，若存在则删除数据库（反之不删除，但通过命令）：drop database if exists student 

共同点：强制 创建 / 删除 关键字命名的数据库：加反引号 

### 4.查看创建数据库的SQL

show create database student

### 5.创建数据库指定字符编码

create database student charset=gbk 

查看字符编码时与查看SQL相同，show create database student 

关于字符编码，常用的两种：GBK（中国标准）， UTF-8，windows系统下用GBK（实际开发中一定用UTF-8），Mac或linux系统下用UTF-8

### 6.修改数据库字符编码

alter database student charset=gbk/utf8

## 三、表的基本操作

### 1.引用数据库和查看数据库中的表

user leslie_school 

show tables 

### 2.创建表

create table student(换行) id name age：称为字段 逼格创建表： create table if not exists lesle( id int     auto_increment primary key comment '主键id', name varchar(30) not     null comment '老师姓名', phone varchar(20)     comment '电话号码', address varchar(100) default '暂时未知' comment '住址' )engine=innodb

auto_increment：自动增长且无重复，primary key：(主要的：用于区分，基本的：不能为空)，not null：不能为空，default：设置默认值，engine=innodb：最常用、效率最高的mysql引擎 show create table student：可以查看当时创建表使用的语句 

**注：**必须将该数据库的字符编码格式改为GBK才能在创建表时使用 default ' ' 指定默认值的语句 创建以关键字命名的表同样需要加反引号才能成功创建

### 3.查看表结构

desc student：以表格的形式显示

### 4.删除表 / 多张表：

drop table if exists a, b, c

### 5.修改表

表中一行行数据称为记录实际上是修改表的结构 

增加记录 / 到指定位置： alter table teacher add gender int(12) after （指定位置）/ first（第一行）

### 6.删除记录

alter table teacher drop     gender 

改字段名字和数据类型 / 只改数据类型：alter table teacher     change id（原名）+ 要改的名字 + 类型modify id（原名）+ 新数据类型 

### 7.改表名

alter table +（原表名）+ rename to + 新名字 三级递增查看：show databases（所有数据库） ——> show tables （所有表）——> desc teacher（单个表）

## 四、数据操作

### 1.插入数据

insert into teacher     values() 亦可以在前面加入格式，只要一一对应即可 插入多行数据时，只需在，后面加括号按原格式写入即可 

**注：**自动增长的数据写入null会自增，默认的数据写入default会自动填入创建时指定的数据，非空的数据不能填入null

### 2.字符编码集问题

client 和 result都是gbk（用于练习）set character_set_%=gbk

### 3.删除数据

delete from teacher(表名) + where + 条件（如id>4, name=Tom等等）



### 4.清空表

truncate table + 表名delete from teacher（表名）（一般不用，底层实现太慢） 用truncate删除再写入新数据时从头开始，用delete删除再插入从原来的数据后面开始

### 5.更新数据

update + 表名 + set + 修改的内容 + where + 条件（可用or连接，如果两个条件都满足则都执行）

### 6.查询表数据

select + （条件，如id, name,*全部）from + 表名

### 7.SQL语句区分

DDL data definition language    数据库定义语言：  create alter drop show 

DML data manipulation language    数据操纵语言： insert update delete select     

## 五、数据类型

### 1.数据库的数据类型问题

没有统一标准，但要符合业务项目的逻辑标准

### 2.int 类型

unsigned：无符号正整数类型，范围是左右相加 插入数据时可以超过定义宽度但不能超过原数据类型最大限度

### 3.浮点数

float, double：超过小数位的位数就会四舍五入位数过长会数据丢失

### 4.定点数

decimal（支持无符号，整数小数分开存）

### 5.字符串与文本类型

char：实际上是字符串类型，一般用varchar，可回收剩余空间，但效率不如char高 6.布尔类型：true(1), false(0)

### 6.枚举类型

enum() 只能取给定的类型中的内容插入时可以用数字，枚举类型存储也是用数字，节省空间，限制数据，速度快

### 7.set类型

set(' ', ' ') 插入时可以选多个内容，但必须在一段' '内（存储方式为2的次方）

### 8.时间日期类型

datetime（常用格式） 按格式插入即可

## 六、列属性完整性

### 1.列属性问题

自增的数据删除后无法再使用删除过的自增数字

### 2.Primary Key

唯一不重复，关键特征，查询速度快，与其他表可能有关联，不能为空

### 3.增加主键

alter table + 表名 + add + primary key （1， 2）可以增加多个（）组合（复合键）主键 删除主键，alter table + 表名 + drop primary key 选择主键时一般只要一个，并且是具有代表性易于区分，易于查询（一般是数字）

### 4.unique

唯一键，与其他表无关联，不能重复，可以为空，可存在多个，可以通过alter添加，添加时用add unique +（字段名），删除唯一键用drop index + 字段名

### 5.数据库完整性

有一个主键，数据类型要选对，为不为空，默认值，添加约束，别的表可能会用

### 6.外键

foreign key （并发项目禁止使用外键） 创建表时添加：foreign key (子表->stuId) references     stu(父表)(stuId) 后期可以用alter修改，用add添加即可 一般都是在创建时就设计好

### 7.删除外键

先用show table 查看，然后用alter table + 表名 + drop priamry key + (外键别名) MUL：表示该列的值可重复

### 8.置空操作

置空：将外键关联的从表中的字段置空为NULL，数据还存在 

级联：从表与主表一起更新数据，（彻底删除）主表中的数据删除，与它绑定外键的数据全部删除 删除时一般不使用级联，一般用置空操作

### 9.置空和级联演示

创建时 on delete set null on     update cascade 级联：update stu set stuId='4'     where name='frank'（将主表中id改为4），从表中与stuId级联的数据也变为4 

置空：delete from stu where     stuId='2'，从表中stuId=2的字段置空为NULL，但该字段对应的数据保留

## 七、数据库设计思维

 Codd第一范式，确保每列字段的原子性 

第二范式，每个表必须有主关键字（Primary key），其他数据元素与主关键字一一对应 

第三范式，消除传递依赖，数据元素之间相互独立

## 八、单表查询

### 1.select

随意查询，可作计算，加as可以起别名 from：select * from 表1， 表2返回两张表的笛卡尔积 

### 2.dual

伪表，from dual，可以省略 where：后面可以接各种各样的条件 in：where 字段 in（‘ ’，‘ ’），和or同理 between and ：where + 字段 + between .. and .. 

### 3.聚合函数

select 函数名（条件）from 表名 

常见函数：sum,max,min,count,avg 

### 4.使用客户端图像界面

Navicat，破解工具直接搜 

### 5.模糊查询

where name like '张%' 配合通配符使用，%代表多个字符，_ 代表一个字符

### 6.分组查询

select avg(age) as '年龄',address as '地区' from info group by address 

group by使用条件：有聚合函数，有分组的字段 group_concat(字段名)，可将符合条件的分组聚合 select group_concat(address),gender from info group by gender 

### 7.having

在已经查询到的虚拟表中利用条件再次查询，后面跟的条件名与别名相同 limit 1, 2：1，起始位置，2，长度

### 8..distinct

去重，后跟字段名，可以配合count函数查询去重后的数据个数count（distinct 字段名）

## 九、多表查询

### 1.union

查询的两张表字段名相同，连接两句查询语句，union后可跟distinct去重 distinct：去重，去掉重复值，多个相同的值只显示一次

### 2.inner join（内连接）

将两张表连接，select （查询的字段）from t1 inner join t2 on (两张表的公共字段作连接) t1.id=t2.stuId、 多张表后再加inner join

### 3.left join

以左表为基准（将左表提到的数据全部显示，数据为空的显示为NULL）

### 4.right join

以右表为基准

### 5.cross join

返回笛卡尔积

### 6.natural join

自然连接，将两张有相同的字段名的表连接，显示两张表的其他剩余所有数据，中间可以加left right 自然左连接和自然右连接 如果没有相同字段名，则返回笛卡尔积

### 7.using

当多表查询时出现多个相同字段但要进行连接时，用using(字段名)，使用该字段名连接

一般都将字段名写全，不使用using

## 十、子查询

###  1.嵌套查询

一个查询语句中嵌套另一个查询语句，将返回值作为where后的条件，用in连接，还有not in，in的反面

### 2.exists 和 not exists

父语句 where exists (子语句)子语句只要满足某一条件，父语句就执行

 