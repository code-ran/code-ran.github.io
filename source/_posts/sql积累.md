---
title: sql积累
date: 2023-06-19 11:47:09
tags: sql
categories: sql相关
---

1、查询某一字段相同，但另一个字段值最大的记录

方法一

```sql
SELECT a.* FROM app a 
WHERE a.app_version = (SELECT MAX(b.app_version) FROM app b WHERE b.app_name = a.app_name) 
ORDER BY a.app_name;
```

方法二

```sql
SELECT a.* FROM app a
WHERE NOT EXISTS(SELECT 1 FROM app b WHERE b.app_name = a.app_name AND b.app_version > a.app_version)
ORDER BY a.app_name;
```

方法三

```sql
SELECT a.* 
FROM app a, (SELECT app_name, MAX(app_version) AS app_version FROM app GROUP BY app_name) b
WHERE a.app_name = b.app_name
AND a.app_version = b.app_version
ORDER BY a.app_name;
```

方法四

```sql
SELECT a.* 
FROM app a INNER JOIN (SELECT app_name, MAX(app_version) AS app_version FROM app GROUP BY app_name) b
ON a.app_name = b.app_name
AND a.app_version = b.app_version
ORDER BY a.app_name;
```
