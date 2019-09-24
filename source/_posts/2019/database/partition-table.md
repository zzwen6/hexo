---
title: Oracle分区表
permalink: partition-table
date: 2019-09-23 14:34:53
tags:
---


sqlplus>
``` sql

-- 1. 查询表是否可以进行在线重定义

exec DBMS_REDEFINITION.CAN_REDEF_TABLE('gdmsaecnew','DETECTIONOFSHIPLIST',DBMS_REDEFINITION.cons_use_pk);

-- 2. 创建一个表与其结构相同的表
-- 已用客户端进行创建

-- 3. 进行同步(客户端执行此命令时会卡住，一直不动，但在sqlplus下执行一下就成功)
exec  DBMS_REDEFINITION.START_REDEF_TABLE('gdmsaecnew','DETECTIONOFSHIPLIST','DETECTIONOFSHIPLIST_TEMP_PART',NULL, DBMS_REDEFINITION.cons_use_pk);


-- 4. 同步对象，如索引，外键等(一定要做，不然会丢失关系)
-- 建一个sql文件，在sqlplus执行这个sql文件 @/path/xx.sql

DECLARE
num_errors PLS_INTEGER;
BEGIN
  DBMS_REDEFINITION.COPY_TABLE_DEPENDENTS('gdmsaecnew', 'DETECTIONOFSHIPLIST','DETECTIONOFSHIPLIST_TEMP_PART',
   DBMS_REDEFINITION.CONS_ORIG_PARAMS, TRUE, TRUE, TRUE, TRUE, num_errors);
END;
/

-- 查询上步出错信息，选择性理会
select object_name, base_table_name, ddl_txt from DBA_REDEFINITION_ERRORS;

-- 6 结束重定义
exec DBMS_REDEFINITION.FINISH_REDEF_TABLE('gdmsaecnew', 'DETECTIONOFSHIPLIST','DETECTIONOFSHIPLIST_TEMP_PART');

-- 7 如果中间出错不知怎么处理，可以先中止
exec DBMS_REDEFINITION.abort_redef_table('gdmsaecnew', 'DETECTIONOFSHIPLIST','DETECTIONOFSHIPLIST_TEMP_PART');

```



