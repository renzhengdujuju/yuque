---
title: "Mysql案例："
yuque_slug: "minq0iycxgfdg4u6"
yuque_id: 227482311
yuque_updated: "2026-10-14T04:54:04.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/minq0iycxgfdg4u6"
---
# Student表的定义

|  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **字段名** | **字段描述** | **数据类型** | **主键** | **外键** | **非空** | **唯一** | **自增** |
| Id | 学号 | INT(10) | 是 | 否 | 是 | 是 | 是 |
| Name | 姓名 | VARCHAR(20) | 否 | 否 | 是 | 否 | 否 |
| Sex | 性别 | VARCHAR(4) | 否 | 否 | 否 | 否 | 否 |
| Birth | 出生年份 | YEAR | 否 | 否 | 否 | 否 | 否 |
| Department | 院系 | VARCHAR(20) | 否 | 否 | 是 | 否 | 否 |
| Address | 家庭住址 | VARCHAR(50) | 否 | 否 | 否 | 否 | 否 |

# Score表的定义

|  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **字段名** | **字段描述** | **数据类型** | **主键** | **外键** | **非空** | **唯一** | **自增** |
| Id | 编号 | INT(10) | 是 | 否 | 是 | 是 | 是 |
| Stu\_id | 学号 | INT(10) | 否 | 否 | 是 | 否 | 否 |
| C\_name | 课程名 | VARCHAR(20) | 否 | 否 | 否 | 否 | 否 |
| Grade | 分数 | INT(10) | 否 | 否 | 否 | 否 | 否 |

1.创建student和score表

2.为student表和score表增加记录

