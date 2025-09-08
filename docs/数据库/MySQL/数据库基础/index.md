# 数据库基础

## 一、 **数据库概念与发展**

<div style="background-color: #e2f7e1; border-left: 5px solid #28a745; padding: 10px; border-radius: 5px; margin: 10px 0;">
  <strong>⏱️ 1.1 </strong> 数据、信息与数据库的关系
</div>


数据库是按照数据结构来组织、存储和管理数据的仓库。它本质上是一个长期存储在计算机内的、有组织的、可共享的数据集合。

数据、信息与数据库的关系

* 数据 (Data)：
  > 是未经处理的原始事实或符号，比如“张三”、“25”、“男”。单独看，这些数据并没有太多意义。

* 信息 (Information)：
> 是经过处理和组织、具有特定意义的数据。例如，当我们将数据“张三”、“25”、“男”组合起来，并赋予其“姓名”、“年龄”、“性别”等标签，我们就得到了有意义的信息，即“张三，男，25岁”。

* 数据库 (Database)：
> 是存储数据的系统化集合，旨在方便数据的存储、管理和检索，从而可以高效地将数据转化为有用的信息。

数据库是数据和信息之间的桥梁，它通过结构化的方式组织数据，使得我们能够方便地获取和利用信息。

<div style="background-color: #e2f7e1; border-left: 5px solid #28a745; padding: 10px; border-radius: 5px; margin: 10px 0;">
  <strong>⏱️ 1.2 </strong>  数据库的产生与发展（层次式、网状、关系型、NoSQL）
</div>

数据库的发展经历了从简单到复杂、从单一到多元的演变，主要可以划分为以下几个阶段：

层次式数据库 (Hierarchical Database)：这是最早的数据库模型，结构像一棵倒挂的树。数据以一对多的父子关系组织。

特点：结构简单，查询速度快，但灵活性差。如果想查询子节点下的其他子节点，需要从根节点开始遍历。

代表：IBM 的 IMS (Information Management System)。

网状数据库 (Network Database)：这是对层次模型的改进，允许一个子节点有多个父节点，从而形成了网状结构。

特点：比层次式更灵活，能更好地表示现实世界中多对多的关系，但结构依然复杂，维护和查询困难。

代表：Codasyl DBTG。

关系型数据库 (Relational Database)：这是目前应用最广泛的数据库类型，由 Edgar F. Codd 于 1970 年提出。它采用表格（二维表）来组织数据，每个表由行和列组成，表与表之间通过主键 (Primary Key) 和外键 (Foreign Key) 建立关系。

特点：模型简单，易于理解和使用；数据独立性高；支持强大的 SQL (Structured Query Language) 进行数据操作。

代表：MySQL, PostgreSQL, Oracle, SQL Server。

NoSQL (Not Only SQL) 数据库：随着互联网和大数据时代的到来，传统关系型数据库在处理海量数据、高并发和灵活数据模型方面遇到了挑战。NoSQL 数据库应运而生，它泛指非关系型数据库，通常不遵循固定的表格模式。

特点：高可扩展性，能轻松处理海量数据；数据模型多样（键值、文档、列族、图等）；读写性能高。

代表：

键值 (Key-Value)：Redis, Memcached

文档 (Document)：MongoDB, CouchDB

列族 (Column-Family)：Cassandra, HBase

图 (Graph)：Neo4j


<div style="background-color: #e2f7e1; border-left: 5px solid #28a745; padding: 10px; border-radius: 5px; margin: 10px 0;">
  <strong>⏱️ 1.3 </strong>  数据库管理系统（DBMS）的功能与特点
</div>

数据库管理系统 (DBMS) 是一个位于用户和操作系统之间的数据管理软件，它负责数据库的定义、管理、维护和安全。

主要功能：

数据定义 (Data Definition)：提供数据定义语言 (DDL)，用于定义数据库的结构，如创建、修改和删除表、视图、索引等。

数据操纵 (Data Manipulation)：提供数据操纵语言 (DML)，用于对数据进行增、删、改、查 (CRUD) 操作。

