# postgresql-50
50条经典sql语句的postgresql实现
##### 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数

> select score from sc a left out join sc b
>
> ```
> select * from Student RIGHT JOIN (
>  select t1.SId, class1, class2 from
>        (select SId, score as class1 from sc where sc.CId = '01')as t1, 
>        (select SId, score as class2 from sc where sc.CId = '02')as t2
>  where t1.SId = t2.SId AND t1.class1 > t2.class2
> )r 
> on Student.SId = r.SId
> ```

------

- 查询同时存在" 01 "课程和" 02 "课程的情况

  > select * from student where sid in(select sid from sc where cid='01' intersect select sid from sc where cid='02');

  ------

- 查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null )

  > select * from (select * from sc where sc.cid='01') as a left  join   
  >
  > (select * from sc where sc.cid='02') as b on a.sid=b.sid;

  ------

- 查询不存在" 01 "课程但存在" 02 "课程的情况

  > select * from student where sid in(select sid from sc where cid='02'except select sid from sc where cid='01');

  - 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩

  > select sid,avg(score) from sc group by sid having avg(score)>60;

  ------

- 查询在 SC 表存在成绩的学生信息

  > select * from student where sid in (select distinct sid from sc);

  ------

- 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null )

  > select sname,sum,num,student.sid from student inner join (select sid,sum(score) as sum,count(cid) as num from sc group by sid) as b on student.sid=b.sid;

- 查询「李」姓老师的数量

  > select count(tid) from teacher where teacher.tname like '李%' ;

- 查询学过「张三」老师授课的同学的信息

  > select * from sc,student where student.sid=sc.sid and cid in(select cid from teacher,course where teacher.tid=course.cid and tname='张三');

- 查询没有学全所有课程的同学的信息

  > select * from student where sid in(select sid from sc group by sid having count(cid)!=3);

- 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息

  > select * from student where student.sid not in(select student.sid from student,sc where student.sid=sc.sid and cid not in (select cid from sc where sid='01'));

  #### 查询和" 01 "号的同学学习的课程 完全相同的其他同学的信息

  > select sid, count(cid) from sc group by sid having count(cid)=3;

> ```
> select t2.sid from ( select sid, cid from sc where sid = '01' ) t1 
> left join (
> select sid, cid from sc where sid != '01'
> ) t2 on t1.cid = t2.cid
> left join (
> select sid, count(cid) cid_cnt from sc where sid = '01' group by sid
> ) t3 on t1.sid = t3.sid
> group by t2.sid 
> having count(t2.sid) = max(distinct t3.cid_cnt)
> order by t2.sid;
> ```

如果某同学和01号同学选课数目相等并且01号同学没有选的课，这位同学也没选，就可以认为二者选课相同.  两个条件取交集出学号

( 对01号同学没有选择但是选择了其中的课程的同学过滤)

```postgresql
select sid from sc  where sid not in(Select distinct sid from sc where cid in ( select cid from course where  cid not in (select cid from sc where sid='01') ))；
```

（和01号同学选择的课程数目相等的学生号）

```
Select sid from sc group by sid having count(cid)=(select count(cid) from sc where sid='01' ) 
```

最终：

```
select sid from sc  where sid not in(Select distinct sid from sc where cid in ( select cid from course where  cid not in (select cid from sc where sid='01') ))
Intersect
Select sid from sc group by sid having count(cid)=(select count(cid) from sc where sid='01' ) ;
```





- 查询没学过"张三"老师讲授的任一门课程的学生姓名

  > select distinct sname from student where sid not in(select sid from sc where cid=(select cid from course,teacher where teacher.tid=course.tid and tname='张三'));

- 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩

  > select * from student inner join(select sid,count(cid),avg(score) from sc where score<60 group by sid having count(cid)>=2) as foo on student.sid=foo.sid;

- 检索" 01 "课程分数小于 60，按分数降序排列的学生信息

  > select * from student,sc where student.sid=sc.sid and score<60 order by score desc;

- 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩

  > select * from sc left join(select sc.sid,avg(score) as a from sc group by sid)as foo on sc.sid=foo.sid order by a desc;

  

  - 查询各科成绩最高分、最低分和平均分：

  以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率

  及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90

  要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列

  > select sc.cid,max(score),min(score),avg(score),sum(case when 
  > score>=60 then 1 else 0 end) as 及格人数,sum(case when 
  > score between 70 and 80 then 1 else 0 end) as 中等人数,sum(case when 
  > score between 80 and 90 then 1 else 0 end) as 优良人数,sum(case when 
  > score>=90 then 1 else 0 end) as 优秀人数 from sc group by cid order by count(*) desc ,cid  ;

