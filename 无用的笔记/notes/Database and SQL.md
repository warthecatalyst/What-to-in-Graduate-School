# 数据库和SQL语句

## 初级SQL
1、创建一些数据库表
```SQL
DROP TABLE IF EXISTS department,course,instructor,favourite,section,teaches;

CREATE TABLE department
	(
		dept_id INT,
		dept_name varchar(20),
		building varchar(15),
		budget numeric(12,2),
		PRIMARY KEY (dept_id));
		
CREATE TABLE course
	(course_id INT,
	 title VARCHAR(20) NOT NULL,
	 dept_id INT,
	 credits NUMERIC(2,0),
	 PRIMARY KEY(course_id),
	 FOREIGN KEY(dept_id) REFERENCES department(dept_id));
	 
CREATE TABLE instructor
(
	iid INT,
	iname VARCHAR(20) NOT NULL,
	dept_id INT,
	salary NUMERIC(8,2),
	PRIMARY KEY (iid),
	FOREIGN KEY(dept_id) REFERENCES department(dept_id));
	
CREATE TABLE favourite
(
	userid INT NOT NULL,
	videoid INT NOT NULL,
	update_time date NOT NULL,
	PRIMARY KEY(userid,videoid)
);

CREATE TABLE section
(
	course_id INT,
	sec_id INT,
	semester VARCHAR(6),
	cyear NUMERIC(4,0),
	building VARCHAR(15),
	room_number VARCHAR(7),
	time_slot_id VARCHAR(4),
	PRIMARY KEY(course_id,sec_id,semester,cyear),
	FOREIGN KEY(course_id) REFERENCES course(course_id)
);

CREATE TABLE teaches
(
	iid INT,
	course_id INT,
	sec_id INT,
	semester VARCHAR(6),
	cyear NUMERIC(4,0),
	PRIMARY KEY(iid,course_id,sec_id,semester,cyear),
	FOREIGN KEY(course_id,sec_id,semester,cyear) REFERENCES section(course_id,sec_id,semester,cyear),
	FOREIGN KEY(iid) REFERENCES instructor(iid)
);

# Alter TABLE r add A D;
# 其中r是现有关系的名称，A是待添加属性的类型，D是待添加属性的类型
ALTER TABLE favourite ADD isDelete SMALLINT;

# 从关系中去除一个属性
ALTER TABLE r drop A;
```
SQL查询中使用distinct可以去除重复项，也可以使用关键字all来显示指明不去重。
select语句还可以带有+,-,*,/运算符的算数表达式，运算对象可以为常数或者元组的属性
```SQL
select distinct dept_name from instructor;

select all dept_name from instructor;

select ID,name,dept_name,salary * 1.1 from instructor;

select name from instructor where dept_name = 'Comp.Sci' and salary > 70000;

select T.name, S.course_id from instructor as T, teaches as S where T.ID = S.ID;

# 找出满足以下条件的所有教师的姓名，他们的工资至少比Biology系的某一位教师的工资要高
select distinct T.name from instructor as T, instructor as S where T.salary > S.salary and S.dept_name = 'Biology';

select * from instructor
order by salary desc, name asc;

select name from instructor
where salary 

# 并交差运算，都可以加上all保留重复项
（select course_id from section where semester='Fall' and year = 2017)
union (all)
(select course_id from section where semester='Spring' and year = 2018)

（select course_id from section where semester='Fall' and year = 2017)
intersect
(select course_id from section where semester='Spring' and year = 2018)

（select course_id from section where semester='Fall' and year = 2017)
except
(select course_id from section where semester='Spring' and year = 2018)

```

## 数据库经典面试题
### 数据库的6大范式级别
数据库范式主要是为解决关系数据库中数据冗余、更新异常、插入异常、删除异常问题而引入的设计理念。简单来说，数据库范式可以避免数据冗余，减少数据库的存储空间，并且减轻维护数据完整性的成本。是关系数据库核心的技术之一，也是从事数据库开发人员必备知识。

