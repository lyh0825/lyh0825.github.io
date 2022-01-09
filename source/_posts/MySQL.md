---
title: MySQL
date: 2021-07-18 13:43:55
tags: MySQL
---

## 1.MySQL

## 函数

### round

【round(avg(COL_1), NUM)】:计算col_1的平均值并保留num有效数字

四舍五入

```sql
SELECT ROUND(AVG(SCORE), 1) AS `AVEAGE SCORE`
FROM STUDNET
>>
90.1
80.2
...
```

### length

length('char')

- 中文算三个长度
- alpha和number算一个长度

## 快捷指令

常用快捷键：

**1、执行整篇sql脚本**

`【Ctrl】+【Shift】+【Enter】`

**2、执行当前行：**

`【Ctrl】+【Enter】`

**3、注释/取消注释：**

`【Ctrl】+【/】`

**4、格式化sql语句（美化sql语句）：**

`【Ctrl】+【B】 `

5、（貌似和输入法有冲突）自动补全：【Ctrl】+【Space】 ，还可以使用edit->Auto-complete手动展示补全。如果有冲突可以去下面的文件找到Auto complete进行修改。Modifier的意思就是Ctrl键。

修改快捷键：

在安装根目录查到\data\main_menu.xml这个文件（Ubuntu放在/usr/share/mysql-workbench）。

**6.MySQL Clint清屏**

`system cls`

## Error

Error Code: 1054. Unknown column '%%' in 'where clause'

出现类似的语法错误时，首先考虑是 **‘** 还是 **`** 出错了

一个是syntax error 一个是通配符使用

```mysql
SELECT COL_NAME
FROM TABLE_NAME
WHERE STUDENT_NAME LIKE '小%';

SELECT `COL_NAME`
FROM `DATABASE_NAME`.`TABLE_NAME`
```

`DataBase/Table``     'wildcard' 

## 问题

### Schema和DataBase是否等同？

**schema是数据库的组织和结构，是数据库对象的集合，集合包括表，视图，储存过程，索引等。**

（1）MySQL的文档中指出，在物理上，模式与数据库是同义的，所以，模式和数据库是一回事。

（2）但是，Oracle的文档却指出，某些对象可以存储在数据库中，但不能存储在schema中。 因此，模式和数据库不是一回事。

（3）而根据这篇SQL Server技术文章[SQLServer technical article](https://technet.microsoft.com/en-us/library/dd283095(v=sql.100).aspx)，schema是数据库SQL Server内部的一个独立的实体。 所以，他们也不是一回事。

因此，取决于您使用的RDBMS，模式和数据库可能不一样。

### 引擎

![img](https://upload-images.jianshu.io/upload_images/11464886-82267cb5926d26fb.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

**【InnoDB】事务性数据库**

**【事务】**

子性、一致性、隔离性、持久性。这四个属性通常称为**ACID特性**。

```txt
原子性（atomicity）。一个事务是一个不可分割的工作单位，事务中包括的操作要么都做，要么都不做。

一致性（consistency）。事务必须是使数据库从一个一致性状态变到另一个一致性状态。一致性与原子性是密切相关的。

隔离性（isolation）。一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。

持久性（durability）。持久性也称永久性（permanence），指一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响。
```

具有**提交、回滚、崩溃恢复能力**的事物安全（ACID兼容）能力，并要求实现并发控制同时支持外键

**【MyISAM】**

数据表主要用来**插入**和**查询记录**MyISAM引擎能提供较高的处理效率

**【Memory】**

如果**只是临时存放数据，**数据量不大，并且不需要较高的数据安全性，可以选择将数据保存在内存中的Memory引擎，**MySQL中使用该引擎作为临时表**，存放查询的中间结果

**【Archive】仓库型数据库**

如果只有**INSERT和SELECT操作**，可以选择Archive，Archive支持高并发的插入操作，但是本身不是事务安全的。Archive非常适合存储归档数据，如记录日志信息可以使用Archive
