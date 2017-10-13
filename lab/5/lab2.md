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
1.	熟悉SQL的**数据更新操作。**
2.	能够使用SQL语句对数据库进行**数据插入、更新、删除操作**。

## 二.  实验内容

在本次实验中,主要的内容是如何使用SQL语句对数据进行更新。

- 使用 INSERT INTO语句插入数据,包括插入一个元组或将子查询的结果插入到数据库中两种方式。
- 使用 SELECT INTO语句,产生一个新表并插入数据。
- 使用 UPDATE语句可以修改指定表中满足 WHERE子句条件的元组,有三种修改的方式:修改某一个元组的值,修改多个元组的值,带子查询的修改语句。
- 使用 DELETE语句删除数据:删除某一个元组的值,删除多个元组的值,带子查询的删除语句。 

## 三.  实验结果

1.  **向 STUDENTS表插入编号是“800022222”且姓名是“ WangLan”的元组。**

    ~~~sql
    INSERT 
    INTO STUDENTS
    VALUES ('800022222','WangLan',NULL,NULL)
    ~~~
    结果如下：
    <center>![1](E:\DataBase\lab\5\1.png)</center>

    查询添加后信息

    ~~~sql
    SELECT * FROM STUDENTS WHERE sid='800022222'
    ~~~

    <center>![1.1](E:\DataBase\lab\5\1_1.png)</center>

2.  **向 TEACHERS表插入元组(“200001000”,“LXL”,“s4zrck@pew,net”,“3024”)。**

    ~~~sql
    INSERT  INTO TEACHERS  VALUES ('200001000','LXL','s4zrck@pew.net','3024')
    ~~~
    结果如下：

    <center>![2](E:\DataBase\lab\5\1.png)</center>

    查询添加后信息

    ~~~sql
    SELECT * 
    FROM TEACHERS
    WHERE tid='200001000'
    ~~~

    <center>![1](E:\DataBase\lab\5\2.png)</center>

3.  **将 TEACHERS表中编号为“200010493”的老师工资改为4000。**

    ~~~sql
    UPDATE TEACHERS
    SET salary=4000
    WHERE tid='200010493'
    ~~~
    结果如下：

    <center>![3](E:\DataBase\lab\5\3.1.png)</center>

    查询修改后信息

    ~~~sql
    SELECT * 
    FROM TEACHERS
    WHERE tid='200010493'
    ~~~

    <center>![3](E:\DataBase\lab\5\3.png)</center>

4.  **将 TEACHERS表中所有工资小于2500的老师工资改为2500。**

    ~~~sql
    UPDATE TEACHERS
    SET salary=2500
    WHERE salary<2500
    ~~~
    结果如下：

    <center>![4](E:\DataBase\lab\5\4.png)</center>

    查询修改后信息

    ~~~sql
    SELECT * 
    FROM TEACHERS
    WHERE salary<=ALL(
    	SELECT salary
    	FROM TEACHERS
    	WHERE salary IS NOT NULL
    )
    ~~~

    <center>![4](E:\DataBase\lab\5\4.1.png)</center>

5.  **将编号为“200016731”的老师讲授的课程全部改成由姓名为“rnupx”的老师讲授**

    ~~~sql
    UPDATE CHOICES
    SET tid=(
    	SELECT tid
    	FROM TEACHERS
    	WHERE tname='rnupx'
    	)
    WHERE tid='200016731'
    ~~~
    结果如下：

    <center>![5](E:\DataBase\lab\5\5.png)</center>

    查询修改后tid=200016731的老师讲授的课程

    ~~~sql
    SELECT tid ,cid
    FROM CHOICES
    WHERE tid='200016731'
    ~~~

    <center>![5](E:\DataBase\lab\5\5.1.png)</center>

6.  **更新编号“800071780”的学生年级为“2001”。**

    ~~~sql
    UPDATE STUDENTS
    SET grade='2001'
    WHERE sid='800071780'
    ~~~
    结果如下：

    <center>![6](E:\DataBase\lab\5\6.png)</center>

    查询修改后学生信息

    ~~~sql
    SELECT *  
    FROM STUDENTS  
    WHERE sid='800071780'
    ~~~

    <center>![6](E:\DataBase\lab\5\6.1.png)</center>

7.  **删除没有学生选修的课程。**

    先查询一下有多少课程没人选修用于验证答案

    ~~~sql
    SELECT COURSES.cname as'课程名称'
    FROM COURSES
    WHERE COURSES.cid NOT IN(
    SELECT DISTINCT cid
    FROM CHOICES
    )
    ~~~

    <center>![6](E:\DataBase\lab\4\7.png)</center>

    ~~~sql
    DELETE 
    FROM COURSES
    WHERE COURSES.cid NOT IN(
    	SELECT DISTINCT cid
    	FROM CHOICES
    )
    ~~~
    结果如下：

    <center>![7](E:\DataBase\lab\5\7.png)</center>

8.  **删除年级高于1998的学生信息。**

    ~~~sql
    DELETE
    FROM CHOICES
    WHERE sid IN(
    	SELECT sid
    	FROM STUDENTS
    	WHERE grade<'1998'
    )
    DELETE 
    FROM STUDENTS
    WHERE grade<'1998'
    ~~~
    结果如下：

    <center>![8](E:\DataBase\lab\5\8.png)</center>

    查询一下删除结果

    ~~~sql
    SELECT sid
    FROM STUDENTS
    WHERE grade<'1998'
    ~~~

    <center>![8](E:\DataBase\lab\5\8.1.png)</center>

9.  **删除没有选修课程的学生信息。**

    先查询一下有多少学生没有选修课程

    ~~~sql
    SELECT *
    FROM STUDENTS
    WHERE sid NOT IN (
    	SELECT DISTINCT sid
    	FROM CHOICES
    )
    ~~~

    <center>![8](E:\DataBase\lab\5\9.1.png)</center>

    ~~~sql
    DELETE 
    FROM STUDENTS
    WHERE sid NOT IN (
    	SELECT DISTINCT sid
    	FROM CHOICES
    )
    ~~~
    结果如下：

    <center>![9](E:\DataBase\lab\5\9.png)</center>

10.  **删除成绩不及格的选课记录。**

  ~~~sql
  DELETE
  FROM CHOICES
  WHERE score<60
  ~~~
    结果如下：
    <center>![10](E:\DataBase\lab\5\10.png)</center>

  验证一下结果

  ~~~sql
  SELECT *
  FROM CHOICES
  WHERE score<=ALL(
  	SELECT score
  	FROM CHOICES
  )
  ~~~

   <center>![10](E:\DataBase\lab\5\10.1.png)</center>




## 四.  实验感想

<p style="text-indent: 2em"> 这次实验主要是增加、修改和删除。增加和修改都比较简单，增加只需要INSERT一下然后key对应就行了，插入其实就是就是查找然后SET。删除比较麻烦，因为一旦代码写错那么很有可能会出现误删的情况，同时因为会出现外键报错的情况，所以删除的时候可能需要先删除一张表的一部分才能删除我们想要的表，主要还是要理解一下外键约束的问题，理解之后就能正确的删除我们想要的部分了，删除之前最好选用SELECT看看我们要删除多少东西对比一下比较好。</p>