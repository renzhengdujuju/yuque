---
title: "Mysql总结"
yuque_slug: "qx45ez4ud7px47c7"
yuque_id: 227482318
yuque_updated: "2026-10-14T04:54:03.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/qx45ez4ud7px47c7"
---
|  |  |  |
| --- | --- | --- |
| **名称** | **解释** | **命令** |
| DDL |  |  |
| DML |  |  |
| DQL |  |  |
| DCL |  |  |

## 1、表结构描述
表名称：dept

描述：部门信息表

|  |  |  |
| --- | --- | --- |
| 英文字段名称 | 中文描述 | 类型 |
| DEPTNO(id) | 部门编号 | INT(2)(括号里面数字表示给他的字节长度) |
| DNAME | 部门名称 | VARCHAR(14) |
| LOC | 位置 | VARCHAR(13) |

表名称：emp

描述：员工信息表

|  |  |  |
| --- | --- | --- |
| 英文字段名称 | 中文描述 | 类型 |
| EMPNO(id) | 员工编号 | INT (4) |
| ENAME | 员工姓名 | VARCHAR(10) |
| JOB | 工作岗位 | VARCHAR(9) |
| MGR | 上级领导 | INT (4) |
| HIREDATE | 入职日期 | DATE |
| SAL | 薪水 | DOUBLE(7,2) |
| COMM | 津贴 | DOUBLE (7,2) |
| DEPTNO | 部门编号 | INT(2) |

注：DEPTNO字段是外键，DEPTNO的值来源于dept表的主键，起到了约束的作用

表名称：salgrade

描述：薪水等级信息表

|  |  |  |
| --- | --- | --- |
| 英文字段名称 | 中文描述 | 类型 |
| GRADE | 等级 | INT |
| LOSAL | 最低薪水 | INT |
| HISAL | 最高薪水 | INT |

## 2****、常用命令
### 2.1****、查看****msyql****版本
• MySQL程序选项具有以下两种通用形式：

– 长选项，由单词之前加两个减号组成

– 短选项，由单个字母之前加一个减号组成

C:\Users\Administrator>mysql --version

mysql Ver 14.14 Distrib 5.5.36, for Win32 (x86)

C:\Users\Administrator>mysql -V

mysql Ver 14.14 Distrib 5.5.36, for Win32 (x86)

### 2.2****、创建数据库
1. create database 数据库名称;

create database testdatabase;

2. use 数据库名称

use testdatabase;

在数据库中建立表，因此创建表的时候必须要先选择数据库。

### 2.3****、查询当前使用的数据库
select database();

查询数据库版本也可以使用

select  version();

### 2.4****、终止一条语句
如果想要终止一条正在编写的语句，可键入\c。

### 2.5****、退出****mysql
可使用\q、QUIT或EXIT：

如：

mysql> \q (ctrl+c)

### 3.6****、查看表的结构
desc <table name>;

如：

![](../_assets/b00903b4_1752206102823-4b661d82-3492-4602-b2e1-0aab4369da8b.png)

### 3.7****、查看表的创建语句
show create table <table name>;

如：

![](../_assets/fa601262_1752206141676-d80657ef-1890-4a96-9094-c9e03a9106a8.png)

![](../_assets/d3b1c720_1752206147324-172a69c8-1fed-4414-9edf-2e76d94da34f.png)

## 4****、简单的查询
### 4.1****、查询一个字段
l 查询员工姓名

|  |
| --- |
| select ename from emp; |
|  |

Select语句后面跟的是字段名称，select是关键字，select和字段名称之间采用空格隔开，from表示将要查询的表，它和字段之间采用空格隔开

### 4.2****、查询多个字段
l 查询员工的编号和姓名

|  |
| --- |
| select empno, ename from emp; |

![](../_assets/af803361_1752206214656-8362a51f-97b0-44c7-9d1d-8a69909b45c2.png)

查询多个字段，select中的字段采用逗号间隔即可。

### 4.3****、查询全部字段
可以将所有的字段放到select语句的后面，这种方案不方便，但是比较清楚，我们可以采用如下便捷的方式查询全部字段

|  |
| --- |
| select \* from emp; |

![](../_assets/23bf51fb_1752206226212-a40bb84f-421c-4058-be3d-63ba85818a24.png)

采用select \* from emp，虽然简单，但是\*号不是很明确，建议查询全部字段将相关字段写到select语句的后面,在以后java连接数据库的时候,是需要在java程序中编写SQL语句的,这个时候编写的SQL语句不建议使用select \* 这种形式,建议写明字段,这样可读性强.

### 4.4****、计算员工的年薪
l 列出员工的编号，姓名和年薪

