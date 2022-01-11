# Python SDK

## 1. 安装OpenMLDB python包

使用pip安装openmldb

```bash
pip install openmldb
```

引入包

```python
import sqlalchemy as db
```

## 2. Python SDK快速上手

### 2.1 创建connection

`create_engine('openmldb:///db_name?zk=zkcluster&zkPath=zkpath')`
这里db_name不要求必须存在，如果不存在需要在创建好connection后创建database

```java
// create engine with configuring db_name, zk and zkPath
engine = db.create_engine('openmldb:///db_test?zk=127.0.0.1:2181&zkPath=/openmldb')
connection = engine.connect()
```

### 2.2 创建数据库

使用`connection.execute()`接口创建数据库：

```python
try:
    connection.execute("create database db_test;");
except Exception as e:
    print(e)
```

### 2.3 创建表

使用`connection.execute()`接口创建一张表：

```python
try:
    connection.execute("create table tsql1010 ( col1 bigint, col2 date, col3 string, col4 string, col5 int, index(key=col3, ts=col1));")
except Exception as e:
    print(e)
```



### 2.4 插入数据到表中

使用`connection.execute()`接口执行SQL的插入语句，可以向表中插入数据：

```python
try:
    connection.execute("insert into tsql1010 values(1000, '2020-12-25', 'guangdon', '广州', 1);")
except Exception as e:
    print(e)
```

使用placeholder的方式执行SQL插入语句：

```python
try:
    insert = "insert into tsql1010 values(1002, '2020-12-27', ?, ?, 3);"
    connection.execute(insert, ({"col3":"fujian", "col4":"fuzhou"}))
except Exception as e:
    print(e)
```



### 2.5 执行SQL批式查询

使用`connection.execute()`接口执行SQL批式查询语句:

```python
try:
    rs = connection.execute("select * from tsql1010;");
    for row in rs:
        print(row)
    rs = connection.execute("select * from tsql1010 where col3 = ?;", ('hefei'))
except Exception as e:
    print(e)
```

### 2.6 执行SQL请求式查询

请求式查询，可以把输入数据放到execute的第二个参数中

```python
try:
   rs = connection.execute("select * from tsql1010;", ({"col1":9999, "col2":'2020-12-27', "col3":'zhejiang', "col4":'hangzhou', "col5":100}));
except Exception as e:
    print(e)
```
### 2.7 删除表

使用`connection.execute()`接口删除一张表：

```python
try:
    connection.execute("drop table tsql1010;")
except Exception as e:
    print(e)
```

### 2.8 删除数据库

使用`connection.execute()`接口删除数据库：

```python
try:
    connection.execute("drop database demo_db;")
except Exception as e:
    print(e)
```

### 