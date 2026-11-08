---
title: "Mysql的基本操作"
yuque_slug: "tnhnn8pgw1xzc6va"
yuque_id: 227482319
yuque_updated: "2026-10-14T04:54:03.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/tnhnn8pgw1xzc6va"
---
### 1. 数据定义语言 (DDL - Data Definition Language)

用于定义或修改数据库中的结构，如创建表、删除表等。

- **CREATE**: 创建数据库对象，例如数据库、表。

```sql
CREATE DATABASE example_db;
CREATE TABLE users (id INT PRIMARY KEY, name VARCHAR(255));
```

- **ALTER**: 修改现有的数据库对象。

```sql
ALTER TABLE users ADD COLUMN email VARCHAR(255);
```

- **DROP**: 删除数据库对象。

```sql
DROP TABLE users;
DROP DATABASE example_db;
```

- **TRUNCATE**: 清空表中的所有数据但保留表结构。

```sql
TRUNCATE TABLE users;
```

### 2. 数据操作语言 (DML - Data Manipulation Language)

用于对数据库中的数据执行插入、更新、删除等操作。

- **INSERT**: 向表中插入新记录。

```sql
INSERT INTO users (id, name) VALUES (1, 'John Doe');
```

- **SELECT**: 查询表中的数据。

```sql
SELECT * FROM users WHERE id = 1;
```

- **UPDATE**: 更新表中的现有记录。

```sql
UPDATE users SET name = 'Jane Doe' WHERE id = 1;
```

- **DELETE**: 从表中删除记录。

```sql
DELETE FROM users WHERE id = 1;
```

### 3. 数据控制语言 (DCL - Data Control Language)

用于控制数据库访问权限。

- **GRANT**: 赋予用户权限。

```sql
GRANT ALL PRIVILEGES ON example_db.* TO 'user'@'localhost';
```

- **REVOKE**: 撤销用户的权限。

```sql
REVOKE ALL PRIVILEGES ON example_db.* FROM 'user'@'localhost';
```

### 4. 事务控制语言 (TCL - Transaction Control Language)

用于管理数据库中的事务，确保数据的一致性和完整性。

- **COMMIT**: 提交当前事务的所有更改。

```sql
COMMIT;
```

- **ROLLBACK**: 撤销当前事务的所有更改。

```sql
ROLLBACK;
```

- **SAVEPOINT**: 在事务中设置保存点，允许部分回滚。

```sql
SAVEPOINT savepoint_name;
```

### 5. 数据查询语言 (DQL - Data Query Language)

虽然有时 DQL 被认为是 DML 的一部分，但它主要指的是 `SELECT` 语句，用于从数据库中检索数据。

```sql
SELECT column1, column2 FROM table_name WHERE condition;
```

# 常见语句及用法：