|  |
| --- |
| select empno, ename, sal\*12 from emp; |

![](../_assets/0a1cb20a_1752206235063-abe21723-1eb8-43b3-906a-487e8d29a483.png)

在select语句中可以使用运算符，以上存在一些问题，年薪的字段名称不太明确

### 4.5****、将查询出来的字段显示为中文
|  |
| --- |
| select empno as ‘员工编号’, ename as ‘员工姓名’, sal\*12 as ‘年薪’ from emp;  注意:字符串必须添加单引号 | 双引号 |

![](../_assets/ddf2efc4_1752206267474-9bbb4ed7-19b2-47c8-a3f8-61b4bc94900c.png)

可以采用as关键字重命名表字段，其实as也可以省略，如：

|  |
| --- |
| select empno  "员工编号", ename  "员工姓名", sal\*12  "年薪" from emp; |

## 5****、条件查询
条件查询需要用到where语句，where必须放到from语句表的后面

支持如下运算符

|  |  |
| --- | --- |
| 运算符 | 说明 |
| = | 等于 |
| <>或!= | 不等于 |
| < | 小于 |
| <= | 小于等于 |
| > | 大于 |
| >= | 大于等于 |
| between … and …. | 两个值之间,**等同于****>= and <=** |
| is null | 为null（is not null 不为空） |
| **and** | 并且 |
| **or** | 或者 |
| in | 包含，相当于多个or（not in不在这个范围中） |
| not | not可以取非，主要用在is 或in中 |
| like | like称为模糊查询，支持%或下划线匹配  %匹配任意个字符  下划线，一个下划线只匹配一个字符 |

### 5.1****、等号操作符
l 查询薪水为5000的员工

|  |
| --- |
| select empno, ename, sal from emp where sal=5000; |

![](../_assets/2682b3b6_1752206288670-b274e282-1634-42a9-8ba2-a0e1af576180.png)

l 查询job为MANAGER的员工

|  |
| --- |
| select empno, ename from emp where job=manager; |

![](../_assets/6534dd13_1752206294365-dcfb7815-3bf6-4f98-94a4-51df41fd7af8.png)

以上查询出现错误，因为job为字符串，所以出现了以上错误

|  |
| --- |
| select empno, ename from emp where job="manager"; |

![](../_assets/0fe6ce3f_1752206300647-9a66441c-a6f6-46a1-a1f9-f8fbbdcfd942.png)

|  |
| --- |
| select empno, ename from emp where job=’manager’; |

![](../_assets/9c46c0b7_1752206307652-aff2977c-78a5-4da9-b275-7127b3574b38.png)

也可以使用单引号

|  |
| --- |
| select empno, ename from emp where job='MANAGER'; |

![](../_assets/19c0584f_1752206317140-abc85c0c-eb7c-4ef3-a601-f3a448d94d00.png)

以上输出正确，Mysql默认情况下大小写是不敏感的。

注意：

MySQL在windows下是不区分大小写的，将script文件导入MySQL后表名也会自动转化为小写，结果再 想要将数据库导出放到linux服务器中使用时就出错了。因为在linux下表名区分大小写而找不到表，查了很多都是说在linux下更改MySQL的设置使其也不区分大小写，但是有没有办法反过来让windows 下大小写敏感呢。其实方法是一样的，相应的更改windows中MySQL的设置就行了。

具体操作：

在MySQL的配置文件my.ini中增加一行：

lower\_case\_table\_names = 0

其中 0：区分大小写，1：不区分大小写

MySQL在Linux下数据库名、表名、列名、别名大小写规则是这样的：

1、数据库名与表名是严格区分大小写的；

2、表的别名是严格区分大小写的；

3、列名与列的别名在所有的情况下均是忽略大小写的；

4、变量名也是严格区分大小写的； MySQL在Windows下都不区分大小写

### 5.2****、****<>****操作符
l 查询薪水不等于5000的员工

|  |
| --- |
| select empno, ename, sal from emp where sal <> 5000; |

![](../_assets/c775e34a_1752206334341-d93a62e6-a67a-405e-9c99-0438978d418d.png)

一下写法等同于以上写法，建议使用第一种写法

|  |
| --- |
| select empno, ename, sal from emp where sal != 5000; |

数值也可以采用单引号引起来，如一下语句是正确的

|  |
| --- |
| select empno, ename, sal from emp where sal <> '5000'; |

l 查询工作岗位不等于MANAGER的员工

|  |
| --- |
| select empno, ename from emp where job <> 'MANAGER'; |

### 5.3****、****between** **…****and** **…****操作符
l 查询薪水为1600到3000的员工(第一种方式，采用>=和<=)

|  |
| --- |
| select empno, ename, sal from emp where sal >= 1600 and sal <= 3000; |

