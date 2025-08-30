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

   * 表、行、列、主键、外键、索引
   * 实体-联系模型（E-R 模型）
   * 数据约束（主键约束、唯一约束、外键约束、非空、检查约束）

### 3. **SQL 基础**

   * DDL：CREATE、ALTER、DROP
   * DML：INSERT、UPDATE、DELETE
   * DQL：SELECT（WHERE、ORDER BY、GROUP BY、HAVING、LIMIT）
   * DCL：用户与权限管理

