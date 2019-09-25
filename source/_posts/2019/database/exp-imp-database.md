---
title: 数据导入导出
permalink: exp-imp-database
date: 2019-08-07 14:19:24
tags:
---

以dmp为文件作为导入导出


#### 导出

``` bash

exp user/passwd@ip:port/sid   file=back.dmp   owner=user

```

如果表中从来没有insert过数据，这样的表是不分配空间的，导出时无法导出，可以执行以下查询获得分配语句，然后执行分配语句
``` sql

select 'alter table '||table_name||' allocate extent;' from user_tables where num_rows=0 or num_rows is null

```


#### 导入

``` bash

imp user/passwd@ip:port/sid  file=back.dmp  full=y ignore=y

```