**第一范式**
强调属性的原子性约束，要求属性具有原子性，不可再分解。
举例：
学生表(学号、姓名、年龄、性别、地址)。地址可以细分为国家、省份、城市、市区、街道，那么该模式就没有达到第一范式。
第一范式存在问题：冗余度大、会引起修改操作的不一致性、数据插入异常、数据删除异常。

**第二范式**
第二范式，强调记录的唯一性约束，数据表必须有一个主键，并且没有包含在主键中的列必须完全依赖于主键，而不能只依赖于主键的一部分。
举例：
版本表(版本编码，版本名称，产品编码，产品名称)，其中主键是(版本编码，产品编码)，这个场景中，数据库设计并不符合第二范式，因为产品名称只依赖于产品编码。存在部分依赖。所以，为了使其满足第二范式，可以改造成两个表：版本表(版本编码，产品编码)和产品表(产品编码，产品名称)

**第三范式**
第三范式，强调数据属性冗余性的约束，也就是非主键列必须直接依赖于主键。也就是消除了非主属性对码的传递函数依赖。

举例：

订单表(订单编码，顾客编码，顾客名称)，其中主键是(订单编码)，这个场景中，顾客编码、顾客名称都完全依赖于主键，因此符合第二范式，但顾客名称依赖于顾客编码，从而间接依赖于主键，所以不能满足第三范式。如果要满足第三范式，需要拆分为两个表：订单表(订单编码，顾客编码)和顾客表(顾客编码，顾客名称)。

说明：3NF的模式肯定满足2NF。产生冗余和异常的两个重要原因是部分依赖和传递依赖。3NF模式中不存在非主属性对码的部分函数依赖和传递函数依赖，性能较好。1NF、2NF一般不适合作为数据库模式，通常需要转换为3NF或者更高级别的范式，这种变换过程称为关系模式规范化处理。

**BCNF范式**
属于修正的第三范式，是防止主键的某一列会依赖于主键的其他列。当3NF消除了主属性对码的部分函数依赖和传递函数依赖称为BCNF。

特性：

1、所有主属性对每一个码都是完全函数依赖

2、所有主属性对每一个不包含它的码，也是完全函数依赖

3、没有任何属性完全函数依赖与非码的任何一组属性

举例：库存表(仓库名，管理员名，商品名，数量)，主键为(仓库名，管理员名，商品名)，这是满足前面三个范式的，但是仓库名和管理员名之间存在依赖关系，因此删除某一个仓库，会导致管理员也被删除，这样就不满足BCNF。

**第四范式**
非主属性不应该有多值。如果有多值就违反了第四范式。4NF是限制关系模式的属性间不允许有非平凡且非函数依赖的多值依赖。

举例：用户联系方式表(用户id，固定电话，移动电话)，其中用户id是主键，这个满足了BCNF,但是一个用户有可能会有多个固定电话或者多个移动电话，那么这种设计就不合理，应该改为(用户id，联系方式类型，电话号码)。

说明：如果只考虑函数依赖，关系模式规范化程度最高的范式是BCNF;如果考虑多值依赖则是4NF。

## 什么是范式和反范式
范式是符合某一种级别的关系模式的集合。构造数据库必须遵循一定的规则。在关系数据库中，这种规则就是范式。

名称	优点	缺点
范式	范式化的表减少了数据冗余，数据表更新操作快、占用存储空间少。	查询时通常需要多表关联查询，更难进行索引优化
反范式	反范式的过程就是通过冗余数据来提高查询性能，可以减少表关联和更好进行索引优化	存在大量冗余数据，并且数据的维护成本更高
所以在平时工作中，我们通常是将范式和反范式相互结合使用。
————————————————
版权声明：本文为CSDN博主「程序猿周周」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/adminpd/article/details/122910606