
mysql 创建 数据库时指定编码很重要，很多开发者都使用了默认编码。但指定数据库的编码可以很大程度上避免导入、导出带来的乱码问题。

可以用下面的语句在创建数据库时指定编码

```
GBK: create database test2 DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;

UTF8: CREATE DATABASE `test2` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```


MySQL中，有时需要查看一下当前数据库的编码，甚至需要修改一下数据库编码。
查看当前数据库编码的SQL语句为：

```
mysql> use dbname;
Database changed
mysql> show variables like 'character_set_database';
+------------------------+--------+
| Variable_name          | Value  |
+------------------------+--------+
| character_set_database | latin1 |
+------------------------+--------+
1 row in set (0.00 sec)
```

上面，我们先切换到 `dbname` 数据库下面来，然后使用SQL语句：`show variables like 'character_set_database'` ; 来查看了 `dbname` 数据库的编码。查询得到的结果是 `latin1` 编码。

下面，我们来修改 `dbname` 数据库的编码，把它修改为 `utf8`。

```
mysql> alter database dbname CHARACTER SET utf8;
Query OK, 1 row affected (0.00 sec)

mysql> show variables like 'character_set_database';
+------------------------+--------+
| Variable_name          | Value  |
+------------------------+--------+
| character_set_database | utf8 |
+------------------------+--------+
1 row in set (0.00 sec)

```

这里同样做了两件事情：
1、使用SQL语句：`alter database dbname CHARACTER SET utf8;` 把 `dbname` 数据库的编码设置为了 `utf8`.
2、再次使用 `show variables like 'character_set_database';`  来确认一下当前 `dbname` 是什么编码。经过确认，数据库编码已经修改为 `utf8` 了。



上面内容是数据库相关的编码 。 




MySQL中，可以用 `show create table ` 这一SQL语句来解决这个问题。

show create table可以查看创建这个表的SQL语句脚本，它的基本语法是：
show create table <表名>;

我们用它看看test表的create脚本：

```
mysql>  show create table test;
+-------+---------------------------------------------
------------------------------------------------------
-------------------------------------------+
| Table | Create Table
+-------+---------------------------------------------
------------------------------------------------------
-------------------------------------------+
| test  | CREATE TABLE `test` (
  `t_id` int(11) DEFAULT NULL,
  `t_name` varchar(50) NOT NULL,
  `t_password` char(32) DEFAULT NULL,
  `t_birth` date DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
+-------+---------------------------------------------
------------------------------------------------------
-------------------------------------------+
1 row in set (0.00 sec)
```

从这个结果我们可以看到，有这样一句：DEFAULT CHARSET=latin1，它表示test表的字符编码类型为：latin1。

另外，我们还可以从这个结果中看出来，当前表的引擎为InnoDB引擎。这个信息同样也非常重要。

修改表的编码方式：<pre><code>ALTER TABLE `test` DEFAULT CHARACTER SET utf8;</code> </pre>该命令用于将表test的编码方式改为utf8； 

修改字段的编码方式：<pre>ALTER TABLE `test` CHANGE `name` `name` VARCHAR(36) CHARACTER SET utf8 NOT NULL;</pre> 该命令用于将表test中name字段的编码方式改为utf8

