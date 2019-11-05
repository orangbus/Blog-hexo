---
title: 五、tp5_数据库之联表查询
date: 2017-09-06 18:31:39
tags: thinkphp
categories:
- php
desc: tp5_数据库之联表查询
---

## [Tp5联表查询](http://blog.csdn.net/qmhball/article/details/8000003)

> 先说一个案例吧！

table1:  id   name   age   pid

table2: id  nowid  web  other 

联合这两张表，要求table2 的 nowid　显示　table1 的　name 字段的内容

```
//tp5的controller 环境
 $res= Db::('Table2')
      ->alias('t2') //设置一个主表别名
      ->join('Table1 t1' , 't2.id = t1.id')　//添加联表的表名，并设置一个别名t1
      ->field('t1.* , t2.name')　//为安全起见，可以设置以下查询的的字段
      ->select()
assgin('show' , $res);
// 输出　
{$show.name}//这里输出的是table１的那么值
{$show.nowid}//这里输出的是table2的数值
```

> 以下是原文内容<!--more-->

**一.内联结、外联结、左联结、右联结的含义及区别**
在SQL标准中规划的（Join）联结大致分为下面四种：
**1.内联结：**将两个表中**存在联结关系**的字段符合联结关系的那些记录形成记录集的联结。
**2.外联结：**分为外左联结和外右联结。
**左联结A、B表**的意思就是将表A中的全部记录和表B中联结的字段与表A的联结字段符合联结条件的那些记录形成的记录集的联结，这里注意的是最后出来的记录集会包括表A的全部记录。
**右联结A、B表**的结果和左联结B、A的结果是一样的，也就是说：

```
Select A.name B.name From A Left Join B On A.id=B.id   
```

```
Select A.name B.name From B Right Join A on B.id=A.id  
```

执行后的结果是一样的。

3.全联结：

​	将两个表中存在联结关系的字段的所有记录取出形成记录集的联结。

4.无联结：

​	没有使用联结功能，也有自联结的说法。

内外联结的区别是内联结将去除所有不符合条件的记录，而外联结则保留其中部分。外左联结与外右联结的区别在于如果用A左联 结B则A中所有记录都会保留在结果中，此时B中只有符合联结条件的记录，而右联结相反。

**二．例子**

假设有如下两张表：

| ID   | Name  |
| ---- | ----- |
| 1    | Tiim  |
| 2    | Jimmy |
| 3    | John  |
| 4    | Tom   |

| ID   | Hobby      |
| ---- | ---------- |
| 1    | Football   |
| 2    | Basketball |
| 2    | Tennis     |
| 4    | Soccer     |

1内联结：

```
Select A.Name B.Hobby from A, B where A.id = B.id  
```

这是隐式的内联结，查询的结果是： 

| Name  | Hobby      |
| ----- | ---------- |
| Tim   | Football   |
| Jimmy | Basketball |
| Jimmy | Tennis     |
| Tom   | Soccer     |

它的作用和 

```
Select A.Name from A INNER JOIN B ON A.id = B.id  
```

是一样的。

2.外左联结

```
Select A.Name from A Left JOIN B ON A.id = B.id  
```

这样查询得到的结果将会是保留所有A表中联结字段的记录，若无与其相对应的B表中的字段记录则留空，结果如下：

| Name  | Hobby             |
| ----- | ----------------- |
| Tim   | Football          |
| Jimmy | Basketball，Tennis |
| John  |                   |
| Tom   | Soccer            |

所以从上面结果看出，因为A表中的John记录的ID没有在B表中有对应ID，因此为空，但Name栏仍有John记录。

3.外右联结

```
Select A.Name from A Right JOIN B ON A.id = B.id  
```

结果将会是：

| Name  | Hobby      |
| ----- | ---------- |
| Tim   | Football   |
| Jimmy | Basketball |
| Jimmy | Tennis     |
| Tom   | Soccer     |

**三．联表查询中用到的一些参数**

**1.USING (column_list)：**
其作用是为了方便书写联结的多对应关系，大部分情况下USING语句可以用ON语句来代替，如下面例子：
a LEFT JOIN b USING (c1,c2,c3)，其作用相当于
a LEFT JOIN b ON a.c1=b.c1 AND a.c2=b.c2 AND a.c3=b.c3
**2.STRAIGHT_JOIN：**
由于默认情况下[MySQL](http://lib.csdn.net/base/mysql)在进行表的联结的时候会先读入左表，当使用了这个参数后[mysql](http://lib.csdn.net/base/mysql)将会先读入右表，这是个MySQL的内置优化参数，大家应该在特定情况下使用，譬如已经确认右表中的记录数量少，在筛选后能大大提高查询速度。

原文地址：http://blog.csdn.net/qmhball/article/details/8000003

欢迎加入**[云南php&layui交流群](https://jq.qq.com/?_wv=1027&k=5FT8Mh5)**：511300462