数据控制 (Data Control)：提供数据控制语言 (DCL)，用于管理用户权限、事务和数据完整性。

数据共享和安全：允许多用户同时访问数据，并提供权限管理、数据加密等安全机制。

数据完整性：通过约束条件（如主键、外键、唯一约束）确保数据的正确性和一致性。

故障恢复：提供备份和恢复机制，确保在系统故障或意外情况下数据不丢失。

<div style="background-color: #e2f7e1; border-left: 5px solid #28a745; padding: 10px; border-radius: 5px; margin: 10px 0;">
  <strong>⏱️ 1.4 </strong>  常见数据库产品
</div>

MySQL：最流行的开源关系型数据库，易于使用，性能出色，广泛应用于 Web 应用。

PostgreSQL：功能强大的开源关系型数据库，被誉为“最先进的开源数据库”，支持复杂的数据类型和高级特性。

Oracle：商业关系型数据库的巨头，功能全面，性能卓越，主要用于大型企业级应用。

SQL Server：微软开发的商业关系型数据库，与 .NET 平台集成紧密，适用于 Windows 环境。

MongoDB：领先的开源文档型 NoSQL 数据库，以其灵活的 JSON-like 数据模型而闻名，非常适合处理非结构化数据。

Redis：高性能的键值型 NoSQL 数据库，主要用于缓存、会话管理和实时分析。


## 二、 **关系数据库模型**



## 三、 **SQL 基础**

> SQL（Structured Query Language，结构化查询语言）是用于管理和操作关系型数据库的标准语言。它广泛应用于数据查询、数据操作、数据定义和数据控制。以下是对SQL语言基础的详细讲解，涵盖其核心概念、语法和常见操作。

---

### 3.1 SQL 概述

SQL 是一种声明式语言，用户通过描述“想要什么”而不是“如何做”来与数据库交互。SQL 的主要功能包括：
<div>
  <ul>
      <li>数据查询：从数据库中检索数据（如 <code>SELECT</code>）。 </li>
      <li>数据操作：插入（`INSERT`）、更新（<code>UPDATE</code>）、删除（<code>DELETE</code>）数据。</li>
      <li>数据定义：定义和修改数据库结构（如 <code>CREATE、ALTER、DROP</code>）。</li>
      <li>数据控制：管理访问权限和安全（如 <code>GRANT、REVOKE</code>）。</li>
  </ul>
</div>
SQL 适用于关系型数据库管理系统（RDBMS），如 MySQL、PostgreSQL、SQL Server、Oracle 等。虽然不同数据库系统对 SQL 的实现略有差异，但标准 SQL（ANSI/ISO 标准）是通用的。

---

### 3.2 SQL 的基本组成

SQL 语句通常由以下几类命令组成：

  1. **DL（Data Definition Language，数据定义语言）**：
    - 用于定义和修改数据库结构。
    - 常见命令：`CREATE`、`ALTER`、`DROP`、`TRUNCATE`。
  
  2. **DML（Data Manipulation Language，数据操作语言）**：
    - 用于操作数据库中的数据。
    - 常见命令：`SELECT`、`INSERT`、`UPDATE`、`DELETE`。

  3. **DCL（Data Control Language，数据控制语言）**：
    - 用于定义访问权限和安全级别。
    - 常见命令：`GRANT`、`REVOKE`。

  4. **TCL（Transaction Control Language，事务控制语言）**：
    - 用于管理事务。
    - 常见命令：`COMMIT`、`ROLLBACK`、`SAVEPOINT`。
---

### 3.3 SQL 基础语法
以下是 SQL 语言的核心语法和操作，结合示例逐步讲解。

#### 1. 数据查询（SELECT）

`SELECT` 是 SQL 中最常用的语句，用于从数据库中检索数据。

**基本语法**：
```sql
SELECT column1, column2, ... 
FROM table_name 
WHERE condition 
GROUP BY column 
HAVING condition 
ORDER BY column [ASC | DESC];
```

