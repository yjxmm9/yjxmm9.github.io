# MySQL语句



## 数据库操作

```mysql
create database mybd;//创建数据库

show databases;//查看现有数据库

use mydb;//在该数据库中进行操作

drop database mydb;//删除数据库
```





## 表操作

```mysql
create table emp(
    id int,
    name varchar(20));//创建表

desc emp;//查看表,能看到字段名，字段类型长度，是否可以为空，是否主键，是否联合主键

show create table emp;//查看表,能看到建表语句，里面内容很详细

alter table emp
rename per;//修改表的名称

alter table emp
add ads varchar(40);//添加字段

alter table emp
add ss int first;//添加字段,指定为第一个

alter table emp
add num int after name;//添加字段,指定在谁后面

alter table emp
drop num;//删除字段

alter table emp
modify name varchar(40);//修改字段的变量类型

alter table emp
change name dname varchar(40);//同时修改字段名称和变量类型(变量类型不变则不修改)

alter table emp
modify name varchar(40) first;//修改字段到第一个位置

alter table emp
modify name varchar(40) after ss;//修改字段到指定位置
```





## 索引操作

索引分为:

- 普通索引

- 唯一索引

- 全文索引

- 多列索引

```mysql
create table emp(
    id int,
    name varchar(20),
    index idindex (id));//创建表时创建普通索引
    
explain select * from emp where id=1;//查看索引创建是否成功,若成功possile_keys和key都为索引名

create index nameindex
on emp (name);//在创建表后创建普通索引

alter table emp
add index nameindex(name);//用修改的方式创建普通索引

create table emp(
    id int,
    name varchar(20),
    unique index idindex (id));//创建表时创建唯一索引
    
create unique index nameindex
on emp (name);//在创建表后创建唯一索引

alter table emp
add unique index nameindex(name);//用修改的方式创建唯一索引

//全文索引为fulltext index,创建和查看与上面两种索引同理
//多列索引为索引名后的列内增加其他字段,如index indexname (id,name),创建和查看与上面两种索引同理
```