```plain
1.dml语句（增、删、改）

 -添加 insert 
       -insert into 表名（放入字段，中间用，隔开）values（添加信息）
       -insert into emp（EMPNO,ENAME,JOB,SAL,COMM,DEPTNO）values(7002,‘jack','SALESMAN',7902,'1980-12-17',400,200,10)   
 -删除 delete
     -删除编号为7002的员工
       -delete from 表名（）where 修改的字段;
       -delete from emp where empno=7002;

 -修改 update
     -将员工jack的薪水改为7000元，补贴改为500元。
     -修改多个字段中间用，隔开 
       -where是条件的关键词 
       -update emp set sal =7000,comn =500 where ename ='jack';//一家公司可能不止一个人叫这个名字，所以对主键进行修改
       -update emp set sal =7000,comn =500 where ename ='7002';//主键是表的唯一标识

2.dql(查询)
 -单表查询，查询所有记录（*表示所有）
       -select * from emp；
 -条件查询
   -查询薪水为5000的员工
       -select * from emp where sal =5000;
   -查询job为MANAGER的员工
       -select * from emp where job ='MANAGER';(记得加单引号或者双引号)
   -查询薪水不等于5000的员工
       -select * from emp where sal <> 5000;(<>不等于)
       -select * from emp where sal != 5000;
   -查询薪水在1600到3000的员工(第一种方法, >= 和<=)
       -select * from emp where sal >=1600 and sal <= 3000;
 -区间查询 between and
       -select * from emp where sal between 1600 and3 000;
   -查询补贴为(不为)空的员工
       -select * from emp where comn is (not) null;
   -查询工作岗位为MANAGER,薪水大于2500的员工
       -select * from emp where job='MANAGER' and sal > 2500;
   -查询出job为manager或者job为salesman的员工
       -select * from emp where job='MANAGER' or job='SALESMAN';

   -查询薪水大于1800，并且部门代码为20或30的员工（错误的写法）
       -select * from emp where sal > 1800 and deptno = 20 or deptno = 30;
   -查询薪水大于1800，并且部门代码为20或30的（正确的写法）
       -select * from emp where sal > 1800 and (deptno = 20 or deptno = 30);

   -查询出job为manager或者job为salesman的员工
       -select * from emp where job in ('manager','salesman');
   -查询出薪水包含1600和薪水包含3000的员工
       -select * from emp where sal in(1600, 3000);
   -查询出薪水不包含1600和薪水不包含3000的员工（第一种写法）
       -select * from emp where sal <> 1600 and sal <> 3000;
   -查询出薪水不包含1600和薪水不包含3000的员工（第二种写法）
       -select * from emp where not (sal = 1600 or sal = 3000);
   -查询出薪水不包含1600和薪水不包含3000的员工（第三种写法）
       -select * from emp where sal not in (1600, 3000);
   -查询出津贴不为null的所有员工
       -select * from emp where comm is not  null;

-模糊查询
-Like可以实现模糊查询，Like支持%和下划线匹配（%是模糊的占位符）

   -查询姓名以M开头所有的员工
       -select * from emp where ename like 'M%';
   -查询姓名以N结尾的所有的员工
       -select * from emp where ename like '%N';
   -查询姓名中包含O的所有的员工
       -select * from emp where ename like '%O%';
   -查询姓名中第二个字符为A的所有员工(下划线表示一个字符，可以作为占位符)
       -select * from emp where ename like '_A%';
-Like中%和下划线的差别？
%匹配任意字符出现的个数
下划线只匹配一个字符
Like 中的表达式必须放到单引号中|双引号中，以下写法是错误的：
select * from emp where ename like _A%

-分页查询
-limit
-select * from emp limit 起始索引位置，分页单位  //分页单位（多少条数据为一页）

   -将前三个数据分一页
       -select * from emp limit 0,3;  
   -将前四五六数据分一页
       -select * from emp limit 3,3;  
   -将前七八九数据分一页
       -select * from emp limit 6,3;
   -起始索引位置=（当前页-1）*分页单位

   -查询第六个到第十个员工的详细信息
       -select * from emp limit 5,5;

-排序
   -按照薪水从小到大排序
      -select * from emp order by sal;
   -取得job为MANAGER的员工，按照薪水由小到大排序               
        -select * from emp where job='MANAGER'order by sal;
      -手动指定按照薪水由大到小排序
        -select * from emp order by sal desc;
-聚合函数
      -取得所有员工数
        -select count (*) from emp;
        -select count (*) (as) '总个数' from emp;//as可省
      -取得津贴不为null的员工数
        -select count(comm) from emp //count自动过滤空值
      -取得工作岗位的个数 //去重关键字distinct
        -select count(distinct job) '岗位的总数' from emp;
      -取得薪水的总和
        -select sum(sal) from emp;
        -select sum(sal)  '薪水总和' from emp;
      -取得津贴的总和
        -select sum(comm) from emp;
        -select sum(comm)  '津贴总和' from emp;
      -取得薪水的合计（sal+comm）//需要把为空的字段替换成0，参与运算 
           //IFNULL方法将为空的字段（此处为comm 津贴），转换成0
        -select sum(sal+IFNULL(comm, 0)) '薪水总和' from emp;
      -平均薪水
        -select avg(sal) '平均薪水' from emp;    
      -取得最高薪水
        -select max(sal) '最高薪水' from emp;
      -取得最低薪水
        -select min(sal) '最低薪水' from emp;
      -组合聚合函数
        -select count(*),sum(sal),max(sal),min(sal),avg(sal)  avg from emp;
-分组查询
      -取的每个工作岗位的工资合计，要求显示岗位名称和薪资合计//参与分组的字段一点要写在select的后面
        -select job '岗位',sum(sal)'薪水总和' from emp group by job;
        -select job '岗位',sum(sal) salSum  from emp group by job order by salSum desc; 
      -取得每个岗位的平均工资大于2000的合集
        -select job, avg(sal) from emp salAvg  group by job having avg(sal) >2000;
        -select job, avg(sal) from emp salAvg  group by job having salAvg >2000;
-多表查询 sql92
   -通过外键来建立表与表中间的关系
   -一定要加条件where 如果不加条件 会出现笛卡尔积现象
      -显示每个员工的信息，并显示所属部门的名称  内连接
        -select emp.ename,dept.dname from emp,dept where emp.deptno = dept.deptno;
        -select emp.ename,dept.dname from emp e,dept d where emp.deptno = dept.deptno;
        -select emp.ename '员工姓名',dept.dname '部门姓名' from emp e,dept d where emp.deptno = dept.deptno;
      -取得员工和所属领导的姓名 自连接（表自己连接自己）
         -select e.ename '员工姓名', m.ename '领导' from emp e, emp m where e.mgr=m.empno;
      -显示薪水大于2000的员工信息，并显示所属部门的名称
      -内连接
         采用SQL92语法：
         -select e.ename '员工信息' , e.sal '薪水' , d.dname '部门名称' from emp e, dept d where e.deptno=d.deptno and  e.sal > 2000;
         采用SQL99语法：
         -select e.ename '员工信息' , e.sal '薪水', d.dname '部门名称'  from emp e join dept d on e.deptno=d.deptno where e.sal>2000;
         或
         -select e.ename '员工信息', e.sal '薪水', d.dname '部门名称' from emp e inner join dept d on e.deptno=d.deptno where e.sal>2000;
在实际中一般不加inner关键字（inner可省）
       -外连接
        -主表 和 从表
        -左外连接
          -通过 left join 实现，会将左边的表看成主表，主表的数据将会全部显示，即使是null，也会正常显示
        -右外连接
          -通过 right join 实现，会将右边的表看成主表，主表的数据将会全部显示，即使是null，也会正常显示
     -显示员工信息，并显示所属的部门名称，如果某一个部门没有员工，那么该部门也必须显示出来
        -左连接 
         -select e.ename, d.dname,d.deptno from dept d left join emp e on e.deptno=d.deptno;
        -右连接 
         -select e.ename, d.dname,d.deptno from dept d roght join emp e on e.deptno=d.deptno;
     -显示所有的员工信息、部门信息、以及每个员工所对应的薪水的薪水等级
         -select e.name '员工姓名',e.sal'员工薪水',d.dname'部门名称',s.grade'薪水等级' from emp e left join dept d on e.depyno =d.depyno left join salgrade s om e.sal between s.LOSAL and s.HISAL;
-子查询     
     -查询员工信息，查询哪些人是管理者，要求显示出其员工编号和员工姓名
 实现思路:
      1、首先取得管理者的编号，去除重复的
         -select distinct mgr from emp where mgr is not null;(distinct 去除重复行)
      2、查询员工编号包含管理者编号的
         -select empno, ename from emp where empno in(select mgr from emp where mgr is not null);
     -查询哪些人的薪水高于员工的平均薪水，需要显示员工编号，员工姓名，薪水
  实现思路:
      1、取得平均薪水
         -select avg(sal) from emp;
      2、取得大于平均薪水的员工
         -select empno, ename, sal from emp where sal > (select avg(sal) from emp);
     -查询每个部门的平均薪水的薪水等级，需要显示部门名称，平均薪水，平均薪水等级
       -查询各个部门的薪水
         -select d.dname,avg(e.sal) from  emp e  left join dept d on e.deptno = d.deptno group by d.dname = X(给前面那一串制造出来的表看成一个X表)
         -select x.dname '部门名称'，, x.avgSal '平均薪水',s.grade '薪水等级' from (select d.dname dname,avg(e.sal) avgSal （//给一个别名） from  emp e  left join dept d on e.deptno = d.deptno group by d.dname） X （//给一个别名）  left join salgrade s on x.avgSal between s.LOSAL and s.HISAL;
     -查询员工信息，并显示出员工所属的部门名称
       -第一种做法，将员工表和部门表连接
         -select e.ename, d.dname from emp e, dept d where e.deptno=d.deptno;
       -第二种做法，在select语句中再次嵌套select语句完成部分名称的查询
         -select e.ename, (select d.dname from dept d where e.deptno=d.deptno) as dname from emp e;
```

# 里面的表格：

```sql
+---------------------+
| Tables_in_database2 |
+---------------------+
| dept                |
| emp                 |
| salgrade            |
+---------------------+
```

```sql
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
```

```sql
    +--------+------------+----------+
    | DEPTNO | DNAME      | LOC      |
    +--------+------------+----------+
    |     10 | ACCOUNTING | NEW YORK |
    |     20 | RESEARCH   | DALLAS   |
    |     30 | SALES      | CHICAGO  |
    |     40 | OPERATIONS | BOSTON   |
    +--------+------------+----------+   
```

```sql
+-------+-------+-------+
| GRADE | LOSAL | HISAL |
+-------+-------+-------+
|     1 |   700 |  1200 |
|     2 |  1201 |  1400 |
|     3 |  1401 |  2000 |
|     4 |  2001 |  3000 |
|     5 |  3001 |  9999 |
+-------+-------+-------+
```
