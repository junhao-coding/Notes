#MySQL约束
>约束指对字段的约束，用于确保数据库的数据满足特定的规则。在MySQL中，数据库的约束包括，**NOT NULL，PRIMARY KEY，UNIQUE，FOREIGN KEY，CHECK** 五种

##PRIMARY KEY(主键)
对字段定义主键约束后，该列数据不能有重复且不能为空，一张表只能唯一定义,但是可以定义复合主键（多个字段一起视为一个主键）
###在字段后定义
~~~SQL
CREATE TABLE freinds
(id INT PRIMARY KEY, -- 表示 id 列是主键
`name` VARCHAR(32),
email VARCHAR(32));
~~~
###在末尾定定义
通常用于定义复合主键，插入数据的多个字段不能同时相等，否则报错。
~~~SQL
CREATE TABLE freinds
(id INT
`name` VARCHAR(32),
email VARCHAR(32)
RIMARY KEY (id, `name`) -- 定义复合主键
);
~~~
###自增长
一般用自增长配合主键使用，且主键是整型的数据类型，作用是不需要插入数据，主键列会自动增长。
**语法：字段名 整型 PRIMARY KEY auto_increment**
~~~SQL
CREATE TABLE freinds
(id INT PRIMARY KEY AUTO_INCREMENT --好友id号自动增长
`name` VARCHAR(32),
email VARCHAR(32));
~~~
说明：
* 自增长默认从1开始增加，也可以通过 **ALTER TABLE 表名 AUTO_INCREMENT = 初始值** 来设置。
* 如果给自增长字段指定了值，会使用指定值，而不是自增长。
##UNIQUE(唯一)
当定义了UNIQUE约束后，数据不能重复，但是可以为空，一张表可以定义一个多个UNIQUE约束。
~~~SQL
CREATE TABLE t21
(id INT UNIQUE , . 
`name` VARCHAR(32) , 
email VARCHAR(32) UNIQUE --邮箱信息不可以重复但是可以为空
);
~~~
##FOREIGN KEY(外键约束)
用于定义主表和从表的关系，外键约束定义在从表上，**主表必须要求有主键约束或者UNIQUE约束**。定义了外键约束后，要求**外键的列数据必须在主表的主键列上存在或者为NULL**。

**语法：FOREIGN KEY 本表字段名 REFERENCES 主表名（主键名或者UNIQUE字段名）**
~~~SQL
-- 主表class
CREATE TABLE class (
id INT PRIMARY KEY , -- 班级编号
`name` VARCHAR(32) NOT NULL DEFAULT ''); 
-- 从表 stu
CREATE TABLE stu (
id INT PRIMARY KEY , -- 学生编号
`name` VARCHAR(32) NOT NULL DEFAULT '', 
class_id INT , -- 学生所在班级的编号
FOREIGN KEY stu(class_id) REFERENCES class(id)) --指定外键
--主表插入数据
INSERT INTO class
VALUES(100, 'java'), (200, 'web');
--向从表插入数据后
INSERT INTO stu
VALUES(3, 'hsp', 300) --在主表中没有为300的班级，因此无法插入
~~~

##CHECK
强制要求插入数据必须满足一定条件，否则不能插入。MySQL8.0支持CHECK约束（MySQL5.7好像不支持）
~~~SQL
CREATE TABLE emp (
id INT PRIMARY KEY, 
`name` VARCHAR(32) , 
sex VARCHAR(6) ,
sal DOUBLE CHECK ( sal > 1000 AND sal < 2000)
);
~~~
**学习总结来源于[<u>韩顺平老师一周学会MYSQL</u>](https://www.bilibili.com/video/BV1H64y1U7GJ?spm_id_from=333.999.0.0&vd_source=352d2df9b015068a15a74f8ed4486f20)**