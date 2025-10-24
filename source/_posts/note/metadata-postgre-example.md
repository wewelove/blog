---
title: PostgreSQL 数据库元数据查询管理 - 示例
date: 2025-05-11 12:14:57
categories:
  - 笔记
tags:
  - PostgreSQL
  - 元数据
---

在 PostgreSQL 中，可以使用系统表、系统视图和 `information_schema` 来查询用户、数据库、模式、表、表结构、视图等数据库对象的元数据信息。以下是一些常用的查询示例：

## 查询用户信息

1. **查询所有用户**：

   ```sql
   SELECT * FROM pg_user;
   ```

2. **查询用户的权限**：

   ```sql
   \du
   ```

   或者

   ```sql
   SELECT usename, usesuper, usecreatedb, userepl, usebypassrls
   FROM pg_catalog.pg_user;
   ```

3. **查询当前用户**：

   ```sql
   SELECT current_user;
   ```

## 查询数据库信息

1. **查询所有数据库**：

   ```sql
   \l
   ```

   或者

   ```sql
   SELECT * FROM pg_database_info;
   ```

2. **查询特定数据库的信息**：

   ```sql
   SELECT * FROM pg_database_info WHERE datname = 'your_database_name';
   ```

## 查询模式（Schema）信息

1. **查询所有模式**：

   ```sql
   \dn
   ```

   或者

   ```sql
   SELECT * FROM information_schema.schemata;

   -- PostgreSQL
   SELECT * FROM information_schema.schemata WHERE "schema_name" NOT IN (
      'information_schema',
      'pg_catalog',
      'pg_toast'
   );

   -- openGauss
   SELECT * FROM information_schema.schemata WHERE "schema_name" NOT IN (
      'pg_toast','cstore','pkg_service','dbe_perf','snapshot','blockchain',
      'pg_catalog','sqladvisor','dbe_pldebugger','dbe_pldeveloper',
      'dbe_sql_util','information_schema','db4ai'
   );

   -- GaussDB
   SELECT * FROM information_schema.schemata WHERE "schema_name" NOT IN (
      'pg_toast','cstore','dbe_perf','snapshot','blockchain',
      'prvt_ilm','sys','pg_catalog','dbe_ilm_admin','sqladvisor',
      'dbe_pldebugger','dbe_pldeveloper','dbe_sql_util','information_schema',
      'pkg_util','dbe_scheduler','pkg_service','dbe_raw','dbe_utility',
      'dbe_output','dbe_xml','dbe_xmldom','dbe_xmlparser','dbe_describe',
      'dbe_stats','dbe_profiler','dbe_heat_map','dbe_ilm','dbe_compression',
      'dbe_xmlgen','dbe_file','dbe_random','dbe_application_info','dbe_sql',
      'db4ai','dbe_lob','dbe_task','dbe_match','dbe_session'
   );
   ```

2. **查询特定模式的信息**：

   ```sql
   SELECT * FROM information_schema.schemata WHERE schema_name = 'your_schema_name';
   ```

## 查询表信息

1. **查询当前模式中的所有表**：

   ```sql
   \dt
   ```

   或者

   ```sql
   SELECT * FROM information_schema.tables WHERE table_schema = 'your_schema_name' AND table_type = 'BASE TABLE';
   ```

2. **查询特定表的信息**：

   ```sql
   SELECT * FROM information_schema.tables WHERE table_schema = 'your_schema_name' AND table_name = 'your_table_name';
   ```

## 查询表结构

1. **查询表的所有列**：

   ```sql
   \d your_table_name
   ```

   或者

   ```sql
   SELECT * FROM information_schema.columns WHERE table_schema = 'your_schema_name' AND table_name = 'your_table_name';
   ```

2. **查询表的主键**：

   ```sql
   SELECT conname AS constraint_name,
          conrelid::regclass AS table_name,
          a.attname AS column_name
   FROM pg_constraint c
   JOIN pg_attribute a ON a.attnum = ANY(c.conkey)
   WHERE c.contype = 'p' AND c.connamespace = (SELECT oid FROM pg_namespace WHERE nspname = 'your_schema_name') AND c.conrelid = (SELECT oid FROM pg_class WHERE relname = 'your_table_name');
   ```

3. **查询表的外键**：

   ```sql
   SELECT conname AS constraint_name,
          conrelid::regclass AS table_name,
          a.attname AS column_name,
          confrelid::regclass AS referenced_table_name,
          b.attname AS referenced_column_name
   FROM pg_constraint c
   JOIN pg_attribute a ON a.attnum = ANY(c.conkey)
   JOIN pg_attribute b ON b.attnum = ANY(c.confkey)
   WHERE c.contype = 'f' AND c.connamespace = (SELECT oid FROM pg_namespace WHERE nspname = 'your_schema_name') AND c.conrelid = (SELECT oid FROM pg_class WHERE relname = 'your_table_name');
   ```

4. **查询表的索引**：

   ```sql
   \d your_table_name
   ```

   或者

   ```sql
   SELECT indexname, indexdef
   FROM pg_indexes
   WHERE schemaname = 'your_schema_name' AND tablename = 'your_table_name';
   ```

5. **查询表的创建语句**：

   ```sql
   SELECT pg_get_tabledef('your_schema_name.your_table_name');
   ```

## 查询视图信息

1. **查询当前模式中的所有视图**：

   ```sql
   \dv
   ```

   或者

   ```sql
   SELECT * FROM information_schema.views WHERE table_schema = 'your_schema_name';
   ```

2. **查询特定视图的信息**：

   ```sql
   SELECT * FROM information_schema.views WHERE table_schema = 'your_schema_name' AND table_name = 'your_view_name';
   ```

3. **查询视图的定义**：

   ```sql
   \dv+ your_view_name
   ```

   或者

   ```sql
   SELECT definition
   FROM pg_views
   WHERE viewname = 'your_view_name' AND schemaname = 'your_schema_name';
   ```

## 查询其他数据库对象

1. **查询序列**：

   ```sql
   \ds
   ```

   或者

   ```sql
   SELECT * FROM information_schema.sequences WHERE sequence_schema = 'your_schema_name';
   ```

2. **查询存储过程和函数**：

   ```sql
   \df
   ```

   或者

   ```sql
   SELECT * FROM information_schema.routines WHERE routine_schema = 'your_schema_name';
   ```

3. **查询触发器**：

   ```sql
   \d your_table_name
   ```

   或者

   ```sql
   SELECT trigger_name, event_manipulation, action_statement, action_timing, is_enabled
   FROM information_schema.triggers
   WHERE event_object_schema = 'your_schema_name' AND event_object_table = 'your_table_name';
   ```

4. **查询扩展**：

   ```sql
   \dx
   ```

   或者

   ```sql
   SELECT * FROM pg_extension;
   ```

通过这些查询，你可以获取到关于用户、数据库、模式、表、表结构、视图以及其他数据库对象的详细元数据信息。根据你的具体需求，可以选择合适的查询来获取所需的信息。如果你需要更具体的查询或操作，请提供更多详细信息，我可以提供更具体的示例。
