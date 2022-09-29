#数据库的CRUD语句

##INSERT语句
###在指定列中插入数据
**INSERT INTO** 表名 (column1,column2,column3,...)
**VALUES** (value1,value2,value3,...);
~~~SQL
INSERT INTO website (url,country)
VALUES ('https://www.baid.com',CN)
~~~
###在所有的列中插入数据
列相关信息可以省略，但是要求按顺序给所有字段指定要添加的值
**INSERT INTO** 表名
**VALUES** (value1,value2,value3,...);
~~~SQL
INSERT INTO website
VALUES (1,'百度','https://www.baid.com',CN)
~~~

##UPDATE语句
**UPDATE** 表名
**SET** column1=value1,column2=value2,...
**WHERE** some_column=some_value;
修改 **WHERE** 确定的某些记录的列数据，如果没有 **WHERE**，将修改所有记录
~~~SQL
//修改所有记录
UPDATE employee
SET salary = 15000
//使用WHERE过滤
UPDATE employee
SET salary = salary*1.2,birthday = '2022-7-13'
WHERE name = '枫原万叶'
~~~
##DELETE语句
**DELETE FROM** 表名
**WHERE** some_column=some_value;
删除 **WHERE** 确定的某些记录，如果没有 **WHERE**，将删除所有记录
~~~SQL
DELETE FROM employee
WHERE salary = 15000
~~~
**注意**： **DELETE** 语句是删除一条记录而不能删除某一列的值，可以通过**UPDATE** 把值设置为 **NULL** 或 **' '**。
##SELECT语句
**SELECT [DISTINCT] <span>*</span> | {column1,column2,..,} FROM 表名**
* **DISTINCT**可选，表示是否去掉重复数据。
* <span>*</span> 表示查询所有列，如果没有 * ，需要指定要查询的列名
###使用表达式查询
可以使用表达式对查询的列数据进行运算。
~~~SQL
//统计学生总分
SELECT `name`, (chinese+english+math) FROM student;
~~~
###使用as指定别名
可以对查询的表达式或者其它字段指定一个别名，执行查询语句后的输出结构将用别名代替字段名。
~~~SQL
SELECT `name` AS '名字', (chinese + english + math + 10) AS total_score
    FROM student
~~~
###使用where字句过滤查找
在where字句中使用的比较运算符
|运算符|功能|
|--|--|
|>,<,=,>=,<>|大于，小于，等于，大于等于，不等于|
|BETWEEN ... AND ...|查找在某一区间的值|
|IN(SET)|查找在IN列表中的值|
|LIKE|模糊查询，%表示0到多个字符，_表示一个任意字符|
|IS NULL|判断是否为空|
还有一些逻辑运算符，**and、or、not**
~~~SQL
-- 比较查找
SELECT * FROM student WHERE english > 90
-- 区间查找，注意是闭区间
SELECT * FROM student WHERE english BETWEEN 80 AND 90
-- IN列表查找
SELECT * FROM student WHERE math IN (89, 90, 91);
-- 模糊查询，查找姓张的人
SELECT * FROM student WHERE `name` LIKE '张%'
-- 日期用于比较
SELECT * FROM emp WHERE hiredate>'1992-01-01'
~~~
###使用order by对查询结构排序
**ORDER BY column asc|desc**
* **column** 指定待排序的列名，列明也可以是之前as定义的别名
* **asc是升序排列，也是默认情况，desc是降序**
* 可以使用多次排序，即对前一个字段相同的数据再根据后一个字段进行排序
~~~SQL
--降序排列
SELECT `name`, (chinese + english + math) AS total_score
    FROM student
        WHERE `name` LIKE '韩%' ORDER BY total_score desc;
-- 两次排序
SELECT * FROM emp 
    ORDER BY deptno ASC,sal DESC;
