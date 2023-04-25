# oracle 练习题

[常用命令](https://blog.csdn.net/K346K346/article/details/52106044?ops_request_misc=&request_id=&biz_id=102&utm_term=mysql%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-52106044.nonecase&spm=1018.2226.3001.4187)

```mysql
-- 1.查询语句
SELECT empno,ename,job,mgr,hiredate,sal,comm,deptno FROM emp;
SELECT ename,job,mgr,hiredate,sal,comm,deptno FROM emp;
SELECT empno,ename,job,mgr,hiredate,sal,comm,deptno FROM emp WHERE sal >= 1000 and sal <= 2000;
select * from emp where sal between 1000 and 2000;
select deptno from emp;
select DISTINCT deptno from emp;
-- %表示0个或者多个字符
-- _表示一个字符
SELECT * from emp WHERE ename like 'A%';
SELECT * from emp WHERE ename like '_L%';
SELECT * from emp WHERE comm is NULL;
SELECT avg(sal) from emp;
SELECT sum(sal) from emp;
SELECT max(sal) from emp;
SELECT min(sal) from emp;
SELECT count(empno) from emp;
SELECT * from emp WHERE sal = (SELECT max(sal) FROM emp);
SELECT * from emp WHERE sal = (SELECT min(sal) FROM emp);
select deptno,avg(sal) from emp group by deptno;
-- 查询出高于10号部门的平均工资的员工信息
SELECT * from emp WHERE sal > (SELECT avg(sal) from emp WHERE deptno = 10) ; -- select avg(sal) from emp WHERE deptno = 10;
-- 查询出比10号部门任何员工薪资高的员工信息
SELECT * from emp WHERE sal > any(SELECT sal from emp WHERE deptno = 10);
SELECT * from emp WHERE sal > (SELECT min(sal) from emp WHERE deptno = 10);
-- 和10号部门同名同工作的员工信息
SELECT ename,job FROM emp WHERE deptno = 10;
SELECT * from emp where (ename,job) in (SELECT ename,job FROM emp WHERE deptno = 10);
-- Select接子查询
-- 获取员工的名字和部门的名字
SELECT emp.ename, dept.dname from emp JOIN dept on emp.deptno = dept.deptno;
SELECT e.ename, d.dname from emp e JOIN dept d on e.deptno = d.deptno;
select * from emp where job = 'MANAGER';
SELECT * from emp WHERE sal > (SELECT avg(sal) from emp WHERE deptno = 10);
-- 有哪些部门的平均工资高于30号部门的平均工资
select deptno, avg(sal) from emp group by deptno;
select deptno from emp group by deptno having avg(sal) > (select avg(sal) from emp where deptno =30);
select * from emp where sal > (select sal from emp where ename = 'JONES');
select * from emp where deptno=(select deptno from emp where ename = 'SCOTT');
-- 工资高于30号部门所有人的员工信息
SELECT * from emp WHERE sal > all(SELECT sal FROM emp WHERE deptno = 30);
select * from emp where sal > (select max(sal) from emp where deptno=30);
-- 查询工作和工资与MARTIN完全相同的员工信息
-- SELECT * from emp WHERE job = (SELECT job FROM emp WHERE ename = 'MARTIN') and sal = (SELECT sal FROM emp WHERE ename = 'MARTIN');
-- select job,sal from emp where ename = "MARTIN";
select * from emp where (job,sal) in (select job,sal from emp where ename = "MARTIN");
-- 有两个以上直接下属的员工信息
SELECT mgr from emp GROUP BY mgr HAVING count(mgr) > 2;
SELECT * FROM emp WHERE empno in (SELECT mgr from emp GROUP BY mgr HAVING count(mgr) > 2);
-- 查询员工编号为7788的员工名称,员工工资,部门名称,部门地址
SELECT e.ename, e.sal, d.dname, d.loc FROM emp e join dept d on e.empno = 7788;

-- 1. 查询出高于本部门平均工资的员工信息
select avg(sal),deptno from emp ee group by deptno;
select * from emp e where sal > (select avg(sal) from emp ee group by deptno having e.deptno=ee.deptno);
-- 2. 列出达拉斯加工作的人中,比纽约平均工资高的人
SELECT * FROM emp WHERE 
			deptno = ( SELECT deptno FROM dept WHERE loc ='DALLAS' )
      AND 
      sal > (SELECT AVG(sal) FROM emp WHERE deptno = (SELECT deptno FROM dept WHERE loc ='NEW YORK'));

-- 3. 查询7369员工编号,姓名,经理编号和经理姓名
SELECT e.empno, e.ename, d.empno, d.ename from emp e, emp d WHERE e.empno = 7369 and e.mgr = d.empno; 
-- 4. 查询出各个部门薪水最高的员工所有信息
select * from emp where (sal, deptno) in (select max(sal), deptno from emp group by deptno);
```