- 按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺

  > select sid,score,cid,rank() over(partition by cid order by score desc)from sc;

- 按各科成绩进行排序，并显示排名， Score 重复时合并名次

  > select sid,score,cid,dense_rank() over(partition by cid order by score desc)from sc;

- 查询学生的总成绩，并进行排名，总分重复时保留名次空缺

  > select sid,sum(score),rank() over(order by sum(score)) from sc group by sid;

- 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺

  > select sid,sum(score),dense_rank() over(order by sum(score)) from sc group by sid;

  - 统计各科成绩各分数段人数：课程编号，课程名称，[100-85]，[85-70]，[70-60]，[60-0] 及所占百分比

  > select sc.cid,max(score),min(score),avg(score),sum(case when 
  > score between 85 and 100 then 1 else 0 end) as 优秀,sum(case when score between 75 and 80 then 1 else 0 end) as 优良,sum(case when score between 60 and 70 then 1 else 0 end) as 良好,sum(case when score<=60 then 1 else 0 end) as 及格 from sc group by cid;

  - 查询各科成绩前三名的记录

  > select sid,score,cid  from sc order by score desc limit 9;
  >
  > select sid,score from sc group by cid order by score desc limit3;

> ​    

- 查询每门课程被选修的学生数

  > select count(sid) from sc group by cid;

- 查询出只选修两门课程的学生学号和姓名

  > select sname,sid from student where sid in(select sid from sc group by sid having count(cid)=2);

- 查询男生、女生人数

  > select count(ssex) from student group by ssex;

- 查询名字中含有「风」字的学生信息

  > select * from student,sc where student.sid=sc.sid and sname like '%风%';

- 查询同名同性学生名单，并统计同名人数

  > select sname,count(sname) from student group by sname having count(sname)>1;

- 查询 1990 年出生的学生名单

  > select sname from student where sage between '1990-1-1' and '1990-12-31';

- 查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列

  > select cid,avg(score) from sc group by cid order by avg(score) desc,cid;

- 查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩

  > select * from student as a inner join (select sc.sid,avg(score) from sc group by sc.sid having avg(score)>=85) as b on a.sid=b.sid;

- 查询课程名称为「数学」，且分数低于 60 的学生姓名和分数

  > select student.sid,score,sname from sc,course,student where sc.cid=course.cid and sc.sid=student.sid and cname='数学' and score<60;

- 查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）

  > select * from student left join sc on student.sid=sc.sid;

- 查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数

  > select sname,cname,score from sc,course,student where sc.sid=student.sid and sc.cid=course.cid and score>70;

- 查询不及格的课程

  > select sc.cid,cname from sc,course where sc.cid=course.cid and score<=60;

- 查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名

  > select sname,sc.sid from sc,student where sc.sid=student.sid and cid='01' and score>80;

- 求每门课程的学生人数

  > select count(cid) from sc group by cid;

- 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

  > select * from sc,student where student.sid=sc.sid and cid in(select cid from teacher,course where teacher.tid=course.cid and tname='张三') order by score desc limit 1;

  #### 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

  > select score,rank() over(order by score desc) from( select * from sc,student where student.sid=sc.sid and cid in(select cid from teacher,course where teacher.tid=course.cid and tname='张三')) as foo ;

  ### #查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩

  > 

  - 查询每门功成绩最好的前两名

  > select * from sc order by cid,score desc;

- 统计每门课程的学生选修人数（超过 5 人的课程才统计）。

  > select count(sid),cid from sc group by cid having count(sid)>=5;

- 检索至少选修两门课程的学生学号

  > select sid from sc group by sid having count(cid)>=2;

- 查询选修了全部课程的学生信

  > select * from student where sid in (select sid from sc group by sid having count(cid)=3);

- 查询各学生的年龄，只按年份来算

  > select sname,sid, ('2019-1-1'-sage)/365 as age from student;

- 按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一

  > 

- 查询本周过生日的学生

  > select extract(week from timestamp  '2000-02-02');
  >
  > select extract(week from current_timestamp );

- 