~~~
###统计函数
####COUNT
返回符号条件的行数，即多少条符合条件的记录
**SELECT COUNT(<span>*</span>) | COUNT(列名) FROM 表名**
如果指定了列名，**COUNT** 不会将空记录计入到总数中。
~~~SQL
//统计表中的所有记录的条数
SELECT COUNT(*) FROM student;
//WHERE语句过滤
SELECT COUNT(*) FROM student WHERE math > 80;
~~~
####SUM
求和函数，对符合的列数据相加求和，只能用于数值列
**SELECT { SUM(列名), SUM(列名) ... } FROM 表名**
~~~SQL
SELECT SUM(math),SUM(english),SUM(chinese) FROM student;
~~~
####AVG
对符合条件的列数据求平均值
**SELECT {AVG(列名), AVG(列名) ...} FROM 表名**
~~~SQL
//各科成绩求平均值
SELECT AVG(math + english + chinese) FROM student;
~~~
####MAN/MIN
**SELECT MAX(列名)/MIN(列名) FROM 表名**
~~~SQL
SELECT MAX(math), MIN(math) FROM student
~~~
###分组查询
####使用GROUP BY分组
GROUP可以对数据按列进行分组，即列相同的放在同一组
~~~SQL
-- 把员工按照部门进行分组，然后统计每个部门的平均薪资和最高工资
SELECT AVG(sal), MAX(sal) , deptno
FROM emp GROUP BY deptno
~~~
**说明：GROUP BY分组后直接查询获得的是第一个该组成员，比如这里部门号为10，如果有多个部门号为10的数据，只会输出按照顺序排第一的数据**
####使用HAVING过滤组
**HAVING** 对之前用**GROUP BY**分组得到的结果进行过滤
~~~SQL
//对之前的分组过过滤掉平均薪资只有两千的部门
SELECT AVG(sal), deptno
    FROM emp GROUP BY deptno
        HAVING AVG(sal) < 2000
~~~
###字符串函数
使用字符串函数可以对查询得到的字符串数据进行一些处理后输出
|函数|功能|
|--|--|
|CONCAT(string2,..)|连接列数据和指定的字符串|
|UCASE(string2)|字符串转大写|
|LCASE(string2)|字符串转小写|
|LENGHT(string)|string长度(字节)|
|REPLACE (str ,search_str ,replace_str )|在 str 中用 replace_str 替换 search_str|
###数学函数
|函数|功能|
|--|--|
|ABS (num)|求绝对值|
|CEILING (number2)|向上取整|
|FLOOR (number2)|向下取整|
|FORMAT (number,decimal_places )|保留小数位数（四舍五入）|
|RAND(seed)|返回随机数 其范围为 0 ≤ v ≤ 1.0|
* 1. 如果使用 rand()， 每次返回不同的随机数 ，在 0 ≤ v ≤ 1.0
* 2. 如果使用 rand(seed) 返回随机数, 范围 0 ≤ v ≤ 1.0, 如果 seed 不变，该随机数也不不会变
###时间日期函数

###加密函数-MD5()
>MD5信息摘要算法（英语：MD5 Message-Digest Algorithm），一种被广泛使用的密码散列函数，可以产生出一个128位（16字节）的散列值（hash value），**相同的数据MD5加密后是一样的**，不同的数据加密后理论上存在相同的可能。

MySQL提供函数 **MD5(str)**，计算一个给定字符串的 MD5 摘要，并将结果作为一个 **32 位的由十六进制字符** 组成的字符串返回。一个十六进制数据可以由4个二进制数据表示，正好128位数据。
通常我们将用户密码写入数据库时，保存的不是密码的原文，而是对密码进行加密后再保存到数据库中，这样可以防止当数据库泄漏后，对方拿到的不是密码的原文，账号就不会被盗走。当验证登录时，用户输入密码，对其进行MD5加密后与数据库中保存的MD5值进行比较，相同则登录成功。
~~~SQL
-- 插入数据，对密码‘yjh123456’进行加密
INSERT INTO `user` 
    VALUES(100,'yjh',MD5('yjh123456'))
~~~

###流程控制函数
主要由两种，用来对不同的数据作不同的处理
####IF语句
语法：**IF(exp1,exp2,exp3)**
说明：exp1为True时返回exp2,否则返回exp3
~~~SQL
-- 从emp表中查找数据comm为空时显示0.0，否则显示本身
SELECT ename,IF(comm IS NULL,0.0,comm) 
    FROM emp
~~~
**注意：这里判断是否为空，要使用 IS NULL，不为空使用 IS NOT NULL**
####CASE语句
相当于条件分支语句，可以对更多的可能情况做处理
语法：<strong>
SELECT CASE
WHEN exp1 THEN exp2
WHEN exp2 THEN exp2
END;</strong>
~~~SQL
SELECT ename,(SELECT CASE 
WHEN job = 'CLERK' THEN '职员' 
WHEN job='MANAGER' THEN '经理' 
WHEN job='SALESMAN'THEN'销售人员'
ELSE job 
END) AS'job'--对整列重命名 
FROM emp;
~~~
###分页查询
当我们查询返回的记录路条数过多，为了防止数据爆炸，可以使用分页查询，即查询满足条件的某几条记录
基本语法：<strong>
SELECT ...
LIMIT START ROWS;</strong>
作用，显示从START + 1 开始的 ROWS 条记录
###多字句查询
当查询的要求比较复杂时，需要用到多条字句，这些字句的顺序应该是：
<strong>SELECT column.. FROM table
    WHERE exp
    GROUP BY column
    HAVING condition
    ORDER BU colunm
    LIMIT start, rows</strong>
