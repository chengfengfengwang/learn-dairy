# 拖拽
    设置在操作元素上的：
        dragstart
        dragend
    设置在放置元素上的：
        dragenter
        dragleave
        dragover //需要preventDefault才能触发dragdrop
        dragdrop
    外部文件拖到浏览器会打开该资源，需要在document ondragover上preventDefault

# mysql
    主键：对于关系表，有个很重要的约束，就是任意两条记录不能重复，指能够通过某个字段唯一区分出不同的记录，这个字段被称为主键。选取主键的一个基本原则是：不使用任何业务相关的字段作为主键。我们一般把这个字段命名为id。常见的可作为id字段的类型有：自增整数类型、全局唯一GUID类型。主键也不应该允许NULL。

    外键：通过定义外键约束，关系数据库可以保证无法插入无效的数据。即如果classes表不存在id=99的记录，students表就无法插入class_id=99的记录

    基本查询：SELECT * FROM <表名>可以查询一个表的所有行和所有列的数据。SELECT是关键字，表示将要执行一个查询，*表示“所有列”，FROM表示将要从哪个表查询，本例中是students表。许多检测工具会执行一条SELECT 1;来测试数据库连接。

    条件查询：SELECT * FROM <表名> WHERE <条件表达式>

    SELECT * FROM students WHERE score >= 80 。WHERE关键字后面的score >= 80就是条件。score是列名，该列存储了学生的成绩，因此，score >= 80就筛选出了指定条件的记录

    投影查询：SELECT *表示查询表的所有列，使用SELECT 列1, 列2, 列3则可以仅返回指定列。
    使用SELECT 列1, 列2, 列3 FROM ...时，还可以给每一列起个别名，这样，结果集的列名就可以与原表的列名不同。它的语法是SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM ...。

    排序：SELECT id, name, gender, score FROM students ORDER BY score DESC;DESC表示“倒序”。默认的排序规则是ASC：“升序”，即从小到大。ASC可以省略。如果有WHERE子句，那么ORDER BY子句要放到WHERE子句后面。
    使用ORDER BY score DESC, gender表示先按score列倒序，如果有相同分数的，再按gender列排序

    分页：使用LIMIT <M> OFFSET <N>可以对结果集进行分页，每次查询返回结果集的一部分；
    LIMIT表示最多查询多少条，OFFSET表示从几号记录开始(0为第一项纪录)
    LIMIT总是设定为pageSize；
    OFFSET计算公式为pageSize * (pageIndex - 1)。

    聚合查询：
    SELECT COUNT(*) FROM students;//COUNT(*)表示查询所有列的行数，查询的结果是一个二维表，列名是COUNT(*)
    SELECT COUNT(*) num FROM students;//给列名设置一个别名，便于处理结果
    SELECT AVG(score) average FROM students WHERE gender = 'M';//统计男生的平均成绩
    COUNT(*)表示查询所有列的行数
    其他函数：
    SUM	计算某一列的合计值，该列必须为数值类型
    AVG	计算某一列的平均值，该列必须为数值类型
    MAX	计算某一列的最大值
    MIN	计算某一列的最小值
    MAX()和MIN()函数并不限于数值类型。如果是字符类型，MAX()和MIN()会返回排序最后和排序最前的字符
    如果聚合查询的WHERE条件没有匹配到任何行，COUNT()会返回0，而SUM()、AVG()、MAX()和MIN()会返回NULL
    分组：
    SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;//GROUP BY子句指定了按class_id分组，因此，执行该SELECT语句时，会把class_id相同的列先分组，再分别计算
    SELECT class_id, gender, COUNT(*) num FROM students GROUP BY class_id, gender;//统计各班的男生和女生人数

    多表查询：SELECT * FROM <表1> <表2>
    SELECT * FROM students, classes;//返回结果集的列数是students表和classes表的列数之和，行数是students表和classes表的行数之积。
    //设置别名
    SELECT
        s.id sid,
        s.name,
        s.gender,
        s.score,
        c.id cid,
        c.name cname
    FROM students s, classes c;
    //多表查询设置条件 ，每行记录都满足条件s.gender = 'M'和c.id = 1
    SELECT
        s.id sid,
        s.name,
        s.gender,
        s.score,
        c.id cid,
        c.name cname
    FROM students s, classes c
    WHERE s.gender = 'M' AND c.id = 1;

    连接查询：先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上
    内连接（INNER JOIN）//INNER JOIN只返回同时存在于两张表的行数据
    外连接（OUTER JOIN）
    RIGHT OUTER JOIN返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以NULL填充剩下的字段。
    LEFT OUTER JOIN则返回左表都存在的行

    场景：两个表 students 和 classes,students表只存了class_id，需要根据students表的class_id，找到classes表对应的班级名称
    SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
    FROM students s
    INNER JOIN classes c
    ON s.class_id = c.id;
    //
    先确定主表，仍然使用FROM <表1>的语法；
    再确定需要连接的表，使用INNER JOIN <表2>的语法；
    然后确定连接条件，使用ON <条件...>，这里的条件是s.class_id = c.id，表示students表的class_id列与classes表的id列相同的行需要连接；
    可选：加上WHERE子句、ORDER BY等子句。

    插入：
    INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);
    INSERT INTO students (class_id, name, gender, score) VALUES (2, '大牛嗷嗷', 'M', 80);//插入一条数据
    INSERT INTO students (class_id, name, gender, score) VALUES (1, '大宝', 'M', 87),(2, '二宝', 'M', 81); //一次插入多条数据

    更新：
    UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
    UPDATE students SET name='大牛', score=67 WHERE id=1;//更新students表id=1的记录的name和score这两个字段
    UPDATE students SET score=score+10 WHERE score<80;//使用表达式，更新多条记录
    UPDATE语句可以没有WHERE条件，这时整个表的所有记录都会被更新

    删除：
    DELETE FROM <表名> WHERE ...;
    DELETE FROM students WHERE id=1;//删除students表中id=1的记录
    DELETE FROM students WHERE id>=5 AND id<=7;//一次删除多条记录
    如果WHERE条件没有匹配到任何记录，DELETE语句不会报错，也不会有任何记录被删除。
    和UPDATE类似，不带WHERE条件的DELETE语句会删除整个表的数据

    MySQL
    安装完MySQL后，除了MySQL Server，即真正的MySQL服务器外，还附赠一个MySQL Client程序。
    命令行程序mysql实际上是MySQL客户端。在MySQL Client中输入的SQL语句通过TCP连接发送到MySQL Server
    mysql -h 10.0.1.99 -u root -p //连接远程mysql server -h指定IP或域名
    使用EXIT命令退出MySQL,断开了客户端和服务器的连接，MySQL服务器仍然继续运行

    本地连接mysql: mysql -u root -p
    列出所有数据库：SHOW DATABASES; //information_schema、mysql、performance_schema和sys是系统库，不要去改动它们
    创建一个新数据库 CREATE DATABASE test;
    删除一个数据库   DROP DATABASE test;
    对一个数据库进行操作时，要首先将其切换为当前数据库：USE test;

    列出当前数据库的所有表：SHOW TABLES;
    创建表: CREATE TABLE <table name>;
    删除表: DROP TABLE <table name>;
    查看表结构: DESC <table name>; //describe table 


    








    
    



