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

    分页：使用LIMIT <M> OFFSET <N>可以对结果集进行分页，每次查询返回结果集的一部分；
    LIMIT总是设定为pageSize；
    OFFSET计算公式为pageSize * (pageIndex - 1)。
    LIMIT表示最多查询多少条，OFFSET表示从几号记录开始(0为第一项纪录)
    



