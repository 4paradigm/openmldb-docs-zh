# REST APIs

### Put

reqeust url: http://ip:port/dbs/{db_name}/tables/{table_name}

http method: PUT 

request body: 
```
{
    "value": [
    	[v1, v2, v3]
    ]
}
```

+ 目前仅支持一条插入，不可以插入多条数据。
+ 数据需严格按照schema排列。

#### example

```
curl http://127.0.0.1:8080/dbs/db/tables/trans -X PUT -d '{
"value": [
    ["bb",24,34,1.5,2.5,1590738994000,"2020-05-05"]
]}'
```
response:

```
{
    "code":0,
    "msg":"ok"
}
```

### 执行deployment

reqeust url: http://ip:port/dbs/{db_name}/deployments/{deployment_name}

http method: POST

request body: 

```
{
    "input": [["row0_value0", "row0_value1", "row0_value2"], ["row1_value0", "row1_value1", "row1_value2"]],
    "need_schema": false
}
```

+ 可以支持多行，输入与response data.data字段数组一一对应。
+ need_schema可以设置为true, 返回就会有输出结果的schema。默认为false。

#### example

```
curl http://127.0.0.1:8080/dbs/demo_db/deployments/demo_data_service -X POST -d'{
        "input": [["aaa", 11, 22, 1.2, 1.3, 1635247427000, "2021-05-20"]],
    }'
```

response:

```
{
    "code":0,
    "msg":"ok",
    "data":{
        "data":[["aaa",11,22]]
    }
}
```
