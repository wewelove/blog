---
title: Oracle、MySQL、PostgreSQL 数据类型对比转换
date: 2025-08-12 07:47:23
categories:
- 数据
tags: 
- MySQL
- Oracle
- PostgreSQL
- 数据库
- 数据类型
---

Oracle、MySQL 和 PostgreSQL 是三种广泛使用的数据库管理系统，它们各自支持不同的数据类型。在实际应用中，开发者经常需要在这几种数据库之间进行数据迁移或在应用程序中使用不同类型的数据库。因此，理解这些数据库的数据类型以及如何将它们与Java数据类型相互转换是非常重要的。

## 数据类型对比

以下是一些常用的数据类型在 Oracle、MySQL 和 PostgreSQL 之间的比较：

- **整型**
  - Oracle: `NUMBER`, `INTEGER`, `SMALLINT`
  - MySQL: `TINYINT`, `SMALLINT`, `MEDIUMINT`, `INT`, `BIGINT`
  - PostgreSQL: `SMALLINT`, `INTEGER`, `BIGINT`

- **浮点型/定点数**
  - Oracle: `FLOAT`, `REAL`, `BINARY_FLOAT`, `BINARY_DOUBLE`, `NUMBER(p,s)`
  - MySQL: `FLOAT`, `DOUBLE`, `DECIMAL`
  - PostgreSQL: `REAL`, `DOUBLE PRECISION`, `NUMERIC`, `DECIMAL`

- **字符型**
  - Oracle: `CHAR`, `VARCHAR2`, `NCHAR`, `NVARCHAR2`
  - MySQL: `CHAR`, `VARCHAR`, `TINYTEXT`, `TEXT`, `MEDIUMTEXT`, `LONGTEXT`
  - PostgreSQL: `CHAR`, `VARCHAR`, `TEXT`

- **日期和时间**
  - Oracle: `DATE`, `TIMESTAMP`, `TIMESTAMP WITH TIME ZONE`
  - MySQL: `DATE`, `TIME`, `DATETIME`, `TIMESTAMP`
  - PostgreSQL: `DATE`, `TIME`, `TIMESTAMP`, `TIMESTAMPTZ`

- **布尔型**
  - Oracle: 没有专门的布尔类型，通常使用 `NUMBER(1)` 或 `CHAR(1)`
  - MySQL: `BOOLEAN` (实际上是 `TINYINT(1)`)
  - PostgreSQL: `BOOLEAN`

- **二进制数据**
  - Oracle: `BLOB`, `RAW`
  - MySQL: `BLOB`, `TINYBLOB`, `MEDIUMBLOB`, `LONGBLOB`
  - PostgreSQL: `BYTEA`

## 与 Java 数据类型的关系

在 Java 中，JDBC (Java Database Connectivity) 提供了标准接口来连接关系型数据库，并且提供了相应的 Java 类型来表示 SQL 数据类型。以下是常见的 SQL 数据类型到 Java 类型的映射：

- 整型
  - `java.lang.Integer` 或 `int` 对应于 SQL 的 `INTEGER`
  - `java.lang.Long` 或 `long` 对应于 SQL 的 `BIGINT`
  - `java.lang.Short` 或 `short` 对应于 SQL 的 `SMALLINT`

- 浮点型/定点数
  - `java.lang.Float` 或 `float` 对应于 SQL 的 `FLOAT`
  - `java.lang.Double` 或 `double` 对应于 SQL 的 `DOUBLE` 或 `REAL`
  - `java.math.BigDecimal` 对应于 SQL 的 `DECIMAL` 或 `NUMBER`

- 字符串
  - `java.lang.String` 对应于 SQL 的 `VARCHAR`, `CHAR`, `TEXT` 等

- 日期和时间
  - `java.sql.Date` 对应于 SQL 的 `DATE`
  - `java.sql.Time` 对应于 SQL 的 `TIME`
  - `java.sql.Timestamp` 对应于 SQL 的 `TIMESTAMP`

- 布尔值
  - `boolean` 对应于 SQL 的 `BOOLEAN`

- 二进制数据
  - `byte[]` 对应于 SQL 的 `BLOB`, `RAW`, `BYTEA` 等

在实际编程时，你需要根据具体使用的数据库系统选择合适的 JDBC 驱动，并通过 `PreparedStatement` 和 `ResultSet` 来处理数据的读写操作。当从数据库获取数据时，通常会调用 `ResultSet` 的相应方法（如 `getInt`, `getString`, `getTimestamp` 等）将数据转换成对应的 Java 类型；反之，在向数据库插入数据时，则是通过 `PreparedStatement` 的相应方法（如 `setInt`, `setString`, `setTimestamp` 等）设置参数值。
