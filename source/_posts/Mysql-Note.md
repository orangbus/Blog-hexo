---
title: Mysql-Note
date: 2017-07-26 20:45:39
tags: 
categories: 
- php
desc: myswli mysql命令 数据库增删改查
---

### MySQL常用命令

> :fa-bookmark: ASCII码：一个英文字母（不分大小写）占一个字节的空间，一个中文汉字占两个字节的空间。
> :fa-bookmark:UTF-8编码：一个英文字符等于一个字节，一个中文（含繁体）等于三个字节。  
> :fa-bookmark:Unicode编码：一个英文等于两个字节，一个中文（含繁体）等于两个字节。

 **1、登陆数据库：mysql -h 主机名 -u 用户名 -p 密码** 

   ** 注意需要大小写，当初总结是为了方便记忆。**

- 创建数据库：create database 数据库名 【其它选项】

  ```
  create database orange_db character set gbk;
  ```

- 查看创建的数据库：show databases;   

- 查看已创建的表的详细信息:describe 表名; 

- 查看数据表列表：show tables [from db_name];

- 查看当前数据表：select database();

- 查看数据表结构：show columns from db_name

- 查看表的内容：select ＊ from db_name; <!--more-->

**2.  **选择操作数库:use 数据库名 （use 可以不加分号）成功后会提示: Database changed** ** 

- 创建数据库表：create table 表名称；

```
create table orange
	（
		id int unsigned not null auto_increment primary key,
		name char(8) not null,
		sex char(4) not null,
		age tinyint unsigned not null,
		tel char(13) null default "-"
	);
```



> null 			字段可以为空     
> not null 		字段不能为空   
> UNSIGNED 		属性就是将数字类型无符号化  
> auto_increment 	自动编号   
> primary key 	主键   
> unique key 		(唯一约束：保证数据的唯一性);

 **3、操作数据库** 

- 这里是列表文本插入数据：insert into 表名 【列明1，列名2，。。】 values （值1，值2，。。）

```
	insert into students values(NULL, "王刚", "男", 20, "13811371377");
	insert into students (name, sex, age) values("孙丽华", "女", 21);
```

- 查询表中数据：select 列名称 from 表名称【查询条件】；

```
 select name, age from students;
	      select * from students;
```

- 按指定条件查询：select 列名称 from 表名称   	
  where 条件；

```
	      select * from students where sex="女";
	      select * from students where age > 21;
	      select * from students where name like "%王%";


- update 表名称 set 列名称=新值 where 更新条件 （表中原来要有值）
```

update students set age=20 where id=5;    
update students set name="orange",age=12 where tel="1353727504";

```
- 删除表中的数据：delete from 表 where 删除条件
```

delete from students where id=2;
delete from students where age>50;
delete from students;(删除所有的数据)

```
- 创建表后修改：alter table 表名 add 列名 列数据类型 [after插入位置]

在表的最后追加address：
```

alter table students add address char(60);

```
在表的age后面添加列 birthday:
```

alter table students add birthday date after age;

```
- 修改列：alter table 表 change 列名称 列新名称 新类型数据；
```

alter table students change tel telphone char(13)default"-";

alter table students change name name char(16)not null;

```
- 删除列：alter table 表 drop 列名；
```

alter table students drop birthday;

```
- 重命名表：alter table 表 rename 新表名；
```

alter table orange rename orangestudio；

```
- 删除整张表：drop table 表名；
```

drop table orange;

```
- 删除整个数据库：drop database 数据库名；
```

drop database orange_db;

```
 **4、修改root用户密码：mysqladmin -u root -p password(旧密码) 新密码；** 


> mysql修改提示符：prompt 提示符
> 	/D  完整的时间
> 	/d  当前的数据库
> 	/h  服务器名称
> 	/u  当前用户
> 
> 显示当前时间：select now();
> 显示当前用户：select user();

![橙子工作室](http://git.oschina.net/uploads/images/2017/0205/195631_6663d774_1206925.jpeg "橙子工作室")
###      欢迎关注橙子工作室，如有错误欢迎大家改正。
	
```

