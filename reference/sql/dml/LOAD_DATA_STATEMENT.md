# LOAD DATA INFILE 语句

## Syntax

```sql
LoadDataInfileStmt
								::= 'LOAD' 'DATA' 'INFILE' filePath LoadDataInfileOptionsList
filePath ::= string_literal
LoadDataInfileOptionsList
									::= 'OPTIONS' '(' LoadDataInfileOptionItem (',' LoadDataInfileOptionItem)* ')'

LoadDataInfileOptionItem
									::= 'DELIMITER' '=' string_literal
											|'HEADER' '=' bool_literal
											|'NULL_VALUE' '=' string_literal
											|'FORMAT' '=' string_literal						
```

`LOAD DATA INFILE`语句以非常高的速度将文件中的行读取到 table 中。`LOAD DATA INFILE` 与 `SELECT ... INTO OUTFILE`互补。要将数据从 table 写入文件，请使用[SELECT...INTO OUTFILE](../dql/SELECT_INTO_STATEMENT.md))。要将文件读回到 table 中，请使用`LOAD DATA INFILE`。两条语句的大部分配置项相同，具体包括：

| 配置项     | 类型    | 默认值 | 描述                                                         |
| ---------- | ------- | ------ | ------------------------------------------------------------ |
| delimiter  | String  | ,      | 列分隔符，默认为`,`                                          |
| header     | Boolean | true   | 是否包含表头, 默认为`true`                                   |
| null_value | String  | null   | NULL值，默认填充`"null"`。加载时，遇到null_value的字符串将被转换为NULL，插入表中。 |
| format     | String  | csv    | 加载文件的格式，默认为`csv`。请补充一下其他的可选格式。      |
| quote      | String  | ""     | 输入数据的包围字符串。字符串长度<=1。默认为""，表示解析数据，不特别处理包围字符串。配置包围字符后，被包围字符包围的内容将作为一个整体解析。例如，当配置包围字符串为"#"时， `1, 1.0, #This is a string field, even there is a comma#`将为解析为三个filed.第一个是整数1，第二个是浮点1.0,第三个是一个字符串。 |

## SQL语句模版

```sql
LOAD DATA INFILE 'file_name' OPTIONS (key = value, ...)
```

## Examples:

从`data.csv`文件读取数据到表`t1`中。并使用`,`作为列分隔符

```sql
LOAD DATA INFILE 'data.csv' INTO TABLE t1 ( delimit = ',' );
```

从`data.csv`文件读取数据到表`t1`中。并使用`,`作为列分隔符, 字符串"NA"将被替换为NULL。

```sql
LOAD DATA INFILE 'data.csv' INTO TABLE t1 ( delimit = ',', nullptr_value='NA');
```

