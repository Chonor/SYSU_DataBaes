<top>

<body style="font-family: 'Times New Roman','楷体'; font-size: 1.2em;  white-space: pre-wrap"  >

<center><b style="font-size: 1.8em">中山大学数据科学与计算机学院本科生实验报告</b></center>

<center style="font-size: 1.2em">**（2017学年秋季学期）**</center>

<center style="font-size: 1.2em">**课程名称：数据库系统          任课教师：刘玉葆** </center>

| 年级，班级  |      15M1       |  专业（方向）   |     软件工程(移动信息工程）     |
| :----: | :-------------: | :-------: | :------------------: |
| **学号** |  **15352008**   |  **姓名**   |       **蔡荣裕**        |
| **电话** | **13727021990** | **Email** | **897389207@qq.com** |

##一.  实验目的
1.	熟悉SQL的数据查询语言
2.	能够使用SQL语句对数据库进行嵌套查询、集合运算。 

## 二.  实验内容

1.    嵌套查询
     - 通过实验验证对子查询的两个限制条件：不能使用order by子句；外层select语句变量可以用在子查询中，但反之不行。
     - 体会相关子查询和不相关子查询的不同：相关子查询要使用外层select语句提供的数据；不相关子查询即内层子查询不依赖于外层select语句。
     - 考察4类谓词的用法,包括:
       - 第1类,IN, NOT IN
       - 第2类,带有比较运算符的子查询
       - 第3类,SOME,ANY或ALL谓词的子查询
       - 第4类,带有 EXISTS谓词的子查询。
2.    连接查询
     - 使用保留字 UNION、INTERSECT、EXCEPT进行集合或、交、减运算。
## 三.  实验结果

1.  查询选修C++课程的成绩比姓名为 ZNKOO的学生高的所有学生的编号和姓名

    先找出ZNKOO的c++成绩，然后在找出c++成绩比ZNKOO高的学生

    ~~~sql
    SELECT DISTINCT STUDENTS.sid as'学号',
    STUDENTS.sname as'学生姓名'
    FROM CHOICES,STUDENTS,COURSES
    WHERE CHOICES.cid=COURSES.cid
    AND COURSES.cname='c++'
    AND CHOICES.sid=STUDENTS.sid
    AND CHOICES.score>(
    SELECT CHOICES.score
    FROM CHOICES,STUDENTS,COURSES
    WHERE STUDENTS.sname='znkoo'
    AND CHOICES.sid=STUDENTS.sid
    AND COURSES.cname='c++'
    AND CHOICES.cid=COURSES.cid
    )
    ~~~
    结果如下：
    <center>![1](E:\DataBase\lab\4\1.png)</center>

2.  找出和学生883794999或学生850955252的年级一样的学生的姓名

    先找出学生883794999，学生850955252的年级，然后再找出年级一样的学生

    ~~~sql
    SELECT sname as'学生姓名',
    grade as '年级'
    FROM STUDENTS
    WHERE grade IN(
    	SELECT grade
    	FROM STUDENTS
    	WHERE sid='883794999' OR  sid='850955252'
    )
    ~~~
    结果如下：

    <center>![2](E:\DataBase\lab\4\2.png)</center>

3.  查询没有选修Java的学生名称

    先找出选修java的学生名字然后用NOT IN排除

    ~~~sql
    SELECT sname as'学生姓名'
    FROM STUDENTS
    WHERE sname NOT IN(
    	SELECT STUDENTS.sname
    	FROM COURSES,STUDENTS,CHOICES
    	WHERE COURSES.cname='java'
    	AND CHOICES.cid=COURSES.cid
    	AND STUDENTS.sid=CHOICES.sid
    )
    ~~~
    结果如下：

    <center>![3](E:\DataBase\lab\4\3.png)</center>

4.  找出课时最少的课程的详细信息

    ~~~sql
    SELECT * FROM COURSES WHERE COURSES.hour<=ALL(
    	SELECT hour FROM COURSES WHERE hour IS NOT NULL
    )
    ~~~
    结果如下：

    <center>![4](E:\DataBase\lab\4\4.png)</center>

5.  查询工资最高的教师的编号和开设的课程号

    先找出工资最高的教师，他们在找出开课编号

    ~~~sql
    SELECT TEACHERS.tid as '教师编号',
    CHOICES.cid as '课程编号'
    FROM CHOICES,TEACHERS
    WHERE TEACHERS.tid=CHOICES.tid 
    AND TEACHERS.salary>=ALL(
    	SELECT salary
    	FROM TEACHERS
    	WHERE salary IS NOT NULL
    )
    ~~~
    结果如下：

    <center>![5](E:\DataBase\lab\4\5.png)</center>

6.  找出选修课程ERP成绩最高的学生编号

    先找出ERP课程的最高分，在找出学生编号

    ~~~sql
    SELECT STUDENTS.sid as'学号',
    STUDENTS.sname as'学生姓名'
    FROM  CHOICES,STUDENTS,COURSES
    WHERE  CHOICES.sid=STUDENTS.sid 
    AND CHOICES.cid=COURSES.cid
    AND COURSES.cname='erp'
    AND CHOICES.score>=ALL(
    	SELECT CHOICES.score
    	FROM CHOICES,COURSES
    	WHERE COURSES.cname='erp' 
    	AND CHOICES.cid=COURSES.cid
    	AND CHOICES.score IS NOT NULL
    ) 
    ~~~
    结果如下：

    <center>![6](E:\DataBase\lab\4\6.png)</center>

7.  查询没有学生选修的课程名称

    找出CHOICES 所有的课程编号，在COURSE中排除

    ~~~sql
    SELECT COURSES.cname as'课程名称'
    FROM  COURSES
    WHERE COURSES.cid NOT IN(
    	SELECT DISTINCT cid
    	FROM CHOICES
    )
    ~~~
    结果如下：

    <center>![7](E:\DataBase\lab\4\7.png)</center>

8.  查询讲授课程UML的教师所讲授的所有课程名称

    先找出教授UML的教师，在找出其开设课程的cid，再去找出cname

    ~~~sql
    SELECT DISTINCT cname as'课程名称'
    FROM COURSES
    WHERE cid IN(
    	SELECT DISTINCT cid
    	FROM CHOICES
    	WHERE tid IN (
    		SELECT DISTINCT CHOICES.tid
    		FROM CHOICES,COURSES
    		WHERE CHOICES.cid=COURSES.cid
    		AND COURSES.cname='uml'
    	)
    )
    ~~~
    结果如下：

    <center>![8](E:\DataBase\lab\4\8.png)</center>

9.  使用集合交运算，查询既选修了database又选修了UML课程的学生编号
    ~~~sql
    SELECT sid as'学号'
    FROM CHOICES
    WHERE cid IN(
    	SELECT cid FROM COURSES WHERE  COURSES.cname='database'
    )
    INTERSECT
    SELECT sid
    FROM CHOICES
    WHERE cid IN(
    	SELECT cid FROM COURSES WHERE  COURSES.cname='uml'
    )
    ~~~
    结果如下：

    <center>![9](E:\DataBase\lab\4\9.png)</center>

10.  使用集合减运算，查询选修了database却没有选修UML课程的学生编号
  ~~~sql
  SELECT sid as'学号'
  FROM CHOICES
  WHERE cid IN(
  	SELECT cid FROM COURSES WHERE  COURSES.cname='database'
  )
  EXCEPT
  SELECT sid
  FROM CHOICES
  WHERE cid IN(
  	SELECT cid FROM COURSES WHERE  COURSES.cname='uml'
  )
  ~~~
    结果如下：
    <center>![10](E:\DataBase\lab\4\10.png)</center>




## 四. 实验感想

<p style="text-indent: 2em">这次实验是学习嵌套查询和集合运算，实际使用时嵌套查询和集合运算大多数可以相互转换，集合运算在实际使用中思路比嵌套查询更加简单，但是查询复杂度来说集合运算复杂度更高效率更低，嵌套查询使用中需要有一定的逻辑思维需要倒着想才能先打外层代码，不然就把代码先打内层查询，然后在转到外层查询。</p>