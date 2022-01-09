---
title: django
date: 2021-07-22 21:56:14
tags: django
---

# DjangoBlog

## 0.前言

### python连接数据库

1. 在mysql中创建一个数据库mydb
2. 在python中import pymysql
3. 连接数据库

```python
import pymysql

# 这些信息可以去mysql的GUI查询
db = pymysql.connect(host='localhost',
                     user='root',
                     password='*****',
                     port=3306,
                     db='mydb')

# python控制mysql的逻辑是模拟cursor
cursor = db.cursor()

# define sql语句
sql = 'SELECT STUDENT_NAME, SCORE\
	   FORM STUDENT\
	   WHERE SCORE >= 60\
	   ORDER BY SCORE DESC'

# 执行sql语句
cursor.execute(sql)

# 将cursor.fetall()return的tuple 赋值给result
result = cursor.fetall()

# 展示data
for item in result:
    print(item, end='\n')

# 每次运行后为了保障安全建议关闭db
db.close()

>>
('小红', 95)
('spark', 88)
('小白', 82)
('DRINK', 80)
('NEW', 80)
('OLD', 78)
('SPARK', 72)
('TANT', 70)
('小红', 60)
```

Python查询Mysql使用fetchone（）方法获取单条数据，使用fetchall（）方法获取多条数据。

- **fetchone（）：该方法获取下一个查询结果集。结果集是一个对象[return single tuple]**
- **fetchall（）：接收全部的返回结果行。[return multiple tuples]**
- **rowcount：这是一个只读属性，并返回执行execute（）方法后影响的行数。**

### 创建虚拟环境

在terminal中输入

```python
# 在当前目录中创建虚拟环境
python -m venv myvenv[虚拟环境的名字]

# 激活虚拟环境
cd myvenv\Scripts>activate

# 退出虚拟环境
deactivate
```

**虚拟环境的优点**

1. 使不同应用开发环境独立
2. 环境升级不影响其他应用，也不会影响全局的python环境
3. 它可以防止系统中出现包管理混乱和版本的冲突

`pip list`查看已经安装的module

### CD命令

```shell
# 切换路径
xxx:cd /d D:+绝对路径

# 返回上一层
xxx:cd..
```

### requirements.txt项目依赖

可以是项目在另一个环境上重新构建项目所需要运行的环境依赖包

**【将依赖包目录打包到项目里】**

```txt
使用之前先在terminal里使用cd切换好路径
xxx： cd /d +绝对路径
```

**第一种【适合但虚拟环境】**

```python
# terminal windows使用
pip freeze > requirements.txt
```

这种方式，会将环境中的依赖包全都加入，如果使用的全局环境，则下载的所有包都会在里面，不管是不时当前项目依赖的

**第二种【优先选择】**

```python
# 安装
pip install pipreqs

# terminal windows使用
pipreqs . --encoding=utf8 --force
```

**【释放依赖包项目】**

```python
# terminal windows使用
pip install -r requirements.txt
```

## 1.配置虚拟环境

虚拟环境（virtualenv、venv）

