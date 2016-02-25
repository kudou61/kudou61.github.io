title: 常用sql语句
tags:
- sql
---

# 基础
## 创建数据库
CREATE DATABASE database_name
## 删除数据库
drop database db_name
## 创建新表
create table table_name(
colume-1 type-1 [not null] [primary key],
colume-2 type-2 [not null],
...
)
<!--more-->
## 根据已有的表创建新表
* create table table-new **like** table_old
* create table table-new **as** select col1,col2... from table_old definition only

## 删除新表
drop table table_name
## 增加一列
alter table table_name **add column** col_name type
## 添加主键
alter table table_name **add primary key**(col)

## 删除主键
alter table table_name **drop primary key**(col)
## 创建索引
create **[unique] index** idx_name on table_name(col...)
## 删除索引
drop index idx_name
## 创建视图
create view viewname as select statement
## 删除视图
drop view viewname
## 简单查询
* 插入：**insert into** table(field1,field2...) values(value1,value2...)
* 删除：**delete from** table where 范围
* 更新：**update** table **set** field=value where 范围
* 查找：select * from table1 where field like '%value%'(like的用法)
* 排序：select * from table order by field1,field2 [desc(倒序)]
* 总数：select **count** as totalcount from table1
* 求和：select **sum(field)** as sumvalue from table1
* 平均：select **avg(field)** as avgvalue from table1
* 最大：select **max(field)** as maxvalue from table1
* 最小：select **min(filed)** as minvalue from talbe1

## 使用左连接
左外连接（左连接）：结果集包括连接表的匹配行，也包括左连接表的所有行。 
left (outer) join:
select a.a,a.b,a.c,b.c,b.d,b.f from a **LEFT JOIN** b **ON** a.a = b.c
## 使用右连接
右外连接(右连接)：结果集既包括连接表的匹配连接行，也包括右连接表的所有行。 
right （outer） join:
## 全外连接
全外连接：不仅包括符号连接表的匹配行，还包括两个连接表中的所有记录。
full/cross （outer） join： 
# 提升
## 子查询
* select field1,field2 from table1 where field3 in (select field4 from table2)
* select field1,field2 from table1 where field3 = (select field4 from table2)

## 四表联查
select * from a left inner join b on a.a=b.b right inner join c on a.a=c.c inner join d on a.a=d.d where .....
## between的用法
select a,b,c, from table1 where a not between 数值1 and 数值2
## in的用法
select * from table1 where a [not] in (‘值1’,’值2’,’值4’,’值6’)
## 两张关联表，删除主表中已经在副表中没有的信息 
delete from table1 where not exists ( select * from table2 where table1.field1=table2.field1 )
# 技巧
## 重建索引
DBCC REINDEX
DBCC INDEXDEFRAG

# mysql用户管理
## mysql修改密码
mysqladmin -u用户名 -p旧密码 password 新密码
开始时root没有密码，所以-p可以省略
## 用户管理
mysql>use mysql;
## 查看
mysql> select host,user,password from user ;
## 创建
mysql> create user  zx_root   IDENTIFIED by 'xxxxx';   //identified by 会将纯文本密码加密作为散列值存储
## 修改
mysql>rename   user  feng  to   newuser；//mysql 5之后可以使用，之前需要使用update 更新user表
## 删除
mysql>drop user newuser;   //mysql5之前删除用户时必须先使用revoke 删除用户权限，然后删除用户，mysql5之后drop 命令可以删除用户的同时删除用户的相关权限
## 更改密码
mysql> set password for username =password('xxxxxx');
mysql> update  mysql.user  set  password=password('xxxx')  where user='otheruser'
## 查看用户权限
mysql> show grants for username;
## 赋予权限
mysql> grant select on dmc_db.*  to user@host;
赋予用户在dmc_db数据库的select权限
## 回收权限
mysql> revoke  select on dmc_db.*  from  zx_root;  //如果权限不存在会报错
## 刷新服务
flush  privileges ;