![](../_assets/365dbaeb_1752206347888-d8d3049c-dbe9-4aee-8282-a4e2d9878131.png)

l 查询薪水为1600到3000的员工(第一种方式，采用between … and …)

|  |
| --- |
| select empno, ename, sal from emp where sal between 1600 and 3000; |

![](../_assets/7d349bda_1752206356777-5bb074b8-4980-4b71-a789-76801073b606.png)

关于between … and …，它是包含最大值和最小值的

### 5.4****、****is null
l Null为空，但不是空串，为null可以设置这个字段不填值，如果查询为null的字段，采用is null

l 查询津贴为空的员工

|  |
| --- |
| select \* from emp where comm=null; |

![](../_assets/30692d18_1752206369751-62744c7a-1f5e-4b64-a0d7-1f80f1e33963.png)

以上也无法查询出符合条件的数据，因为null类型比较特殊，必须使用 is来比较

|  |
| --- |
| select \* from emp where comm is null; |

![](../_assets/4ec6bc6b_1752206375937-3e6ff97e-3e91-41fe-b7cd-bacde9e8fee1.png)

以上查询正确

### 5.5****、** **and
and表示并且的含义，表示所有的条件必须满足

l 工作岗位为MANAGER,薪水大于2500的员工

|  |
| --- |
| select \* from emp where job='MANAGER' and sal > 2500; |

![](../_assets/7772f9f8_1752206383223-b149c1b1-bd93-4fc4-aab1-0a4bac4bc4da.png)

### 5.6****、****or
or，只要满足条件即可,相当于包含

l 查询出job为manager或者job为salesman的员工

|  |
| --- |
| select \* from emp where job='MANAGER' or job='SALESMAN'; |

![](../_assets/724d474f_1752206412036-d453b4e6-ed11-4e26-be5e-67605d3809ab.png)

### 5.7****、表达式的优先级
l 查询薪水大于1800，并且部门代码为20或30的员工（错误的写法）

|  |
| --- |
| select \* from emp where sal > 1800 and deptno = 20 or deptno = 30; |

![](../_assets/97295864_1752206420669-a271d179-49c2-494c-85ee-34628250df0f.png)

以上输出不是预期结果，薪水小于1800的数据也被查询上来了，原因是表达式的优先级导致的，首先过滤sal > 1800 and deptno = 20，然后再将deptno = 30员工合并过来，所以是不对的

l 查询薪水大于1800，并且部门代码为20或30的（正确的写法）

|  |
| --- |
| select \* from emp where sal > 1800 and (deptno = 20 or deptno = 30); |

![](../_assets/677a6386_1752206427660-218d6f3a-413c-48d7-8013-2239ee4cc84c.png)

关于运算符的问题：不用记，没有把握尽量采用括号

### 5.8****、****in
in表示包含的意思，完全可以采用or来表示，采用in会更简洁一些

l 查询出job为manager或者job为salesman的员工

|  |
| --- |
| select \* from emp where job in ('manager','salesman'); |

![](../_assets/c0c7230f_1752206434752-a686b04c-c1fb-4c81-9fe8-aa31b01598d8.png)

l 查询出薪水包含1600和薪水包含3000的员工

|  |
| --- |
| select \* from emp where sal in(1600, 3000); |

![](../_assets/c877bc13_1752206440271-6d315a51-7d98-4534-9375-bfe741ca4f7a.png)

### 5.9****、****not
l 查询出薪水不包含1600和薪水不包含3000的员工（第一种写法）

|  |
| --- |
| select \* from emp where sal <> 1600 and sal <> 3000; |

![](../_assets/f84ef4a6_1752206447059-7278f92b-3a58-40dc-8044-dba80efed0d0.png)

l 查询出薪水不包含1600和薪水不包含3000的员工（第二种写法

|  |
| --- |
| select \* from emp where not (sal = 1600 or sal = 3000); |

![](../_assets/adf99768_1752206452652-0999d58a-3307-4d33-a8db-1f6bf2d649e1.png)

l 查询出薪水不包含1600和薪水不包含3000的员工（第三种写法）

|  |
| --- |
| select \* from emp where sal not in (1600, 3000); |

![](../_assets/df8e71e2_1752206458250-2b600913-8052-4555-ba9a-fc09fe6a9ce6.png)

l 查询出津贴不为null的所有员工

|  |
| --- |
| select \* from emp where comm is not null; |

![](../_assets/a1ecf05d_1752206465373-f4651284-2456-4a03-adfd-73fee420baba.png)

### 5.10****、****like
l Like可以实现模糊查询，like支持%和下划线匹配

l 查询姓名以M开头所有的员工

