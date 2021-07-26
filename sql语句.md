复制复制emp表 —— 新的表名称为myemp
CREATE TABLE myemp AS SELECT * FROM emp ;

增加数据
（1）插入一条新的数据 INSERT INTO 表名称[(列1,列2,列3,…)]VALUES(值1,值2,值3,…);
INSERT INTO myemp(empno,job,hiredate,ename,mgr,sal,comm,deptno) VALUES (8888,‘CLERK’,SYSDATE,‘TOM’,7369,800,100,20);不用输入的列，不写即可
增加一个没有领导、没有部门、没有奖金的新雇员
INSERT INTO myemp(empno,ename,job,hiredate,sal) VALUES(6612,’LEE‘,’CLERK‘,TO_DATE(’1982-10-19‘,’yyyy-mm-dd’),600);		
通过子查询插入数据 INSERT INTO 表名称[(列1,列2,列3,…)]子查询；
INSERT INTO myemp SELECT * FROM emp WHERE deptno=10 ; 把deptno=10的用户复制一遍

（二）修改数据
（1）指定要更新数据的内容 UPDATE 表名称 SET 字段=值 [,字段=值…][WHERE 更新条件
将SMITH（雇员编号为7369）的工资修改为3000元，并且每个月有500元的奖金
UPDATE myemp SET sal=3000,comm=500 WHERE empno=7369 ;
将工资低于公司平均薪金的雇员的基本工资上涨20%
select avg(sal) from emp; 查看平均工资
UPDATE myemp SET sal=sal*1.2WHERE sal<(SELECT AVG(sal) FROM myemp) ;
（2）基于子查询的更新
UPDATE 表名称 SET （列1,列2,…）=(SELECT 列1,列2,…FROM table WHERE 查询条件
将雇员7369的职位、基本工资、雇佣日期更新为与7839相同的信息
UPDATE myemp SET(job,sal,hiredate)=(SELECT job,sal,hiredate FROM myemp WHERE empno=7839) WHERE empno=7369 ;

（三）删除数据
DELETE FROM 表名称 [WHERE 删除条件]；
注：不写删除条件表示删除全部！
删除雇员编号是7566的雇员信息
DELETE FROM myemp WHERE empno=7566 ;