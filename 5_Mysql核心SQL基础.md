#### 结构化查询语言SQL
结构化查询语言SQL是关系型数据库的通用语言  
SQL(Structure Query Language)结构化查询语言
分以下3个类别：
1.DDL(Data Definition Language)  
定义不同的数据库、表、列、索引等数据库对象的定义

2.DML(Data Manipulation Language)  
增、删、改、查

3.DCL(Data Control Language)  
控制不同的许可和访问级别


### 库操作
**工作中一般把关键字全大写，自己定义的名字用小写**


数据库组织方式：  
> RDBMS(关系型数据库管理系统)   
&emsp;&emsp;数据库1&emsp;&emsp;库名  
&emsp;&emsp;&emsp;&emsp;table1  
&emsp;&emsp;&emsp;&emsp;table2  
&emsp;&emsp;数据库2&emsp;&emsp;库名  
&emsp;&emsp;&emsp;&emsp;table3  
&emsp;&emsp;&emsp;&emsp;table4  


查询有哪些数据库

> show databases;
 

选择使用具体哪个数据库

> use DataBaseName
>
> 结尾加不加分号都可以

查看当前数据库中的表

> show tables;

创建数据库
> create database DataBaseNase;
>
> 一次只能创建一个数据库，因此database不用加s   

删除数据库
> drop database DataBaseName;
>
> 一次只能删除一个数据库，因此database不用加s   
> 删除数据库会连同数据库下面的所有的表全部删除  



### 表操作
查看当前数据库下有哪些表
> show tables;

创建表
> create table user(  
	id int unsigned primary key not null auto_increment,  
	name varchar(50) unique not null,  
	age tinyint not null,  
	sex enum('M','F') not null  
)engine=INNODB default charset=utf8;  
>
> 要注意中英文标点，且最后一条语句后不用逗号，结尾用分号


查看表
> desc user;  
> desc user\G

删除表
> drop table user;  
drop会将表整个删除

显示创建表的SQL语句
> show create table user\G


### CRUD(增删改查)操作
#### 增加操作
方法一：
> insert into user(name,age,sex) values('chen',24,'M');  
insert into user(name,age,sex) values('cheng',124,'M');

方法二：
> insert into user(name,age,sex) values('chen',22,'M'),('cheng',32,'F'),("wang",11,'F');  
与方法一相比，省了多次SQL client和server TCP三次握手的次数

#### 删除操作
> delete from user where id = 1;  
只是删除表的数据或表中数据项，表依然存在  
where是对数据的过滤  

> delete from user;  
删除表中的所有数据


#### 更新操作
> update user set age = age + 1;  
所有成员的age项+1

> update user set age = age + 1 where name = 'chen';  
姓名为chen的成员age+1


#### 查询操作
> select * from user;  
查看整张表

> select name,age from user;  
查询指定字段的值

> select * from user where age > 20;  
查询age>20的所有项

> select name from user where age > 18 and sex = 'F';  
查询age>18并且sex='F'的项的name属性

> select * from user where age between 12 and 32;  
between a and b的查询结果是[a,b],两端都能取到

> select * from user where name like "ch%";  
如果写为name="ch%",绝对匹配，匹配ch%的那么  
通配符匹配要用like关键字，%是匹配任意多个字符，_是匹配一个字符

> select name ,age,sex from user where name is null;  
判断null不能用=等号判断，要用is判断

> select name,age from user where age in (18,19);  
子查询，用in判断结果在不在括号里

> select * from user where age not in (18,19);


> select distinct age from user where age > 16;  
去重，distinct关键字应用于所有列，而不仅是前置他的列

> select name,age,sex from user where age > 16 union all select name,age,sex from user where sex='M';  
联合查询



#### 带in子查询/嵌套查询


#### explain
可以查看MYSQL的执行计划，大致显示SQL的执行过程和关键信息
> explain select * from user where sex = 'F';  
explain无法显示一些MySQL优化，如limit


#### 分页查询limit
对于有索引的属性，直接从索引树上查找，  
对于没有索引的属性，limit可以优化查询，能起到*立即停*的作用  
> select * from user limit 3;  
从第0列开始显示表的3列，等同下面的语句  
> select * from user limit 3 offset 0;

> select * from user limit 1,3;  
从第1列开始显示表的3列，等同下面的语句   
> select * from user limit 3 offset 1;

> select * from user sex = 'F' limit 1;  
加了limit，因此MySQL从上往下查询的时候，查到第一个后就结束查询，而不会整表查询，进行了部分优化

offset偏移会对性能造成查找时间花费不均衡，因此一般写成下面这样通过索引抵消offset的不均衡，因为索引是常量时间  
> select * form user_t where id > 上一页最后一条数据的id limit 20


#### 排序order by
> select * from user order by name;  
默认是升序  
select * from user order by name asc;  
显式表明升序  
select * from user order by name desc;  
显式表明降序  


#### 分组group by
> select name,age,count(age) as number from user group by age;
```
+-------+-----+--------+
| name  | age | number |
+-------+-----+--------+
| chen  |  22 |      1 |
| cheng |  32 |      1 |
| wang  |  11 |      1 |
| li    |  18 |      2 |
+-------+-----+--------+
```
> group by按age分组，count(age)统计相同age出现的次数，count(age)as number给count(age)命别名

> select age,count(age) as num from user group by age having age > 16;  
having作为判断条件,上下两条语句相同    
select age,count(age) as num from user where age > 16 group by age desc;

#### 生成大量测试数据
> delimiter $  
设置以$为结尾符  
create Procedure add_t_user(in n int)
begin
declare i int;
set i =0;
while i<n do
insert into user_t values(null,concat(i+1,'@fixbug.com'),i+1);
set i = i +1;
end while;
end$  
创建函数  
delimiter ;  
恢复MySQL用;结尾  
call add_t_user(20000);






