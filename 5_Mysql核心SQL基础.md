#### �ṹ����ѯ����SQL
�ṹ����ѯ����SQL�ǹ�ϵ�����ݿ��ͨ������  
SQL(Structure Query Language)�ṹ����ѯ����
������3�����
1.DDL(Data Definition Language)  
���岻ͬ�����ݿ⡢���С����������ݿ����Ķ���

2.DML(Data Manipulation Language)  
����ɾ���ġ���

3.DCL(Data Control Language)  
���Ʋ�ͬ����ɺͷ��ʼ���


### �����
**������һ��ѹؼ���ȫ��д���Լ������������Сд**


���ݿ���֯��ʽ��  
> RDBMS(��ϵ�����ݿ����ϵͳ)   
&emsp;&emsp;���ݿ�1&emsp;&emsp;����  
&emsp;&emsp;&emsp;&emsp;table1  
&emsp;&emsp;&emsp;&emsp;table2  
&emsp;&emsp;���ݿ�2&emsp;&emsp;����  
&emsp;&emsp;&emsp;&emsp;table3  
&emsp;&emsp;&emsp;&emsp;table4  


��ѯ����Щ���ݿ�

> show databases;
 

ѡ��ʹ�þ����ĸ����ݿ�

> use DataBaseName
>
> ��β�Ӳ��ӷֺŶ�����

�鿴��ǰ���ݿ��еı�

> show tables;

�������ݿ�
> create database DataBaseNase;
>
> һ��ֻ�ܴ���һ�����ݿ⣬���database���ü�s   

ɾ�����ݿ�
> drop database DataBaseName;
>
> һ��ֻ��ɾ��һ�����ݿ⣬���database���ü�s   
> ɾ�����ݿ����ͬ���ݿ���������еı�ȫ��ɾ��  



### �����
�鿴��ǰ���ݿ�������Щ��
> show tables;

������
> create table user(  
	id int unsigned primary key not null auto_increment,  
	name varchar(50) unique not null,  
	age tinyint not null,  
	sex enum('M','F') not null  
)engine=INNODB default charset=utf8;  
>
> Ҫע����Ӣ�ı�㣬�����һ�������ö��ţ���β�÷ֺ�


�鿴��
> desc user;  
> desc user\G

ɾ����
> drop table user;  
drop�Ὣ������ɾ��

��ʾ�������SQL���
> show create table user\G


### CRUD(��ɾ�Ĳ�)����
#### ���Ӳ���
����һ��
> insert into user(name,age,sex) values('chen',24,'M');  
insert into user(name,age,sex) values('cheng',124,'M');

��������
> insert into user(name,age,sex) values('chen',22,'M'),('cheng',32,'F'),("wang",11,'F');  
�뷽��һ��ȣ�ʡ�˶��SQL client��server TCP�������ֵĴ���

#### ɾ������
> delete from user where id = 1;  
ֻ��ɾ��������ݻ�������������Ȼ����  
where�Ƕ����ݵĹ���  

> delete from user;  
ɾ�����е���������


#### ���²���
> update user set age = age + 1;  
���г�Ա��age��+1

> update user set age = age + 1 where name = 'chen';  
����Ϊchen�ĳ�Աage+1


#### ��ѯ����
> select * from user;  
�鿴���ű�

> select name,age from user;  
��ѯָ���ֶε�ֵ

> select * from user where age > 20;  
��ѯage>20��������

> select name from user where age > 18 and sex = 'F';  
��ѯage>18����sex='F'�����name����

> select * from user where age between 12 and 32;  
between a and b�Ĳ�ѯ�����[a,b],���˶���ȡ��

> select * from user where name like "ch%";  
���дΪname="ch%",����ƥ�䣬ƥ��ch%����ô  
ͨ���ƥ��Ҫ��like�ؼ��֣�%��ƥ���������ַ���_��ƥ��һ���ַ�

> select name ,age,sex from user where name is null;  
�ж�null������=�Ⱥ��жϣ�Ҫ��is�ж�

> select name,age from user where age in (18,19);  
�Ӳ�ѯ����in�жϽ���ڲ���������

> select * from user where age not in (18,19);


> select distinct age from user where age > 16;  
ȥ�أ�distinct�ؼ���Ӧ���������У���������ǰ��������

> select name,age,sex from user where age > 16 union all select name,age,sex from user where sex='M';  
���ϲ�ѯ



#### ��in�Ӳ�ѯ/Ƕ�ײ�ѯ


#### explain
���Բ鿴MYSQL��ִ�мƻ���������ʾSQL��ִ�й��̺͹ؼ���Ϣ
> explain select * from user where sex = 'F';  
explain�޷���ʾһЩMySQL�Ż�����limit


#### ��ҳ��ѯlimit
���������������ԣ�ֱ�Ӵ��������ϲ��ң�  
����û�����������ԣ�limit�����Ż���ѯ������*����ͣ*������  
> select * from user limit 3;  
�ӵ�0�п�ʼ��ʾ���3�У���ͬ��������  
> select * from user limit 3 offset 0;

> select * from user limit 1,3;  
�ӵ�1�п�ʼ��ʾ���3�У���ͬ��������   
> select * from user limit 3 offset 1;

> select * from user sex = 'F' limit 1;  
����limit�����MySQL�������²�ѯ��ʱ�򣬲鵽��һ����ͽ�����ѯ�������������ѯ�������˲����Ż�

offsetƫ�ƻ��������ɲ���ʱ�仨�Ѳ����⣬���һ��д����������ͨ����������offset�Ĳ����⣬��Ϊ�����ǳ���ʱ��  
> select * form user_t where id > ��һҳ���һ�����ݵ�id limit 20


#### ����order by
> select * from user order by name;  
Ĭ��������  
select * from user order by name asc;  
��ʽ��������  
select * from user order by name desc;  
��ʽ��������  


#### ����group by
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
> group by��age���飬count(age)ͳ����ͬage���ֵĴ�����count(age)as number��count(age)������

> select age,count(age) as num from user group by age having age > 16;  
having��Ϊ�ж�����,�������������ͬ    
select age,count(age) as num from user where age > 16 group by age desc;

#### ���ɴ�����������
> delimiter $  
������$Ϊ��β��  
create Procedure add_t_user(in n int)
begin
declare i int;
set i =0;
while i<n do
insert into user_t values(null,concat(i+1,'@fixbug.com'),i+1);
set i = i +1;
end while;
end$  
��������  
delimiter ;  
�ָ�MySQL��;��β  
call add_t_user(20000);






