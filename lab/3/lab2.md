<top>

<body style="font-family: 'Times New Roman','楷体'; font-size: 1.2em"  >

<center><b style="font-size: 1.8em">中山大学数据科学与计算机学院本科生实验报告</b></center>

<center style="font-size: 1.2em">**（2017学年秋季学期）**</center>

<center style="font-size: 1.2em">**课程名称：数据库系统          任课教师：刘玉葆** </center>

| 年级，班级  |      15M1       |  专业（方向）   |     软件工程(移动信息工程）     |
| :----: | :-------------: | :-------: | :------------------: |
| **学号** |  **15352008**   |  **姓名**   |       **蔡荣裕**        |
| **电话** | **13727021990** | **Email** | **897389207@qq.com** |

##一.  实验目的	

1.    熟悉SQL的数据查询语言
2.    能够使用SQL语句对数据库进行单表查询、连接查询

## 二.  实验内容

1.  单表查询
   - 查询的目标表达式为所有列、指定列或指定列的运算。
   - 用 DISTINCT保留字消除重复行
   - 对查询结果排序和分组。
   - 集合分组使用集函数进行各项统计
2.  连接查询
   - 笛卡儿连接和等值连接。
   - 自连接
   - 外连接
   - 复合条件连接
   - 多表连接
## 三.  实验结果

1.  查询全部课程的详细记录
    ~~~sql
    SELECT * FROM COURSES
    ~~~
    结果如下：
    <center>![1](E:\DataBase\lab\3\1.png)</center>

2.  查询所有有选修课的学生的编号
    ~~~sql
    SELECT DISTINCT sid as'学号'
    FROM CHOICES
    ~~~
    结果如下：

    <center>![2](E:\DataBase\lab\3\2.png)</center>

3.  查询课时<88(小时)的课程的编号
    ~~~sql
    SELECT cid as'课程编号',cname as '课程名称',hour as '课时' 
    FROM  COURSES 
    WHERE hour < 88
    ~~~
    结果如下：

    <center>![3](E:\DataBase\lab\3\3.png)</center>

4.  请找出总分超过400分的学生

    将学生用sid分组然后计算总分

    ~~~sql
    SELECT CHOICES.sid as'学号', STUDENTS.sname as'学生姓名',
    SUM(CHOICES.score)as'总分'
    FROM  CHOICES,STUDENTS
    WHERE CHOICES.sid = STUDENTS.sid
    GROUP BY CHOICES.sid,STUDENTS.sname  HAVING SUM(CHOICES.score)>400
    ~~~
    结果如下：

    <center>![4](E:\DataBase\lab\3\4.png)</center>

5.  查询课程的总数
    ~~~sql
    SELECT COUNT(cid) as'课程总数' 
    FROM COURSES
    ~~~
    结果如下：

    <center>![5](E:\DataBase\lab\3\5.png)</center>

6.  查询所有课程和选修该课程的学生总数

    将所有课程按照cid分组计算sid总数

    ~~~sql
    SELECT CHOICES.cid as'课程编号' , COURSES.cname as '课程名称', 
    COUNT(CHOICES.sid)as'选修该课程学生总数' 
    FROM  CHOICES,COURSES
    WHERE CHOICES.cid = COURSES.cid 
    GROUP BY CHOICES.cid,COURSES.cname 
    ~~~
    结果如下：

    <center>![6](E:\DataBase\lab\3\6.png)</center>

7.  查询选修成绩超过60的课程超过两门的学生编号
    ~~~sql
    SELECT CHOICES.sid as'学号',STUDENTS.sname as'学生姓名'
    FROM  CHOICES,STUDENTS
    WHERE CHOICES.sid = STUDENTS.sid  AND CHOICES.score>60 
    GROUP BY CHOICES.sid ,STUDENTS.sname 
    HAVING COUNT(CHOICES.sid)>2
    ~~~
    结果如下：

    <center>![7](E:\DataBase\lab\3\7.png)</center>

8.  统计各个学生的选修课程数目和平均成绩

    将学生按照sid分组在CHOICES中计算选修课程总数和平均分

    ~~~sql
    SELECT CHOICES.sid as'学号',STUDENTS.sname as'学生姓名',
    COUNT(CHOICES.sid) as '选修课程数目',AVG(CHOICES.score) as '平均成绩' 
    FROM  CHOICES,STUDENTS
    WHERE CHOICES.sid = STUDENTS.sid 
    GROUP BY CHOICES.sid,STUDENTS.sname 
    ~~~
    结果如下：

    <center>![8](E:\DataBase\lab\3\8.png)</center>

9.  查询选修Java的所有学生的编号及姓名
    ~~~sql
    SELECT CHOICES.sid as'学号', STUDENTS.sname as'学生姓名'
    FROM  CHOICES,STUDENTS,COURSES
    WHERE COURSES.cname='java' AND CHOICES.cid=COURSES.cid AND CHOICES.sid = STUDENTS.sid
    ~~~
    结果如下：

    <center>![9](E:\DataBase\lab\3\9.png)</center>

10.  查询姓名为sssht的学生所选的课程的编号和成绩
  ~~~sql
  SELECT CHOICES.cid as '课程编号',COURSES.cname as '课程名称',CHOICES.score as '成绩' 
  FROM  CHOICES,STUDENTS,COURSES
  WHERE CHOICES.sid = STUDENTS.sid AND STUDENTS.sname='sssht' AND CHOICES.cid=COURSES.cid
  ~~~
    结果如下：
    <center>![10](E:\DataBase\lab\3\10.png)</center>


11.  查询其他课时比课程C++多的课程的名称
    ~~~sql
    SELECT Y.cname as'课程名称', Y.hour as '课时' 
    FROM  COURSES as X, COURSES as Y
    WHERE X.cname='c++' AND Y.cname !='c++' AND Y.hour>X.hour
    ~~~
    结果如下：

    <center>![11](E:\DataBase\lab\3\11.png)</center>

## 四. 实验感想

<p style="text-indent: 2em">本次实验主要是学习如何使用SQL语句进行查询，要我们完成的11个查询中有基本都是单表查询，除了几个需要连接查询的。连接查询需要增加一个主键外键相等的条件，不然自然连接的时候很容易出现重复导致一个查询跑了很久还没有出来。其他时候就是要注意语法的正确性，在多条件查询的时候还要主要查询的顺序，否则会得到不理想的结果</p>