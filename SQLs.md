#一些课本上的SQL语句
##1. 定义基本表
###例3.5 建立一个学生表Student
```SQL
CREATE TABLE Student
	(Sno CHAR(9) PRIMARY KEY,  
	Sname CHAR(20) UNIQUE,  
	Ssex CHAR(2),  
	Sage SMALLINT,  
	Sdept CHAR(20) 	 
	);
```

###例3.6 建立一个课程表Course
```SQL
CREATE TABLE Course
	(Cno CHAR(4) PRIMARY KEY,
	Cname CHAR(40) NOT NULL,    /*列级完整性约束，Cname不能取空值*/
	Cpno CHAR(4) 				/*先修课*/
	Ccredit SMALLINT,
	FOREIGN KEY (Cpno) REFERENCES Course(Cno)    /*表级完整性约束条件，Cpno是外码，被参照表是Course，被参照列是Cno*/
	);	
```
###例3.7 建立学生选课表SC
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

## 2. 修改基本表
### 例3.8 向Student表增加"入学时间"列，其数据类型为日期型
```SQL
ALTER TABLE Student ADD S_entrance DATE;
```
> 不论基本表中原来是否已有数据，新增加的列一律为空值 
### 例3.9 将年龄的数据类型由字符型改为整数
```SQL
ALTER TABLE Student ALTER COLUMN Sage INT;
```
### 例3.10 增加课程名称必须取唯一值的约束条件
```SQL
ALTER TABLE Course ADD UNIQUE(Cname);
```