**示例**：

假设有一个名为 `students` 的表，结构如下：

| id  | name    | age | score |
|-----|---------|-----|-------|
| 1   | Alice   | 20  | 85    |
| 2   | Bob     | 22  | 90    |
| 3   | Charlie | 19  | 78    |

- 查询所有学生的姓名和年龄：

```sql
SELECT name, age 
FROM students;
```
结果：

| name    | age |
|---------|-----|
| Alice   | 20  |
| Bob     | 22  |
| Charlie | 19  |

- 查询年龄大于 20 的学生：

```sql
SELECT name, age 
FROM students 
WHERE age > 20;
```

结果：

| name | age |
|------|-----|
| Bob  | 22  |

- 按分数降序排序：

```sql
SELECT name, score 
FROM students 
ORDER BY score DESC;
```
结果：

| name    | score |
|---------|-------|
| Bob     | 90    |
| Alice   | 85    |
| Charlie | 78    |

**常用子句**：

- `WHERE`：过滤条件（如 `age > 20`、`name = 'Alice'`）。

- `GROUP BY`：按列分组，常与聚合函数（如 `COUNT`、`SUM`、`AVG`、`MAX`、`MIN`）一起使用。

- `HAVING`：过滤分组结果。

- `ORDER BY`：排序，`ASC`（升序，默认）或 `DESC`（降序）。

#### 2. 数据操作（INSERT, UPDATE, DELETE）
这些语句用于修改数据库中的数据。

- **插入数据（INSERT）**：
```sql
INSERT INTO table_name (column1, column2, ...) 
VALUES (value1, value2, ...);
```
示例：
```sql
INSERT INTO students (id, name, age, score) 
VALUES (4, 'David', 21, 88);
```

- **更新数据（UPDATE）**：
```sql
UPDATE table_name 
SET column1 = value1, column2 = value2, ... 
WHERE condition;
```
示例：
```sql
UPDATE students 
SET score = 92 
WHERE name = 'Alice';
```

- **删除数据（DELETE）**：
```sql
DELETE FROM table_name 
WHERE condition;
```
示例：
```sql
DELETE FROM students 
WHERE age < 20;
```

**注意**：`WHERE` 子句非常重要，若省略可能导致整个表的数据被更新或删除。

#### 3. 数据定义（CREATE, ALTER, DROP）
这些语句用于定义或修改数据库结构。

- **创建表（CREATE TABLE）**：
```sql
CREATE TABLE table_name (
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
);
```
示例：
```sql
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT,
    score DECIMAL(5,2)
);
```
- 数据类型：`INT`（整数）、`VARCHAR`（字符串）、`DECIMAL`（小数）、`DATE`（日期）等。

- 约束：`PRIMARY KEY`（主键）、`NOT NULL`（非空）、`FOREIGN KEY`（外键）、`UNIQUE`（唯一）等。

- **修改表（ALTER TABLE）**：
```sql
ALTER TABLE table_name 
ADD column_name datatype;
```
示例：
```sql
ALTER TABLE students 
ADD email VARCHAR(100);
```

- **删除表（DROP TABLE）**：
```sql
DROP TABLE table_name;
```
示例：
```sql
DROP TABLE students;
```

#### 4. 数据控制（GRANT, REVOKE）

这些语句用于管理数据库的访问权限。

- **授予权限（GRANT）**：

```sql
GRANT privilege_name ON table_name TO user;
```
示例：
```sql
GRANT SELECT, INSERT ON students TO 'user1';
```

- **撤销权限（REVOKE）**：

```sql
REVOKE privilege_name ON table_name FROM user;
```
示例：

```sql
REVOKE INSERT ON students FROM 'user1';
```

#### 5. 事务控制（COMMIT, ROLLBACK）

事务是一组 SQL 操作，要么全部成功，要么全部失败。

- **提交事务（COMMIT）**：

```sql
COMMIT;
```

- **回滚事务（ROLLBACK）**：

```sql
ROLLBACK;
```

示例：