|  |
| --- |
| select \* from emp where ename like 'M%'; |

![](../_assets/86472815_1752206471514-bc662257-43d6-43fa-b2a4-170bb87c77d0.png)

l 查询姓名以N结尾的所有的员工

|  |
| --- |
| select \* from emp where ename like '%N'; |

![](../_assets/3b7bcac0_1752206476395-b2a4ebc6-f91e-4f82-8740-5a85ee3c4503.png)

l 查询姓名中包含O的所有的员工

|  |
| --- |
| select \* from emp where ename like '%O%'; |

![](../_assets/6d7fe0e1_1752206481970-657a253d-15b6-41dc-a698-fd4871a6b868.png)

l 查询姓名中第二个字符为A的所有员工

|  |
| --- |
| select \* from emp where ename like '\_A%'; |

![](../_assets/35e4b6ea_1752206488609-a9212c0a-e82a-4e96-aaa6-8f657ebae4f6.png)

Like中%和下划线的差别？

%匹配任意字符出现的个数

下划线只匹配一个字符

Like 中的表达式必须放到单引号中|双引号中，以下写法是错误的：

|  |
| --- |
| select \* from emp where ename like \_A% |

## 6****、排序数据
### 6.1****、单一字段排序
排序采用order by子句，order by后面跟上排序字段，排序字段可以放多个，多个采用逗号间隔，order by默认采用升序，如果存在where子句那么order by必须放到where语句的后面

l 按照薪水由小到大排序(系统默认由小到大)

|  |
| --- |
| select \* from emp order by sal; |

![](../_assets/36f4105a_1752206497369-020cb658-1111-4c71-a4f0-10f96aaed14a.png)

l 取得job为MANAGER的员工，按照薪水由小到大排序(系统默认由小到大)

|  |
| --- |
| select \* from emp where job='MANAGER' order by sal; |

![](../_assets/6caa39f2_1752206504122-13279b02-2643-46cf-845f-4c1795d1ed89.png)

如果包含where语句order by必须放到where后面，如果没有where语句order by放到表的后面

以下写法是错误的：

|  |
| --- |
| select \* from emp order by sal where job='MANAGER'; |

l 按照多个字段排序，如：首先按照job排序，再按照sal排序

|  |
| --- |
| select \* from emp order by job,sal; |

![](../_assets/d15063ff_1752206603213-338cf8eb-5fed-4eff-99f1-c3e5e3a2301a.png)

### 6.2****、手动指定排序顺序
l 手动指定按照薪水由小到大排序(默认情况)

|  |
| --- |
| select \* from emp order by sal asc; |

![](../_assets/fd6e4504_1752206871873-d054d72c-a142-461d-b180-5e02df4b60e6.png)

l 手动指定按照薪水由大到小排序

|  |
| --- |
| select \* from emp order by sal desc; |

![](../_assets/0fa06072_1752206877806-c4fd6fee-a5a8-405c-bfdc-acc6aed3cb21.png)

### 6.3****、多个字段排序
l 按照job和薪水倒序

|  |
| --- |
| select \* from emp order by job desc, sal desc; |

![](../_assets/4e911fb4_1752206883795-b710701c-b494-4cea-acf3-2aec8ff0a2cb.png)

如果采用多个字段排序，如果根据第一个字段排序重复了，会根据第二个字段排序

### 6.4****、使用字段的位置来排序
l 按照薪水升序

|  |
| --- |
| select \* from emp order by 6; |

![](../_assets/ce5b17c2_1752206889980-783ae1c0-8e1f-4ab3-b459-a7995e961af6.png)

不建议使用此种方式，采用数字含义不明确，程序不健壮

## 7****、分组函数****/****聚合函数****/****多行处理函数
|  |  |
| --- | --- |
| count | 取得记录数 |
| sum | 求和 |
| avg | 取平均 |
| max | 取最大的数 |
| min | 取最小的数 |

**注意：分组函数自动忽略空值，不需要手动的加****where****条件排除空值。**

**select count(\*) from emp****where xxx****;****符合条件的所有记录总数。**

**select count(comm) from emp;****comm****这个字段中不为空的元素总数。**

注意：分组函数不能直接使用在where关键字后面。

mysql> select ename,sal from emp where sal > avg(sal);

ERROR 1111 (HY000): Invalid use of group function

### 7.1****、****count
l 取得所有的员工数

|  |
| --- |
| select count(\*) from emp; |

![](../_assets/bad5bea7_1752206897881-8c2fae96-330f-427a-a0ce-3fdb55c9f98f.png)

Count(\*)表示取得所有记录，忽略null，为null的值也会取得

l 取得津贴不为null员工数

