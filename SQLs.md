#####例3.5 建立一个学生表Student
```

	CREATE TABLE Student
	(Sno CHAR(9) PRIMARY KEY,  
	Sname CHAR(20) UNIQUE,  
	Ssex CHAR(2),  
	Sage SMALLINT,  
	Sdept CHAR(20) 	 
	);
```

#####例3.6 建立一个课程表Course
CREATE TABLE Course
	(Cno CHAR(4) PRIMARY KEY,
	Cname CHAR(40) NOT NULL,    /*列级完整性约束，Cname不能取空值*/
	Cpno CHAR(4) 				/*先修课*/
	Ccredit SMALLINT,
	FOREIGN KEY (Cpno) REFERENCES Course(Cno)    /*表级完整性约束条件，Cpno是外码，被参照表是Course，被参照列是Cno*/
	);	

#####例3.7 建立学生选课表SC
CREATE TABLE SC
	(Sno CHAR(9),
	Cno CHAR(4),
	Grade SMALLINT,
	PRIMARY KEY (Sno,Cno),
	FOREIGN KEY(Sno) REFERENCES Student(Sno),
	FOREIGN KEY(Cno) REFERENCES Course(Cno)
	);

