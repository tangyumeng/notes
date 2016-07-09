### MySQL 数字类型(numberic type)

MySQL 数字类型可以存放 integer, floating-point, 和 fixed-point 类型的值。
数字类型的列可以是有符号或者无符号。

#### 数字型数据类型

| 类型名        | 含义           |取值范围 |
| ------------- |:-------------:|:---------:|
| TINYINT      | 非常小的整数 (A very small integer)|一个字节<br>带符号的范围是-128到127 <br>无符号的范围是0到255  |
| SMALLINT     | 小整数(A small integer)      |2个字节<br>有符号的范围是-32768(从 -2^15 (-32,768)到32767( 2^15 – 1 (32,767))，<br>无符号的范围是0到65535  的整型数据。存储大小为 2 个字节。 |
| MEDIUMINT |  A medium-sized integer     | 3个字节 <br>有符号的范围是-8388608到8388607，<br>无符号的范围是0到16777215。|  
|INT (INTEGER 同义词)| A standard integer | 4个字节 <br>有符号的范围是-2147483648到2147483647，<br>无符号的范围是0到4294967295|
| BIGINT | A large integer| 8个字节 <br>-9223372036854775808 - 	9223372036854775807<br>0-18446744073709551615|
| FLOAT | A single-precision floating-point number | [float 使用注意点](http://www.cnblogs.com/zhoujinyi/archive/2013/04/26/3043160.html)|
| DOUBLE |A double-precision floating-point number  | |  
| DECIMAL | A fixed-point number |  |
| BIT |  A bit field |  |  |


### MySQL 帮助文档

终端登录 MySQL  后执行 `help 命令`。如: `mysql> help show`

### MySQL 查询

#### 多表查询：

- 连接：
  - 交叉连接: 笛卡尔乘积
  - 自然连接
  - 外连接:
    - 左外连接
    - 右外连接
  - 自连接  

#### 子查询
- 比较操作中使用自查询:子查询只能返回单个值
- IN(): 使用子查询
- 在FROM中使用子查询

#### 联合查询
- UNION


### 连接管理器
  - 接受请求
  - 创建线程
  - 认证用户
  - 建立安全连接

### 并发控制

### 锁
  - 读锁:共享锁
  - 写锁:独占锁
      LOCK TABLES tb_name {READ|WRITE}<br>
      UNLOCK TABLES<br>
  锁粒度:从大到小，MySQL 服务器仅支持表级锁、行锁需要存储引擎支持<br>
  - 表锁<br>
  - 页锁<br>
  - 行锁<br>


### 事务日志

- 重做日志
  - redo log
- 撤销日志
  - undo log

### 事务
- 原子
- 一致
- 隔离
- 持久

#### 隔离级别
- READ UNCOMMITED读未提交
- READ COMMITED读提交
- REPEATABLE READ可重读（默认级别）
- SERIALIZABLE可穿行

#### 查看数据库默认隔离级别
mysql>`SHOW GLOBAL VARIABLES LIKE '%iso%';`
或者
mysql>`select @@tx_isolation;`

修改默认隔离级别
mysql>`SET tx_isolation='READ-UNCOMMITTED';`