|  |
| --- |
| select count(comm) from emp; |

![](../_assets/b4908928_1752206905421-04b8dced-f26f-48e2-adc1-e550bf9415a0.png)

采用count(字段名称)，不会取得为null的记录

l 取得工作岗位的个数

|  |
| --- |
| select count(distinct job ) from emp; |

![](../_assets/08911542_1752206913212-c845590f-8f63-4ca3-9161-bdf2b3f077d3.png)

### 7.2****、****sum
l Sum可以取得某一个列的和，null会被忽略

l 取得薪水的合计

|  |
| --- |
| select sum(sal) from emp; |

![](../_assets/f65ab5bb_1752206920071-f0a57675-ac63-40c8-b27e-3f6317c89587.png)

l 取得津贴的合计

|  |
| --- |
| select sum(comm) from emp; |

![](../_assets/284d0ae0_1752206926031-4e22c172-f0cc-4fff-b5cb-f44e57b96e02.png)

null会被忽略

l 取得薪水的合计（sal+comm）

|  |
| --- |
| select sum(sal+comm) from emp; |

![](../_assets/c5dd7aea_1752206930243-e464f0a5-dab7-48fe-b6a7-3588ef90e886.png)

从以上结果来看，不正确，原因在于comm字段有null值，所以无法计算，sum会忽略掉，正确的做法是将comm字段转换成0

|  |
| --- |
| select sum(sal+IFNULL(comm, 0)) from emp; |

![](../_assets/56dc4741_1752206936112-7963be2e-4c46-4fb7-adbe-4c0cbcf5b583.png)

### 7.3****、****avg
取得某一列的平均值

l 取得平均薪水

|  |
| --- |
| select avg(sal) from emp; |

![](../_assets/112976d5_1752206943050-25ad3ef3-e3b7-4d10-98a8-d98aea9128cc.png)

### 7.4****、****max
取得某个一列的最大值

l 取得最高薪水

|  |
| --- |
| select max(sal) from emp; |

![](../_assets/45ba41c2_1752206947801-b1ba0e2d-81c2-4e91-a7de-53a068770509.png)

l 取得最晚入职得员工

|  |
| --- |
| select max(str\_to\_date (hiredate, '%Y-%m-%d')) from emp; |

![](../_assets/e2994213_1752206953334-eb650863-74a0-4619-8bd6-969967b5921d.png)

### 7.5****、****min
取得某个一列的最小值

l 取得最低薪水

|  |
| --- |
| select min(sal) from emp; |

![](../_assets/9795e5dd_1752206959328-0dc2206a-f556-4bce-8841-b28e65f488c5.png)

l 取得最早入职得员工（可以不使用str\_to\_date转换）

|  |
| --- |
| select min(str\_to\_date(hiredate, '%Y-%m-%d')) from emp; |

![](../_assets/8d4688f0_1752206963984-534bd29e-ec8b-4b01-9284-7e69a61911a8.png)

### 7.6****、组合聚合函数
可以将这些聚合函数都放到select中一起使用

|  |
| --- |
| select count(\*),sum(sal),avg(sal),max(sal),min(sal) from emp; |

![](../_assets/30a02945_1752206969264-7356da67-5ffd-49ed-85f2-338cd27fa3cd.png)

## 8****、分组查询
分组查询主要涉及到两个子句，分别是：group by和having

### 8.1****、****group by
l 取得每个工作岗位的工资合计，要求显示岗位名称和工资合计

|  |
| --- |
| select job, sum(sal) from emp group by job; |

![](../_assets/96406ae6_1752206975028-01fc2a51-c16d-433b-88df-f0899fb3b217.png)

如果使用了order by，order by必须放到group by后面

![](../_assets/6be21282_1752206981534-d286f5ed-04ea-411a-9689-1083a750f12b.png)

l 按照工作岗位和部门编码分组，取得的工资合计(**题目表意不明)**

n 原始数据

![](../_assets/d2b418a7_1752206987549-4af14f2b-cad8-44b8-a4a0-7553d829881d.png)

n 分组语句

|  |
| --- |
| select job,deptno,sum(sal) from emp group by job,deptno; |

![](../_assets/64ea2417_1752206994615-ebb1145c-b3e3-4207-8a70-981c0fec5bb4.png)

![](../_assets/c90936de_1752207013796-10d6f71f-504a-499c-bb5a-55552fffa107.png)

mysql> select empno,deptno,avg(sal) from emp group by deptno;

+-------+--------+-------------+

| empno | deptno | avg(sal) |

+-------+--------+-------------+

| 7782 | 10 | 2916.666667 |

| 7369 | 20 | 2175.000000 |

| 7499 | 30 | 1566.666667 |

