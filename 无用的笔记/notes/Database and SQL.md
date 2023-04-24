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