---
title: 表空间相关
permalink: tablespace
date: 2019-09-25 10:14:52
tags:
---

## 1. 表空间使用率查询

https://www.cnblogs.com/xwdreamer/p/3511047.html

``` sql

SELECT Upper(F.TABLESPACE_NAME)         "表空间名",
       D.TOT_GROOTTE_MB                 "表空间大小(M)",
       D.TOT_GROOTTE_MB - F.TOTAL_BYTES "已使用空间(M)",
       To_char(Round(( D.TOT_GROOTTE_MB - F.TOTAL_BYTES ) / D.TOT_GROOTTE_MB * 100, 2), '990.99')
       || '%'                           "使用比",
       F.TOTAL_BYTES                    "空闲空间(M)",
       F.MAX_BYTES                      "最大块(M)"
FROM   (SELECT TABLESPACE_NAME,
               Round(Sum(BYTES) / ( 1024 * 1024 ), 2) TOTAL_BYTES,
               Round(Max(BYTES) / ( 1024 * 1024 ), 2) MAX_BYTES
        FROM   SYS.DBA_FREE_SPACE
        GROUP  BY TABLESPACE_NAME) F,
       (SELECT DD.TABLESPACE_NAME,
               Round(Sum(DD.BYTES) / ( 1024 * 1024 ), 2) TOT_GROOTTE_MB
        FROM   SYS.DBA_DATA_FILES DD
        GROUP  BY DD.TABLESPACE_NAME) D
WHERE  D.TABLESPACE_NAME = F.TABLESPACE_NAME
ORDER  BY 1;

-- 表空间的文件
select b.file_id　　文件ID,
　　b.tablespace_name　　表空间,
　　b.file_name　　　　　物理文件名,
　　b.BYTES/1024/1024　　　　　　　"总字节数(M)",
　　(b.bytes-sum(nvl(a.bytes,0)))/1024/1024　　　"已使用(M)",
　　sum(nvl(a.bytes,0))/1024/1024　　　　　　　　"剩余(M)",
　　ROUND( sum(nvl(a.bytes,0))/(b.bytes)*100,2)　剩余百分比
　　from dba_free_space a,dba_data_files b
　　where a.file_id=b.file_id
　　group by b.tablespace_name,b.file_name,b.file_id,b.bytes
　　order by b.tablespace_name;


```

## 2. 创建表空间

``` sql

create TABLESPACE nav -- 表空间名
DATAFILE  'nav.dbf' -- 表空间文件名，可以写绝对路径，默认是放在用户的SID下
size 500M  -- 初始容量为500M
AUTOEXTEND on  -- 会自动增加
next 5M maxsize UNLIMITED; --自动增加大小为5M

```

## 3. 表空间扩容

每一个datafile文件最大支持到32G，当表空间满时，需要增加datafile

``` sql
-- 为表空间nav增加了一个文件，初始值500M，自动增加
alter tablespace nav add datafile '/home/oracle/dbs/nav01.dbf' size 500M AUTOEXTEND on;

```

如果在创建表空间时，没有指定他自动增加，可以用以下命令让其 自动增加
``` sql

alter database datafile 'nav.dbf' AUTOEXTEND ON NEXT 100M MAXSIZE UNLIMITED;

```


## 4. 删除表空间
把表空间的数据和datafile文件都删除
``` sql 
drop tablespace nav including contents and datafiles;

```
仅删除表空间，不删除物理文件
``` sql

drop tablespace nav;
```



## 5. ASM下的表空间
未实操过 TODO
