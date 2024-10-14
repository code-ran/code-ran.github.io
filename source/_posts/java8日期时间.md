---
title: java8日期时间
date: 2024-10-14 14:15:14
tags:  java8日期时间
categories: java8日期时间相关
---

### jdk8日期时间类API

旧版日期时间存在的问题:

1、设计很差：在java.util和java.sql的包中都有日期类，java.util.Date同时包含日期和时间，而ava.sql.Date仅包
含日期。此外用于格式化和解析的类在java.text包中定义

2、非线程安全：java.util.Date是非线程安全的，所有的日期类都是可变的，这是java日期类最大的问题之一。

3、时区处理麻烦：日期类并不提供国际化，没有时区支持，因此java引入了java.util.Calendar和java.util.TimeZone类，但他们同样存在上述所有的问题。

#### 概述

jdk新增的日期时间api位于java.time包下，并且是线程安全的。

