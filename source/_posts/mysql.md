---
title: mysql相关知识
date: 2018-11-16 11:32:33
tags: [mysql]
categories: mysql
---
# MySQL Server日志
**四种日志类型**
- ERROR Log（错误日志）
- General Query Log（一般查询日志）

- Binary Log（数据改动日志）
- Slow Query Log（慢查询SQL语句）

**设置log-bin启动mysql服务失败解决方案**：  
加上server-id=1
# mysql安全模式
有时在使用mysql的批量delete或者update的时候会出现


```
Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column To disable safe mode, toggle the option in Preferences -> SQL Queries and reconnect.
```



原因是因为此时mysql处于安全模式（safe-updates），需要执行命令，即先解除安全模式，待数据更新后再恢复安全模式

```
SET SQL_SAFE_UPDATES = 0;
update maoyao.user set is_banner=b'0' where is_banner=null;
SET SQL_SAFE_UPDATES = 1;
```