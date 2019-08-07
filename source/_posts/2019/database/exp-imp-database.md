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

#### 导入

``` bash

imp user/passwd@ip:port/sid  file=back.dmp  full=y ignore=y

```