向student表插入记录的INSERT语句如下：INSERT INTO student VALUES( 901,'张老大', '男',1995,'计

算机系', '北京市海淀区');

INSERT INTO student VALUES( 902,'张老二', '男',2000,'中

文系', '北京市昌平区');

INSERT INTO student VALUES( 903,'张三', '女',1998,'中文

系', '湖南省永州市');

INSERT INTO student VALUES( 904,'李四', '男',1990,'英语

系', '辽宁省阜新市');

INSERT INTO student VALUES( 905,'王五', '女',1991,'英语

系', '福建省厦门市');

INSERT INTO student VALUES( 906,'王六', '男',2003,'计算

机系', '湖南省衡阳市');

向score表插入记录的INSERT语句如下：

INSERT INTO score VALUES(1,901, '计算机',98);

INSERT INTO score VALUES(2,901, '英语', 80);

INSERT INTO score VALUES(3,902, '计算机',65);

INSERT INTO score VALUES(4,902, '中文',88);

INSERT INTO score VALUES(5,903, '中文',95);

INSERT INTO score VALUES(6,904, '计算机',70);

INSERT INTO score VALUES(7,904, '英语',92);

INSERT INTO score VALUES(8,905, '英语',94);

INSERT INTO score VALUES(9,906, '计算机',90);

INSERT INTO score VALUES(10,906, '英语',85);

3.查询student表的所有记录

4.查询student表的第2条到4条记录5.从student表查询所有学生的学号（id）、姓名（name）和院系

（department）的信息

6.从student表中查询计算机系和英语系的学生的信息

7.从student表中查询年龄18~25岁的学生信息

8.从student表中查询每个院系有多少人

9.从score表中查询每个科目的最高分

10.查询李四的考试科目（c\_name）和考试成绩（grade）

11.用连接的方式查询所有学生的信息和考试信息

12.计算每个学生的总成绩

13.计算每个考试科目的平均成绩

14.查询计算机成绩低于95的学生信息

15.查询同时参加计算机和英语考试的学生的信息

# 案例：

### ✅ 1. 创建 `student` 和 `score` 表

```sql
-- 创建 student 表
CREATE TABLE student (
  Id INT(10) PRIMARY KEY AUTO_INCREMENT,
  Name VARCHAR(20) NOT NULL,
  Sex VARCHAR(4),
  Birth YEAR,
  Department VARCHAR(20) NOT NULL,
  Address VARCHAR(50)
);

-- 创建 score 表
CREATE TABLE score (
  Id INT(10) PRIMARY KEY AUTO_INCREMENT,
  Stu_id INT(10) NOT NULL,
  C_name VARCHAR(20),
  Grade INT(10)
);
```

#### `PRIMARY KEY`

- 表示该字段是**主键（Primary Key）**
- 主键的作用：唯一标识表中的每一行记录
- 特性：

- **唯一性（Unique）**：不能有重复值
- **非空性（Not Null）**：不能为 NULL

#### `AUTO_INCREMENT`

- 表示该字段的值会**自动递增**
- 每次插入新记录时，如果不指定该字段的值，MySQL 会自动为其赋一个比当前最大值大 1 的值
- 通常用于自动生成唯一的 ID（如学号、订单号等）

---

### 示例说明

```sql
CREATE TABLE student (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20) NOT NULL
);
```

在这个例子中：

|  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| **字段名** | **数据类型** | **是否主键** | **是否自增** | **是否可为空** | **描述** |
| id | INT | 是 | 是 | 否 | 学生编号 |
| name | VARCHAR(20) | 否 | 否 | 否 | 学生姓名 |

---

### 使用注意事项

1. **只能有一个自增字段**

- 一张表中最多只能有一个字段设置为 `AUTO_INCREMENT`

2. **必须为主键或唯一索引**

- 一般都和 `PRIMARY KEY` 一起使用

3. **数据类型必须是整数类型**

- 如 `INT`, `BIGINT` 等
- 不支持字符串、浮点数等类型

4. **自增值从 1 开始**

- 默认情况下，第一个插入的记录 `id` 为 1
- 可通过 `ALTER TABLE` 修改起始值

---

### 常用操作示例

#### 插入数据时无需指定自增字段：

```sql
INSERT INTO student (name) VALUES ('张三');
```

MySQL 自动分配 `id`，比如 1、2、3...

#### 查看当前自增值：

```sql
SHOW CREATE TABLE student;
```

#### 修改自增起始值：

```sql
ALTER TABLE student AUTO_INCREMENT = 100;
```

下次插入的记录将从 100 开始。

---

### ✅2. 插入记录

#### 向 `student` 表插入记录：

```sql
INSERT INTO student VALUES (901, '张老大', '男', 1995, '计算机系', '北京市海淀区');
INSERT INTO student VALUES (902, '张老二', '男', 2000, '中文系', '北京市昌平区');
INSERT INTO student VALUES (903, '张三', '女', 1998, '中文系', '湖南省永州市');
INSERT INTO student VALUES (904, '李四', '男', 1990, '英语系', '辽宁省阜新市');
INSERT INTO student VALUES (905, '王五', '女', 1991, '英语系', '福建省厦门市');
INSERT INTO student VALUES (906, '王六', '男', 2003, '计算机系', '湖南省衡阳市');
```

#### 向 `score` 表插入记录：

```sql
INSERT INTO score VALUES (1, 901, '计算机', 98);
INSERT INTO score VALUES (2, 901, '英语', 80);
INSERT INTO score VALUES (3, 902, '计算机', 65);
INSERT INTO score VALUES (4, 902, '中文', 88);
INSERT INTO score VALUES (5, 903, '中文', 95);
INSERT INTO score VALUES (6, 904, '计算机', 70);
INSERT INTO score VALUES (7, 904, '英语', 92);
INSERT INTO score VALUES (8, 905, '英语', 94);
INSERT INTO score VALUES (9, 906, '计算机', 90);
INSERT INTO score VALUES (10, 906, '英语', 85);
```

---

### ✅ 3. 查询 `student` 表的所有记录

```sql
SELECT * FROM student;
```

---

### ✅ 4. 查询 `student` 表的第 2 条到第 4 条记录

```sql
SELECT * FROM student LIMIT 1, 3;
```

注意：MySQL 的 `LIMIT offset, count`，从第 `offset` 条开始取 `count` 条。

---

### ✅ 5. 查询学生的学号、姓名和院系信息

```sql
SELECT Id, Name, Department FROM student;
```

---

### ✅ 6. 查询计算机系和英语系的学生信息

```sql
SELECT * FROM student WHERE Department IN ('计算机系', '英语系');
```

---

### ✅ 7. 查询年龄在 18~25 岁之间的学生信息（假设当前年份为 2025）

```sql
SELECT * FROM student WHERE (2025 - Birth) BETWEEN 18 AND 25;
```

---

### ✅ 8. 查询每个院系有多少人

```sql
SELECT Department '院系', COUNT(*)  Total '人数'FROM student GROUP BY Department;
```

---

### ✅ 9. 查询每门科目的最高分

```sql
SELECT C_name, MAX(Grade)  MaxGrade FROM score GROUP BY C_name;
```

---

### ✅ 10. 查询李四的考试科目和成绩

```sql
SELECT s.C_name, s.Grade
FROM score s
JOIN student st ON s.Stu_id = st.Id
WHERE st.Name = '李四';
```

---

### ✅ 11. 使用连接查询所有学生的信息和考试信息

```sql
SELECT st.*, s.C_name, s.Grade
FROM student st
JOIN score s ON st.Id = s.Stu_id;
```

---

### ✅ 12. 计算每个学生的总成绩

```sql
SELECT st.Id, st.Name, SUM(s.Grade) AS TotalScore
FROM student st
JOIN score s ON st.Id = s.Stu_id
GROUP BY st.Id, st.Name;
```

---

### ✅ 13. 计算每门科目的平均成绩

```sql
SELECT C_name, AVG(Grade) AS AvgGrade FROM score GROUP BY C_name;
```

---

### ✅ 14. 查询计算机成绩低于 95 分的学生信息

```sql
SELECT st.*
FROM student st
JOIN score s ON st.Id = s.Stu_id
WHERE s.C_name = '计算机' AND s.Grade < 95;
```

---

### ✅ 15. 查询同时参加计算机和英语考试的学生信息

```sql
SELECT st.*
FROM student st
JOIN score s1 ON st.Id = s1.Stu_id AND s1.C_name = '计算机'
JOIN score s2 ON st.Id = s2.Stu_id AND s2.C_name = '英语';
```

```sql
select * from student where id in (select stu_id from score where c_name in ("计算机","英语") group by stu_id having count(c_name) = 2);
```

```sql
select st.* from student st join score sc1 on st.Id=sc1.Stu_id jOIN score sc2 on st.id=sc2.Stu_id WHERE sc1.C_name="计算机" and sc2.C_name="英语";
```