###多表查询

有时候我们需要查两个或两个以上的表的数据，只需要在 **FROM** 后面多添加一个表名即可，在不作任何 **WHRER字句过滤时**，得到的是一个**笛卡尔集**，它将一个表的每一条记录与其它表任何一条记录组合，因此得到的**总记录数是每个表的记录的乘积。**
~~~SQL
--使用where字句过滤多表查询的结果
SELECT ename,sal,dname,emp.deptno
FROM emp,dept
WHERE emp.deptno = dept.deptno--出现相同字段时，用表名.字段
~~~

####自连接
自连接是将同一张表当作两张表来查询，当我们需要两次查询一个字段但是查询的条件不一样时我们可以使用自连接来解决问题。使用自连接由几点需要注意：
* 因为是两张一样的表，所以我们要分别为这两张表指定别名，跟在表名后即可
* 最后为列名也指定别名
~~~SQL
--查询员工的名字并且根据它的上级编号再查出上级的名字
SELECT worker.ename AS'职员名',boss.ename AS'上级名'
FROM emp worker,emp boss
WHERE worker.mgr = boss.empno;
~~~
####外连接
如果我们在进行多表查询时，想全部保留其中一张表的内容，可以使用外连接，如果保留的表在右侧，则是右连接，在左侧就是左连接。

举个例子，比如在学生成绩表和学生信息表中，我们要显示所有人的成绩，并根据成绩表对应的学号找到学生相关的信息，但是有的人没有参加考试，没有它的成绩，但是我们也要输出它的信息，我们可以使用右连接全部显示学生信息表，在成绩这个字段，没有的则显示为空。

**语法：SELECT ... FROM 表1 LEFT JOIN 表二 ON [条件...]**
~~~SQL
SELECT `name`, stu.id, grade
FROM stu LEFT JOIN exam
ON stu.id = exam.id;
~~~

###子查询
>子查询是指嵌入在其它 sql 语句中的 select 语句,也叫嵌套查询，子查询分为单行子查询，多行子查询，多列子查询，同时子查询结果还可以作为一张表使用。
####单行子查询
单行子查询返回的是一行的查询数据
~~~SQL
-- 单行子查询：显示与 SMITH 同一部门的所有员工
SELECT *
FROM emp
WHERE deptno = (
SELECT deptno
FROM emp
WHERE ename = 'SMITH' )
~~~
####多行子查询
多行子查询使用 **IN** 关键字
~~~SQL
-- 如何查询和部门 10 的工作相同的雇员的
SELECT ename, job, sal, deptno
FROM emp
WHERE job IN (
SELECT DISTINCT job
FROM emp
WHERE deptno = 10
) AND deptno <> 10
~~~
####子查询作为临时表
可以把子查询得到的结果作为一张表，然后进行多表查询，这种查询方式能够解决很多复杂的查询问题，目前展示一个见到那的的案例，以后有更多的体悟再来丰富一下。
~~~SQL
--查询 ecshop 中各个类别中，价格最高的商品
--先得到 各个类别中，价格最高的商品 max + group by cat_id, 当做临时表
SELECT goods_id, ecs_goods.cat_id, goods_name, shop_price
FROM (SELECT cat_id , MAX(shop_price) AS max_price
        FROM ecs_goods
            GROUP BY cat_id) temp , ecs_goods
WhERE temp.cat_id = ecs_goods.cat_id
AND temp.max_price = ecs_goods.shop_price
~~~
**注释：关于这个查询需求，一开始我以为直接用 MAX+分组就好了，但是输出结果与目标不一致，这是因为这里还要输出商品号，商品号和这个最高价格不匹配，这是GROUP BY导致的，这里的商品号其实是每一组第一个成员的商品号**
####多列子查询
如果要求查询结果对多个字段有要求可以使用多列子查询
~~~SQL
-- 查询与 allen 的部门和岗位完全相同的所有雇员(并且不含 allen 本人)
SELECT *
FROM emp
WHERE (deptno , job) = (
SELECT deptno , job
FROM emp
WHERE ename = 'ALLEN' ) AND ename != 'ALLEN
~~~

###合并查询
有时候为了多个查询结构需要合并，可以使用集合操作符 **UNION**（去重）， **UNION ALL**（不去重）

**学习总结来源于[<u>韩顺平老师一周学会MYSQL</u>](https://www.bilibili.com/video/BV1H64y1U7GJ?spm_id_from=333.999.0.0&vd_source=352d2df9b015068a15a74f8ed4486f20)**
