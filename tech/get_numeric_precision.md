postgresql数据库numeric类型精度如何获取
=======================================
建表的时候字段类型设置为 numeric(9,2)，查询表结构的时候从 pg_attribute读出的atttypmod值为589830， 请问postgresql是怎么从这个589830计算出精度为 9,2的？
解决方案 »
建议你建一个表create table test (
f1 numeric(5,2),
f2 numeric(5,3),
f3 numeric(5,4),
f4 numeric(5,2),
f5 numeric(6,2),
f6 numeric(7,2),
f7 numeric(8,2),
f8 numeric(9,2)
);然后看一下，这个表中这些字段的atttypmod 是多少？贴出来大家或许能找到规律。
nueric(5,2) => 327686
nueric(5,3) => 327687
nueric(5,4) => 327688
nueric(5,5) => 327689
nueric(2,2) => 393222
nueric(7,2) => 458758
nueric(8,2) => 524294
nueric(9,2) => 589830
再加一点信息 在数据库中 numeric对应的typeid是 1700 
把327686 数字转换为二进制，则比较清晰了。
第一个字节为 numeric (n,m) 的N， 最后一个字节为 m+4[code=BatchFile]nueric(5,2) => 327686   0101 0000 0000 0000 0110
nueric(5,3) => 327687   0101 0000 0000 0000 0111
nueric(5,4) => 327688   0101 0000 0000 0000 1000
nueric(5,5) => 327689   0101 0000 0000 0000 1001
nueric(2,2) => 393222   0110 0000 0000 0000 0110
nueric(7,2) => 458758   0111 0000 0000 0000 0110
nueric(8,2) => 524294   1000 0000 0000 0000 0110
nueric(9,2) => 589830   1001 0000 0000 0000 0110[/code]
当您的问题得到解答后请及时结贴.
http://topic.csdn.net/u/20090501/15/7548d251-aec2-4975-a9bf-ca09a5551ba5.html
我在源代码中看到的 * NUMERIC - the decimal_digits is stored in atttypmod as follows:
 *
 * column_size =((atttypmod - VARHDRSZ) >> 16) & 0xffff
 * decimal_digits  = (atttypmod - VARHDRSZ) & 0xffff
========================================
2.  任意精度数值
numeric类型最多能存储有1000个数字位的数字并且能进行准确的数值计算。它主要用于需要准确地表示数字的场合，如货币金额。不过，对numeric 类型进行算术运算比整数类型和浮点类型要慢很多。
numeric类型有两个术语，分别是标度和精度。numeric类型的标度（scale）是到小数点右边所有小数位的个数， numeric 的精度（precision）是所有数字位的个数，例如，23.5141 的精度是6而标度为4。可以认为整数的标度是零。
numeric 类型的最大精度和最大标度都是可以配置的。可以用下面的语法定义一个numeric类型：
a)      NUMERIC(precision, scale)
b)      NUMERIC(precision)
c)      NUMERIC
精度必须为正数，标度可以为零或正数。
在上面的第二种语法中没有指定标度，则系统会将标度设为0，所以NUMERIC(precision,0)和NUMERIC(precision)是等价的。
第三种类型的语法没有指定精度和标度，则这种类型的列可以接受任意精度和标度的numeric数据（在系统能表示的最大精度范围内），而不会对输入的数据进行精度或标度的变换。如果一个列被定义成numeric类型而且指定了标度，那么输入的数据将被强制转换成这个标度（如果它的标度比定义列的numeric的标度大），进行标度转换时的规则是四舍五入。如果输入的数据进行标度转换后得到的数据在小数点左边的数据位的个数超过了列的类型的精度减去标度的差，系统将会报错
--构建环境
CREATE TABLE test (numeric1 numeric(3,3));
--插入超过精度和标度的值
INSERT into test VALUES(1.3456);
--报错信息
ERROR:  numeric field overflow
DETAIL:  A field with precision 3, scale 3 must round to an absolute value less than 1.
--插入超过标度的值，超过标度的部分被四舍五入成小于1的数。
INSERT into test VALUES(0.3455);
INSERT into test VALUES(0.3454);
SELECT * from test;
 numeric1
----------
    0.346
    0.345
numeric 类型可以接受一个特殊的值 “NaN” （Not a Number），它的意思是“不是一个数字"。任何在 NaN 上面的操作都生成另外一个 NaN。 如果在 SQL 命令里把这些值当作一个常量写，必须把它用单引号引起来，比如 UPDATE table SET x = 'NaN'。在输入时，”NaN”的大小写无关紧要。例：
INSERT into test values('NaN');
SELECT * from test;
numeric1
----------
    0.346
    0.345
      NaN
注意：在其它的数据库中，NaN和任何的数值数据都不相等，两个NaN也是不相等的，在postgresSQL中，为了允许numeric值可以被排序和使用基于树的索引，PostgreSQL把NaN值视为相等，并且比所有非NaN值都要大。例：
SELECT * from test where numeric1='NaN';
 numeric1
----------
      NaN
SELECT * from test t1,test t2 where t1.numeric1=t2.numeric1;
 numeric1 | numeric1
----------+----------
    0.345 |    0.345
    0.346 |    0.346
      NaN |      NaN
SELECT * from test where NUMERIC1>10;
 numeric1
----------
      NaN
3.  浮点类型
数据类型 real 和 double precision 表示不准确的变精度的数字。不准确意味着一些值不能准确地转换成内部格式并且是以近似的形式存储的，存储在数据库里的只是它们的近似值。如果要求准确地保存某些数值（比如计算货币金额），应使用 numeric 类型。另外，比较两个浮点数是否相等时，可能会得到意想不到的结果。例：
create table test(real1 real)
insert into test VALUES(1.6666666);
insert into test VALUES(1.666666);
insert into test VALUES(1.66666);
SELECT * FROM test;
  real1 
---------
 1.66667
 1.66667
 1.66666
(3 rows)
mydb=# SELECT * from test where real1=1.66666;            --等值运算未能获取到想要的数据
 real1
-------
(0 rows)
mydb=# SELECT * from test where real1=1.66667;           --等值运算未能获取到想要的数据
 real1
-------
(0 rows)
mydb=# SELECT * from test where real1>1.66666;
  real1 
---------
 1.66667
 1.66667
(2 rows)
mydb=# SELECT * from test where real1>1.6666;
  real1 
---------
 1.66667
 1.66667
 1.66666
(3 rows)
浮点类型还有几个特殊值：Infinity、-Infinity、NaN。这些值分别表示 IEEE 754标准中的特殊值"正无穷大"，"负无穷大"， 以及"不是一个数字"。如果在 SQL 命令里把这些数值当作常量写，必须在它们用单引号引起来，例如UPDATE table SET x = 'Infinity'。 输入时，这些值的大小写无关紧要。
PostgreSQL 还支持 SQL 标准中定义的类型float和float(p)。p 定义以二进制位表示的最低可以接受的精度，p的取值在1到53之间。实际上，如果p的取值在1到24之间，float(p)被看成是real类型，如果p的取值在25到53之间，float(p)被看成是double precision类型。不带精度的float被看成是double precision类型。