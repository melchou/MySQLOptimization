如何在 MySQL 中对一条 SQL 语句的执行时间进行分析

首先我们需要查看 profiling 是否开启，开启它可以让 MySQL 收集 SQL 执行时所使用的资源情况，命令如下：

```
mysql> select @@profiling;
```

> profiling=0 代表关闭，我们需要把 profiling 打开，即设置为 1：

```
mysql> set profiling=1;
```

然后我们执行一个SQL查询

```
mysql> select * from test.vc;
```

查看当前会话所产生的所有 profiles

```
mysql> show profiles;
+----------+------------+-------------------+
| Query_ID | Duration   | Query             |
+----------+------------+-------------------+
|        1 | 0.00492110 | show profiling    |
|        2 | 0.01828640 | show databases    |
|        3 | 0.00663150 | SELECT DATABASE() |
|        4 | 0.00274690 | show tables       |
|        5 | 0.01757810 | select * from vc  |
+----------+------------+-------------------+
```

从上可以看出我们查询的次数，如果我们想要获取上一次查询的执行时间，可以使用：

```
mysql> show profile;
```

当然你也可以查询指定的 Query ID，比如：

```
mysql> show profile for query 5;
```

```
mysql> show profile;
+----------------------+----------+
| Status               | Duration |
+----------------------+----------+
| starting             | 0.000059 |
| checking permissions | 0.000010 |
| Opening tables       | 0.000020 |
| After opening tables | 0.000013 |
| System lock          | 0.000007 |
| Table lock           | 0.000010 |
| init                 | 0.000018 |
| optimizing           | 0.000012 |
| statistics           | 0.000015 |
| preparing            | 0.000018 |
| executing            | 0.000005 |
| Sending data         | 0.000050 |
| end                  | 0.000014 |
| query end            | 0.000025 |
| closing tables       | 0.000013 |
| Unlocking tables     | 0.000013 |
| freeing items        | 0.000007 |
| updating status      | 0.000050 |
| cleaning up          | 0.000012 |
+----------------------+----------+
19 rows in set (0.16 sec)
```

> checking permissions    检查权限
> 
> Opening tables    打开表
> 
> init    初始化
> 
> System lock    锁系统
> 
> optimizing    优化查询
> 
> preparing    准备
> 
> executing    执行




