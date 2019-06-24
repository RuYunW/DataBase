# 数据库的一些定义
## 第一章
### DB、DBS、DBMS
数据：描述事物的符号记录称为数据  
数据库：长期存储在计算机内、有组织的、可共享的大量数据的集合  
数据库管理系统：位于用户和操作系统间的数据管理软件，提供数据定义语言DDL，数据操纵语言DML  
数据库系统：由数据库、数据库管理系统（及其应用开发工具）、应用程序和数据库管理员（DBA）组成的存储、管理、处理和维护数据的系统  
数据库系统的特点：数据结构化、数据共享性高、冗余度低易扩充、数据独立性高、由DBMS统一管理和控制  
数据库是长期储存在计算机内有组织、大量、共享的数据集合  
### 模型
两类数据模型：概念模型、逻辑模型和物理模型  
概念模型（实体、属性、码、实体型、实体集、联系）  
实体间联系：1-1，1-n，n-n  
数据模型通常由数据结构、数据操作、数据完整性约束组成  
数据结构：描述数据库的组成对象及对象间联系  
### 数据库的二级映像功能与数据独立性
外模式-模式：模式改变，外模式不变，逻辑独立性，应用程序不变  
模式-内模式：数据库存储结构改变，模式不变，应用程序不变，物理独立性  

## 第二章
### 关系模式
R（U,D,DOM,F）

### 关系操作
五种基本操作：选择、投影、并、差、笛卡尔积  
传统集合运算：并、差、交、笛卡尔积  

### 关系完整性
三类完整性约束：实体完整性、参照完整性、用户定义完整性  

## 第三章
### SQL概述
SQL特点：综合统一、高度非过程化、面向集合的操作方式、以同一种语法结构提供多种使用方式、语言简洁易学易用  
视图：从一个或几个基本表导出的表，数据库只存放定义，不存放数据，虚表，概念上与基本表等同  

### 基本表的定义、删除与修改
#### 1. 定义基本表

##### 例3.5 建立一个学生表Student
```SQL
CREATE TABLE Student
	(Sno CHAR(9) PRIMARY KEY,  
	Sname CHAR(20) UNIQUE,  
	Ssex CHAR(2),  
	Sage SMALLINT,  
	Sdept CHAR(20) 	 
	);
```

##### 例3.6 建立一个课程表Course
```SQL
CREATE TABLE Course
	(Cno CHAR(4) PRIMARY KEY,
	Cname CHAR(40) NOT NULL,    /*列级完整性约束，Cname不能取空值*/
	Cpno CHAR(4) 				/*先修课*/
	Ccredit SMALLINT,
	FOREIGN KEY (Cpno) REFERENCES Course(Cno)    /*表级完整性约束条件，Cpno是外码，被参照表是Course，被参照列是Cno*/
	);	
```
##### 例3.7 建立学生选课表SC
```SQL
CREATE TABLE SC
	(Sno CHAR(9),
	Cno CHAR(4),
	Grade SMALLINT,
	PRIMARY KEY (Sno,Cno),
	FOREIGN KEY(Sno) REFERENCES Student(Sno),
	FOREIGN KEY(Cno) REFERENCES Course(Cno)
	);
```

#### 2. 修改基本表
##### 例3.8 向Student表增加"入学时间"列，其数据类型为日期型
```SQL
ALTER TABLE Student ADD S_entrance DATE;
```
> 不论基本表中原来是否已有数据，新增加的列一律为空值 
##### 例3.9 将年龄的数据类型由字符型改为整数
```SQL
ALTER TABLE Student ALTER COLUMN Sage INT;
```
##### 例3.10 增加课程名称必须取唯一值的约束条件
```SQL
ALTER TABLE Course ADD UNIQUE(Cname);
```
#### 3. 删除基本表
##### 删除Student表
```SQL
DROP TABLE Student CASCADE;
```
> SC通过外码Sno引用Student，也被级联删除
> RESTRICT/CASCADE
### 4. 索引的建立与删除
#### 建立索引
```SQL
CREATE UNIQUE INDEX Stusno ON Student(Sno);
CREATE UNIQUE INDEX Coucno ON Course(Cno);
CREATE UNIQUE INDEX SCno ON SC(Sno ASC,Cno DESC);
```

#### 修改索引
```SQL
ALTER INDEX SCno RENAME TO SCSno;
```

#### 删除索引
```SQL
DROP INDEX Stusname;
```

### 数据查询
#### 单表查询
```SQL
SELECT Sname,2014-Sage FROM Student WHERE Sdept in('CS','MA','IS');
```
```SQL
SELECT DISTINCE Sno FROM SC WHREE Grade<60 AND Sage BETWEEN 20 AND 30;
```
```SQL
SELECT Sname,Sno FROM  Student WHERE Sname LIKE '_阳%' ORDER BY Sno DESC;
```
> \转义通配符

```SQL
SELECT AVG(Grade) FROM CS WHERE Cno='1';
```
```SQL
SELECT Cno,COUNT(Sno) FROM SC GROUP BY Cno;
```
```SQL
SELECT Sno FROM CS GROUP BY Sno HAVING COUNT(*)>3;
```
```SQL
SELECT Sno,AVG(Grade) FROM SC GROUP BY Sno HAVING AVG(Grade)>=90;
```
> WHERE 作用于基本表或视图，从中选择满足条件的元组；HAVING 作用于组，从中选择满足条件的组

### 数据更新
```SQL
INSERT INTO Student (Sno,Sname,Ssex,Sdept,Sage) VALUES ('2012111','杨超越','女','CS',18);
```
```SQL
CREATE TABLE Dept_age
	(Sdept CHAR(15),
	Avg_age SMALLINT);
INSERT INTO Dept_age(Sdept,Avg_age) SELECT Sdept,AVG(Sage) FROM Student GROUP BY Sdept;
```
### 修改数据
```SQL
UPDATE Student SET Sage=22 WHERE Sno='20120001';
```
```SQL
UPDATE Student SET Sage=Sage+1;
```

### 删除数据
```SQL
DELETE FROM Student WHERE Sno='20120001';
```

```SQL
DELETE FROM SC WHERE Sno IN (
	SELECT Sno FROM Student WHERE Sdept='CS';
	)
```
### 视图
```SQL
CREATE VIEW IS_Student AS SELECT Sno,Sname,Sage FROM Student WHERE Sdepy='iS' WITH CHECK OPTION;
```
```SQL
CREATE VIEW IS_S1(Sno,Sname,Grade) 
	AS 
	SELECT Student.Sno,Sname,Grade 
	FROM Student,SC 
	WHERE Sdept='IS' AND 
		Student.Sno=SC.Sno AND 
		CS.Cno='1';
```
## 第四章
### 概述
数据库安全性：保护数据库以防止不合法使用所造成的数据泄露、更改或破坏  

## 第五章
数据库完整性：数据的正确性和相容性  
数据库管理系统必须能够实现如下功能  
> 1. 提供定义完整性约束的条件和机制
> 2. 提供完整性检查的方法
> 3. 进行违约处理

### 完整性约束命名子句
```SQL
CREATE TABLE Student
	(Sno NUMERIC(6)
		CONSTRAINT C1 CHECK (Sno BETWEEN 90000 AND 99999),
	Sname CHAR(20)
		CONSTRAINT C2 NOT NULL,
	Sage NUMERIC(3)
		CONSTRAINT C3 CHECK (Sage<30),
	Ssex CHAR(2)
		CONSTRAINT C4 CHECK (Ssex IN ('男','女')),
		CONSTRAINT StudentKey PRIMARY KEY(Sno)
	);
```


