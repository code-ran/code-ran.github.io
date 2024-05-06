---
title: sql重要知识总结
date: 2022-12-26 10:39:40
tags: sql
categories: sql相关
---

#####                                                                                                  sql重要知识总结：

###### 1、order by语句

```sql
order by 关键字用于对结果集按照一个列或者多个列进行排序。(对于多列，如果第一列有重复的值，那么再根据第二列进行排序)
order by 关键字默认按照升序对记录进行排序。如果需要按照降序对记录进行排序，可以使用 DESC 关键字。
```

注意：若列名后不加限制条件，将直接按照离order by语句最近的无限制条件的列名排序。

```sql
//以id的值进行默认的升序排序
SELECT * FROM websites ORDER BY id , alexa;
SELECT * FROM websites ORDER BY id ASC, alexa DESC;

//以alexa的值进行默认的升序排序
SELECT * FROM websites ORDER BY alexa, id;

//以id的值进行降序排序
SELECT * FROM websites ORDER BY id DESC, alexa DESC;
SELECT * FROM websites ORDER BY id DESC, alexa ASC;
```

增加条件后

```sql
//以id分为两组:<= 3为第一组， > 3 为第二组,然后每个组根据字段alexa进行默认升序排序
SELECT * FROM websites ORDER BY id >3, alexa

//以id分为两组:<= 3为第一组， > 3 为第二组,然后每个组根据字段alexa进行降序排序
SELECT * FROM websites ORDER BY id >3, alexa DESC

//以id分为两组:> 3为第一组， <= 3 为第二组,然后每个组根据字段alexa进行降序排序
SELECT * FROM websites ORDER BY id >3 DESC, alexa DESC

//按照alexa进行升序排序,此时后面的条件不生效,跟SELECT * FROM websites ORDER BY alexa, id查询的结果集相同
SELECT * FROM websites ORDER BY alexa, id > 3
```