```sql
BEGIN TRANSACTION;
INSERT INTO students (id, name, age, score) VALUES (5, 'Eve', 23, 95);
UPDATE students SET score = 80 WHERE name = 'Eve';
COMMIT;
```

若中途发生错误，可用 `ROLLBACK` 撤销操作。

---

### 3.4 SQL 的高级特性

以下是一些 SQL 的高级功能，适合进一步学习：

**聚合函数**：
<ul>
<li><code>COUNT</code>：统计行数。</li>
<li><code>SUM</code>：求和。</li>
<li><code>AVG</code>：平均值。</li>
<li><code>MAX/MIN</code>：最大/最小值。</li>
</ul>
   示例：
   ```sql
   SELECT AVG(score) 
   FROM students 
   WHERE age > 20;
   ```

**连接（JOIN）**：

> 用于合并多个表的数据。
<div>
  <ul>
    <li><code>INNER JOIN</code>：返回匹配的记录。</li>
    <li><code>LEFT JOIN</code>：返回左表所有记录，右表无匹配记录则返回 NULL。</li>
    <li><code>RIGHT JOIN</code>：类似 LEFT JOIN，反向操作。</li>
    <li><code>FULL JOIN</code>：返回两表所有记录。</li>
  </ul>
</div>

   示例：
   ```sql
   SELECT s.name, c.course_name 
   FROM students s 
   INNER JOIN courses c ON s.id = c.student_id;
   ```

**子查询**：
> 在查询中嵌套另一个查询。
  
示例：

   ```sql
   SELECT name 
   FROM students 
   WHERE score > (SELECT AVG(score) FROM students);
   ```
**索引**：

   提高查询性能。

   ```sql
   CREATE INDEX idx_name ON students(name);
   ```

**视图（VIEW）**：

   虚拟表，基于查询结果。

   ```sql
   CREATE VIEW high_scores AS 
   SELECT name, score 
   FROM students 
   WHERE score > 85;
   ```

---

### 3.5 SQL 最佳实践

1. **规范命名**：表名、列名使用有意义的英文单词，避免保留字。

2. **使用注释**：在复杂查询中使用 `--` 或 `/* */` 添加注释。

3. **避免 SELECT ***：明确指定需要的列，减少资源消耗。

4. **小心 WHERE 条件**：确保条件准确，避免意外修改或删除数据。

5. **优化查询**：使用索引、分析查询计划（`EXPLAIN`）来提高性能。

---

### 3.6 SQL 的学习建议

1. **实践为主**：使用 MySQL、PostgreSQL 或 SQLite 等工具进行练习。

2. **在线资源**：如 LeetCode、HackerRank 提供 SQL 练习题。

3. **理解数据库设计**：学习规范化（Normalization）和 ER 图。

4. **掌握具体数据库的特性**：如 MySQL 的 `LIMIT`、SQL Server 的 `TOP`。

---

### 3.7 示例数据库操作
以下是一个完整的示例，展示从创建表到查询的流程：

```sql
-- 创建表
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50) NOT NULL,
    department VARCHAR(50),
    salary DECIMAL(10,2)
);

-- 插入数据
INSERT INTO employees (emp_id, emp_name, department, salary) 
VALUES 
    (1, 'Alice', 'HR', 50000),
    (2, 'Bob', 'IT', 60000),
    (3, 'Charlie', 'HR', 55000);

-- 查询数据
SELECT emp_name, salary 
FROM employees 
WHERE department = 'HR' 
ORDER BY salary DESC;

-- 更新数据
UPDATE employees 
SET salary = 58000 
WHERE emp_name = 'Charlie';

-- 删除数据
DELETE FROM employees 
WHERE emp_id = 1;
```

---

### 3.8 总结
SQL 是操作关系型数据库的核心语言，掌握其基础语法（如 `SELECT`、`INSERT`、`UPDATE`、`DELETE`）和高级特性（如 `JOIN`、子查询）是数据库开发的基础。通过不断实践和优化查询，你可以高效地管理数据并应对复杂需求。

