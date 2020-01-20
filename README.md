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
    1、显示数据库列表。 
    show databases; 
    2、显示库中的数据表： 
    show tables; 
    3、显示数据表的结构： 
    describe 表名; 
    4、select database()
    查看当前所在数据库
    5、执行数据库脚本
    登录到数据库 source + .sql文件的路径
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

    sql事务：把多条语句作为一个整体进行操作要么全成功，要么全失败，被称为数据库事务

访问MySQL数据库只有一种方法，就是通过网络发送SQL命令，然后，MySQL服务器执行后返回结果。
对于Node.js程序，访问MySQL也是通过网络发送SQL命令给MySQL服务器。这个访问MySQL服务器的软件包通常称为MySQL驱动程序

#canvas
    Canvas 是为了解决 Web 页面中只能显示静态图片这个问题而提出的，一个可以使用 JavaScript 等脚本语言向其中绘制图像的 HTML 标签。
    svg（Scalable Vector Graphics，可缩放矢量图形）是基于 XML用于描述二维矢量图形的一种图形格式

    canvas坐标：左上角（坐标为（0,0））所有元素的位置都相对于原点定位
    canvas可以使用CSS来定义大小，但在绘制时图像会伸缩以适应它的框架尺寸：如果CSS的尺寸与初始画布的比例不一致，它会出现扭曲
    尽量使用 HTML 的width 和 height 属性或者直接使用 JS 动态来设置宽高，不要使用 CSS 设置。

    canvas绘图的步骤：
    beginPath()/开始一个路径 绘制路径 closePath()/关闭路径 设置填充颜色或描边颜色 填充颜色或描边

    画一个弧形：ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);startAngle圆弧的起始点， x轴方向开始计算，单位以弧度表示
    弧度公式：degrees*Math.PI/180 举例：如需旋转 5 度，可使用下面的公式：5*Math.PI/180

    如果没有 moveTo，那么第一次 lineTo 的就视为 moveTo每次 lineTo 后如果没有 moveTo，那么下次 lineTo 的开始点为前一次 lineTo 的结束点。
    线性渐变：var gradient = ctx.createLinearGradient(0,0,200,0);使用 createLinearGradient 方法创建一个指定了开始和结束点的CanvasGradient 对象。创建成功后， 你就可以使用 CanvasGradient.addColorStop() 方法，根据指定的偏移和颜色定义一个新的终止
#svg
svg标签上应该有version、baseProfile、xmlns(命名空间)
#koa
中间件：它处在 HTTP Request 和 HTTP Response 中间，用来实现某种中间功能。app.use()用来加载中间件。
下面的logger就是一个中间件。基本上，Koa 所有的功能都是通过中间件实现的，前面例子里面的main也是中间件。每个中间件默认接受两个参数，第一个参数是 Context 对象，第二个参数是next函数。只要调用next函数，就可以把执行权转交给下一个中间件。
```
const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  next();
}

const main = ctx => {
  ctx.response.body = 'Hello World';
};

app.use(logger);
app.use(main);
```
中间件栈：
多个中间件会形成一个栈结构。
最外层的中间件首先执行。
调用next函数，把执行权交给下一个中间件。
...
最内层的中间件最后执行。
执行结束后，把执行权交回上一层的中间件。
...
最外层的中间件收回执行权之后，执行next函数后面的代码。

异步中间件：
如果有异步操作（比如读取数据库），中间件就必须写成 async 函数。

async会把函数返回值包装成promise返回
await 求值关键字、有阻塞线程的作用
为了保证洋葱模型的执行顺序，必须在异步函数和next()前加上await
洋葱模型的存在保证了代码的有序执行，不至于异步混乱

由于Node环境执行的JavaScript代码是服务器端代码，所以，绝大部分需要在服务器运行期反复执行业务逻辑的代码，必须使用异步代码，否则，同步代码在执行时期，服务器将停止响应，因为JavaScript只有一个执行线程。

mz模块可以让nodejs的异步操作返回一个promise，可以方便地适用async await写法

# 面向对象
类是对象的类型模板，实例是根据类创建的对象；类和实例是大多数面向对象编程语言的基本概念
JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程,在 ES2015/ES6 中引入了 class 关键字，但那只是语法糖，JavaScript 仍然是基于原型的

JavaScript 对象有一个指向一个原型对象的链。当试图访问一个对象的属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾。

function Student(prop){
    this.name = prop.name || 'unname'
}
Student.prototype.hello= function(){
    console.log('hello ' + this.name)
}
var xiaoming = new Student({name:'xiaoming'})
xiaoming.__proto__ === Student.prototype //true
所有的函数会有一个特别的属性 —— prototype

# npm
require一个包，就是require package.json的main入口文件
npm whoami 可以查看当前登录用户
npm publish 会发布package.json所在目录的所有文件

npm run新建的这个 Shell，会将当前目录的node_modules/.bin子目录加入PATH变量，执行结束后，再将PATH变量恢复原样。
这意味着，当前目录的node_modules/.bin子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径。比如，当前项目的依赖里面有 Mocha，只要直接写mocha test就可以了。"test": "mocha test"

如果是并行执行（即同时的平行执行），可以使用&符号。
如果是继发执行（即只有前一个任务成功，才执行下一个任务），可以使用&&符号。

# koa
ctx.path 和 ctx.method等价于 ctx.request.path和ctx.request.method
使用ctx.body返回

不要在低级模块里引用高级模块
# git
git pull命令相当于git fetch远程仓库，取下来之后再进行merge操作
1.你和别人同时开发，当别人做了几次commit并且推送到了远程仓库
2.你做了几次commit，当你准备推到远程时，发现git提醒你先要git pull
3.这时的pull会要求你填写合并信息，因为git发现本地少了几个远端的commit，远端也没有本地的commit

git push是把当前本地branch的所有commit提交到远程仓库
如果branch是自己创建的，需要指定仓库名和分支名

merge是从两个commit的分叉位置起，把目标commit内容应用到当前commit，并且生成一个新的commit
git merge --abort取消合并

# 数据库连结
使用内联结的情况一：
需要将商品表中的厂商id扩展成详细厂商信息，商品表中有厂商id，厂商信息存储在厂商表中；
select vendor_name, product_name, product_price from vendors, products where vendors.vendor_id = products.vendor_id;
在联结两个表时，实际上是将第一个表中的每一行与第二个表中的每一行进行配对，where子句是执行配对时的过滤条件。
也被称为内联结；
select vendor_name, product_name, product_price from vendors inner join products on vendors.vendor_id = products.vendor_id;