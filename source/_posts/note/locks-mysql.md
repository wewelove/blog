---
title: MySQL 如何查看数据库锁
abbrlink: 4f4d5766
categories:
  - [笔记]
tags:
  - MySQL
  - 数据库
  - 数据库锁
date: 2025-08-12 07:47:23
---

在 MySQL 中，可以使用以下命令来查看数据库中的锁信息：

查看当前数据库中的所有锁信息:

```sql
SHOW OPEN TABLES WHERE In_use > 0;
```

该命令可以显示当前数据库中正在使用的表的锁信息，包括表名、锁的类型、锁的状态等。

查看当前数据库中正在被锁定的进程信息：

```sql
SHOW PROCESSLIST;
SHOW FULL PROCESSLIST; -- 包含了完整的 SQL
SHOW DETAIL PROCESSLIST; -- 包含了锁
```

该命令可以显示当前数据库中正在执行的所有进程信息，包括进程的 ID、用户、主机、数据库、命令、时间等。通过查看该信息，可以找出哪些进程正在持有或等待锁。

可能的锁状态包括：

- Locked
- Waiting for lock
- Lock wait timeout exceeded

查看当前数据库中所有的锁信息：

```sql
SELECT * FROM information_schema.INNODB_LOCKS;
SELECT * FROM INFORMATION_SCHEMA.TABLE_LOCKS;
```

此表包含有关当前已获取锁的信息，包括：

- lock_id：锁的唯一标识符
- lock_mode：锁的类型 (例如，共享锁、排他锁)
- transaction_id：获取锁的事务 ID
- object_instance_id：锁定的对象
- lock_type：锁定的对象类型 (例如，表锁、行锁)

通过查询 information_schema.INNODB_LOCKS 表，可以查看当前数据库中所有的锁信息，包括锁的类型、锁的状态、锁的持有者等。

通过以上方法，可以查看数据库中的锁信息，帮助我们了解当前数据库中的锁情况，及时处理锁冲突问题

查看所有数据库容量大小:

```sql
SELECT
    table_schema AS '数据库',
    sum( table_rows ) AS '记录数',
    sum( TRUNCATE ( data_length / 1024 / 1024, 2 )) AS '数据容量(MB)',
    sum(TRUNCATE ( index_length / 1024 / 1024, 2 )) AS '索引容量(MB)'
FROM information_schema.TABLES
GROUP BY table_schema
ORDER BY sum( data_length ) DESC, sum( index_length ) DESC;
```

显示 SQL 耗时:

```sql
SHOW FULL PROCESSLIST; -- 包含了完整的 SQL
SHOW DETAIL PROCESSLIST; -- 包含了锁
```
