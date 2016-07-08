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