+-------+--------+-------------+

以上SQL语句在Oracle数据库中无法执行，执行报错。

以上SQL语句在Mysql数据库中可以执行，但是执行结果矛盾。

在SQL语句中若有group by 语句，那么在select语句后面只能跟**分组函数****+****参与分组的字段**。

### 8.2****、****having
如果想对分组数据再进行过滤需要使用having子句

取得每个岗位的平均工资大于2000

|  |
| --- |
| select job, avg(sal) from emp group by job having avg(sal) >2000; |

![](../_assets/bc08d165_1752207028721-746ea072-263e-455e-b9c2-35dc91dc3a72.png)

分组函数的执行顺序：

根据条件查询数据

分组

采用having过滤，取得正确的数据

### 8.3****、****select****语句总结
一个完整的select语句格式如下

|  |
| --- |
| select 字段  from 表名  where …….  group by ……..  having …….(就是为了过滤分组后的数据而存在的—不可以单独的出现)  order by …….. |

以上语句的执行顺序

1. 首先执行where语句过滤原始数据

2. 执行group by进行分组

3. 执行having对分组数据进行操作

4. 执行select选出数据

5. 执行order by排序

原则：能在where中过滤的数据，尽量在where中过滤，效率较高。having的过滤是专门对分组之后的数据进行过滤的。

## 9****、连接查询
### 9.1****、****SQL92****语法
连接查询：也可以叫跨表查询，需要关联多个表进行查询

l 显示每个员工信息，并显示所属的部门名称

|  |
| --- |
| select ename, dname from emp, dept; |
| SQL> select ename, dname from emp, dept;    ENAME  DNAME  ---------- --------------  SMITH  ACCOUNTING  ALLEN  ACCOUNTING  WARD  ACCOUNTING  JONES  ACCOUNTING  MARTIN  ACCOUNTING  BLAKE  ACCOUNTING  CLARK  ACCOUNTING  SCOTT  ACCOUNTING  KING  ACCOUNTING  TURNER  ACCOUNTING  ADAMS  ACCOUNTING  JAMES  ACCOUNTING  FORD  ACCOUNTING  MILLER  ACCOUNTING  SMITH  RESEARCH  ALLEN  RESEARCH  WARD  RESEARCH  JONES  RESEARCH  MARTIN  RESEARCH  BLAKE  RESEARCH  CLARK  RESEARCH  SCOTT  RESEARCH  KING  RESEARCH  TURNER  RESEARCH  ADAMS  RESEARCH  JAMES  RESEARCH  FORD  RESEARCH  MILLER  RESEARCH  SMITH  SALES  ALLEN  SALES  WARD  SALES  JONES  SALES  MARTIN  SALES  BLAKE  SALES  CLARK  SALES  SCOTT  SALES  KING  SALES  TURNER  SALES  ADAMS  SALES  JAMES  SALES  FORD  SALES  MILLER  SALES  SMITH  OPERATIONS  ALLEN  OPERATIONS  WARD  OPERATIONS  JONES  OPERATIONS  MARTIN  OPERATIONS  BLAKE  OPERATIONS  CLARK  OPERATIONS  SCOTT  OPERATIONS  KING  OPERATIONS  TURNER  OPERATIONS  ADAMS  OPERATIONS  JAMES  OPERATIONS  FORD  OPERATIONS  MILLER  OPERATIONS    已选择56行。 |

以上输出，不正确，输出了56条数据，其实就是两个表记录的成绩，这种情况我们称为：“笛卡儿乘积”，出现错误的原因是：没有指定连接条件

指定连接条件

|  |
| --- |
| select emp.ename, dept.dname from emp, dept where emp.deptno=dept.deptno;  也可以使用别名  select e.ename , d.dname from emp e, dept d where e.deptno=d.deptno; |

![](../_assets/5f6193e3_1752207048112-c444a9f6-07ae-4db5-9d7b-c738b5dadb57.png)

以上结果输出正确，因为加入了正确的连接条件

以上查询也称为“内连接”，只查询相等的数据（连接条件相等的数据）

l 取得员工和所属的领导的姓名

