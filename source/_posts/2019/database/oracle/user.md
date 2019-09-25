---
title: 用户管理
permalink: user
date: 2019-08-07 14:18:25
tags:
type: oracle
---

以下操作用sysdba进行执行

## 用户创建和授权

``` sql

--创建表空间
create tablespace nav
DATAFILE  'nav.dbf'
size 500M AUTOEXTEND on next 5M maxsize unlimited;

-- 创建用户
create user nav
identified BY nav
default tablespace nav
temporary tablespace TEMP
profile default;

-- 授权
grant connect, resource, dba to nav;
grant connect to nav with admin option;
grant dba to nav with admin option;
grant resource to nav with admin option;

```


## 删除用户及其数据

``` sql

 -- delete user datas
 drop tablespace nav including contents and datafiles;
 
 drop user nav cascade;

```
