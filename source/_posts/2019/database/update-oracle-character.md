---
title: 更改字符集
permalink: update-oracle-character
date: 2019-08-07 11:29:54
tags:
---

## 更改服务端的字符集

``` bash

# 登录
$ sqlplus / as sysdba

```

``` sql
# 将数据库置为mount模式
SQL>shutdown immediate;

SQL>startup mount;

# 修改数据库的一些参数
SQL>ALTER SYSTEM ENABLE RESTRICTED SESSION;
SQL>ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0;
SQL>ALTER SYSTEM SET AQ_TM_PROCESSES=0;

# ALTER OEPN
SQL>alter database open;


# 将字符集更改为ZHS16GBK
SQL>ALTER DATABASE CHARACTER SET ZHS16GBK;

ORA-12712: new character set must be a superset of old character set
(可能会提示这个,那就用下面这个命令)

# 跳过超集检查
SQL>ALTER DATABASE character set INTERNAL_USE ZHS16GBK;



# 再次重启数据库生效
SQL>shutdown immediate;

SQL>startup open;


# 查询是否生效
SQL>select * from v$nls_parameters;

```


## 更改客户端字符集

### Linux 增加环境变量

``` bash

$ echo "NLS_LANG=AMERICAN_AMERICA.UTF8" >> ~/.bash_profile
$ source ~/.bash_profile


```


### Windows 增加环境变量

新建一个环境变量 NLS_LANG=AMERICAN_AMERICA.UTF8 即可