|  |
| --- |
| select e.ename '员工姓名', m.ename '领导' from emp e, emp m where e.mgr=m.empno; |
| SQL> select \* from emp;（普通员工）    EMPNO ENAME  JOB  MGR HIREDATE  SAL  COMM  DEPTNO  ---------- ---------- --------- ---------- -------------- ---------- ---------- ----------  7369 SMITH  CLERK  7902 17-12月-80  800  20  7499 ALLEN  SALESMAN  7698 20-2月 -81  1600  300  30  7521 WARD  SALESMAN  7698 22-2月 -81  1250  500  30  7566 JONES  MANAGER  7839 02-4月 -81  2975  20  7654 MARTIN  SALESMAN  7698 28-9月 -81  1250  1400  30  7698 BLAKE  MANAGER  7839 01-5月 -81  2850  30  7782 CLARK  MANAGER  7839 09-6月 -81  2450  10  7788 SCOTT  ANALYST  7566 19-4月 -87  3000  20  7839 KING  PRESIDENT  17-11月-81  5000  10  7844 TURNER  SALESMAN  7698 08-9月 -81  1500  0  30  7876 ADAMS  CLERK  7788 23-5月 -87  1100  20  7900 JAMES  CLERK  7698 03-12月-81  950  30  7902 FORD  ANALYST  7566 03-12月-81  3000  20  7934 MILLER  CLERK  7782 23-1月 -82  1300  10    已选择14行。    SQL> select \* from emp;（管理者）    EMPNO ENAME  JOB  MGR HIREDATE  SAL  COMM  DEPTNO  ---------- ---------- --------- ---------- -------------- ---------- ---------- ----------  7369 SMITH  CLERK  7902 17-12月-80  800  20  7499 ALLEN  SALESMAN  7698 20-2月 -81  1600  300  30  7521 WARD  SALESMAN  7698 22-2月 -81  1250  500  30  7566 JONES  MANAGER  7839 02-4月 -81  2975  20  7654 MARTIN  SALESMAN  7698 28-9月 -81  1250  1400  30  7698 BLAKE  MANAGER  7839 01-5月 -81  2850  30  7782 CLARK  MANAGER  7839 09-6月 -81  2450  10  7788 SCOTT  ANALYST  7566 19-4月 -87  3000  20  7839 KING  PRESIDENT  17-11月-81  5000  10  7844 TURNER  SALESMAN  7698 08-9月 -81  1500  0  30  7876 ADAMS  CLERK  7788 23-5月 -87  1100  20  7900 JAMES  CLERK  7698 03-12月-81  950  30  7902 FORD  ANALYST  7566 03-12月-81  3000  20  7934 MILLER  CLERK  7782 23-1月 -82  1300  10    已选择14行。    SQL> select e.ename, m.ename from emp e, emp m where e.mgr=m.empno;    ENAME  ENAME  ---------- ----------  SMITH  FORD  ALLEN  BLAKE  WARD  BLAKE  JONES  KING  MARTIN  BLAKE  BLAKE  KING  CLARK  KING  SCOTT  JONES  TURNER  BLAKE  ADAMS  SCOTT  JAMES  BLAKE  FORD  JONES  MILLER  CLARK    已选择13行。 |

以上称为“自连接”，只有一张表连接，具体的查询方法，把一张表看作两张表即可，如以上示例：第一个表emp e代码了员工表，emp m代表了领导表，相当于员工表和部门表一样

### 9.2****、****SQL99****语法
l （内连接）显示薪水大于2000的员工信息，并显示所属的部门名称

|  |
| --- |
| 采用SQL92语法：  select e.ename '员工信息' , e.sal '薪水' , d.dname '部门名称' from emp e, dept d where e.deptno=d.deptno and e.sal > 2000;  采用SQL99语法：  select e.ename '员工信息' , e.sal '薪水', d.dname '部门名称' from emp e join dept d on e.deptno=d.deptno where e.sal>2000;  或  select e.ename '员工信息', e.sal '薪水', d.dname '部门名称' from emp e inner join dept d on e.deptno=d.deptno where e.sal>2000;  在实际中一般不加inner关键字（inner可省） |

Sql92语法和sql99语法的区别：99语法可以做到表的连接和查询条件分离，特别是多个表进行连接的时候，会比sql92更清晰

l （外连接）显示员工信息，并显示所属的部门名称，如果某一个部门没有员工，那么该部门也必须显示出来

|  |
| --- |
| 右连接：  select e.ename, e.sal, d.dname from emp e right join dept d on e.deptno=d.deptno;  左连接：  select e.ename, e.sal, d.dname from dept d left join emp e on e.deptno=d.deptno;  以上两个查询效果相同 |

![](../_assets/a5f27f7d_1752207064831-b1feb94c-dd60-42b0-ba3c-ce0b56f8a0b5.png)

连接分类：

内链接

\* 表1 inner join 表2 on 关联条件

\* 做连接查询的时候一定要写上关联条件

\* inner 可以省略

外连接

\*左外连接

\* 表1 left outer join 表2 on 关联条件

\* 做连接查询的时候一定要写上关联条件

\* outer 可以省略\*右外连接

\* 表1 right outer join 表2 on 关联条件

\* 做连接查询的时候一定要写上关联条件

