# Python SDK

### 安装OpenMLDB python包
```bash
pip install openmldb
```

### 引入包
```python
import sqlalchemy as db
```

### 使用

#### 创建connection
create_engine('openmldb:///db_name?zk=zkcluster&zkPath=zkpath')
这里db_name不要求必须存在，如果不存在需要在创建好connection后创建database
```
engine = db.create_engine('openmldb:///db_test?zk=127.0.0.1:2181&zkPath=/openmldb')
connection = engine.connect()
```

#### 执行sql
用execute接口可以执行创建database, 创建表，插入数据，创建存储过程, 删除表等操作
```python
try:
    connection.execute("create database db_test;");
    connection.execute("create table tsql1010 ( col1 bigint, col2 date, col3 string, col4 string, col5 int, index(key=col3, ts=col1));")
    connection.execute("insert into tsql1010 values(1000, '2020-12-25', 'guangdon', '广州', 1);")
except Exception as e:
    print(e)
```
执行insert语句还可以用placeholder的方式
```python
try:
    insert = "insert into tsql1010 values(1002, '2020-12-27', ?, ?, 3);"
    connection.execute(insert, ({"col3":"fujian", "col4":"fuzhou"}))
except Exception as e:
    print(e)
```
执行select
```python
try:
    rs = connection.execute("select * from tsql1010;");
    for row in rs:
        print(row)
    rs = connection.execute("select * from tsql1010 where col3 = ?;", ('hefei'))
except Exception as e:
    print(e)
```
如果是请求模式，可以把输入数据放到execute的第二个参数中
```python
try:
   rs = connection.execute("select * from tsql1010;", ({"col1":9999, "col2":'2020-12-27', "col3":'zhejiang', "col4":'hangzhou', "col5":100}));
except Exception as e:
    print(e)
```
#### 执行deployment
```python
raw_connection = engine.raw_connection()
mouse = raw_connection.cursor()
try:
    rs = mouse.callproc("demo", ({"col1":1002, "col2":'2020-12-27', "col3":'fujian', "col4":'fuzhou', "col5":3}))
except Exception as e:
    print(e)
```