\* outer 可以省略

\*左外连接（左连接）和右外连接（右连接）的区别：

\*左连接以左面的表为准和右边的表比较，和左表相等的不相等都会显示出来，右表符合条件的显示,不符合条件的不显示

\*右连接恰恰相反，以上左连接和右连接也可以加入outer关键字，但一般不建议这种写法，如：

|  |
| --- |
| select e.ename, e.sal, d.dname from emp e right outer join dept d on e.deptno=d.deptno;  select e.ename, e.sal, d.dname from dept d left outer join emp e on e.deptno=d.deptno; |

左连接能完成的功能右连接一定可以完成

![](../_assets/3a1f33be_1752207074439-e1eca268-0ff7-43d3-a120-9fd467c02f27.png)

## 10****、子查询
子查询就是嵌套的select语句，可以理解为子查询是一张表

### 10.1****、在****where****语句中使用子查询，也就是在****where****语句中加入****select****语句
l 查询员工信息，查询哪些人是管理者，要求显示出其员工编号和员工姓名

实现思路：

l 1、首先取得管理者的编号，去除重复的

|  |
| --- |
| select distinct mgr from emp where mgr is not null;    distinct 去除重复行 |

l 2、查询员工编号包含管理者编号的

|  |
| --- |
| select empno, ename from emp where empno in(select mgr from emp where mgr is not null); |

![](../_assets/4263c09e_1752207149012-7a2d6830-cf61-4ae8-b6ab-e969d21bb969.png)

l 查询哪些人的薪水高于员工的平均薪水，需要显示员工编号，员工姓名，薪水

实现思路

1、 取得平均薪水

|  |
| --- |
| select avg(sal) from emp; |

2、 取得大于平均薪水的员工

|  |
| --- |
| select empno, ename, sal from emp where sal > (select avg(sal) from emp); |

![](../_assets/ffde3d78_1752207161672-8311f436-dfd1-432c-93d1-ecd44c215635.png)

### 10.2****、在****from****语句中使用子查询，可以将该子查询看做一张表
l 查询员工信息，查询哪些人是管理者，要求显示出其员工编号和员工姓名

首先取得管理者的编号，去除重复的

|  |
| --- |
| select distinct mgr from emp where mgr is not null; |

将以上查询作为一张表，放到from语句的后面

|  |
| --- |
| 使用92语法：  select e.empno, e.ename from emp e, (select distinct mgr from emp where mgr is not null) m where e.empno=m.mgr;  使用99语法：  select e.empno, e.ename from emp e join (select distinct mgr from emp where mgr is not null) m on e.empno=m.mgr; |

![](../_assets/10498f8a_1752207218989-8eaba685-8a16-4b09-8bce-ca53a3328c97.png)

l 查询各个部门的平均薪水所属等级，需要显示部门编号，平均薪水，等级编号

实现思路

1、首先取得各个部门的平均薪水

|  |
| --- |
| select deptno, avg(sal) avg\_sal from emp group by deptno; |

![](../_assets/dcf2f299_1752207224925-65f96176-5e7a-4fb7-be26-66f34bcb88dd.png)

2、将部门的平均薪水作为一张表与薪水等级表建立连接，取得等级

|  |
| --- |
| select deptno,avg(sal) avg\_sal from emp group by deptno; |
| select \* from salgrade; |
| select a.deptno,a.avg\_sal,g.grade from (select deptno,avg(sal) avg\_sal from emp group by deptno ) a join salgrade g on a.avg\_sal between g.losal and hisal; |

![](../_assets/cd925b30_1752207232992-69660193-b862-4cc3-9792-1a71bd258583.png)

### 10.3****、在****select****语句中使用子查询
l 查询员工信息，并显示出员工所属的部门名称

第一种做法，将员工表和部门表连接

|  |
| --- |
| select e.ename, d.dname from emp e, dept d where e.deptno=d.deptno; |

第二种做法，在select语句中再次嵌套select语句完成部分名称的查询

|  |
| --- |
| select e.ename, (select d.dname from dept d where e.deptno=d.deptno) as dname from emp e; |

![](../_assets/64567bd0_1752207313547-93528b39-4d40-48b2-a65c-887876329e0d.png)

## 11、分页查询
### limit

|  |
| --- |
| -select \* from emp limit 起始索引位置，分页单位 //分页单位（多少条数据为一页）    -将前三个数据分一页  -select \* from emp limit 0,3;  -将前四五六数据分一页  -select \* from emp limit 3,3;  -将前七八九数据分一页  -select \* from emp limit 6,3;  -起始索引位置=（当前页-1）\*分页单位    -查询第六个到第十个员工的详细信息  -select \* from emp limit 5,5; |
