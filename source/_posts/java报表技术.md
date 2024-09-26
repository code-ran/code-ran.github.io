---
title: java报表技术
date: 2024-09-26 10:19:14
tags: java报表技术
categories: java报表技术相关
---

### 报表技术

#### Excel版本概述

```
目前excel有两个大的版本:Excel2003和Excel2007及以上的版本
```

|                           | Excel2003                      | Excel2007                     |
| ------------------------- | ------------------------------ | ----------------------------- |
| 后缀                      | xls                            | xlsx                          |
| 结构                      | 二进制格式，核心是复合文档结构 | xml类型结构                   |
| 单sheet数据量（最多支持） | 行:65535，列: 256              | 行:1048576，列:16384          |
| 特征                      | 存储容量有限                   | 基于xml压缩，占用率小，效率高 |

#### 常见Excel操作工具

##### Apache POI

官网: https://poi.apache.org/index.html

javadocs: https://poi.apache.org/apidocs/index.html

```
Apache POI是Apache软件基金会的开放源码函式库，POI提供API给Java程序对Microsoft Office格式档案读和写的功能。POI就是对业务数据，可以导入导出，形成报表等的一个工具。
```

api对象介绍

|              | 工作簿       | 工作表    | 行      | 单元格   | 单元格样式    |
| ------------ | ------------ | --------- | ------- | -------- | ------------- |
|              | WorkBook     | Sheet     | Row     | Cell     | CellStyle     |
| 03版本       | HSSFWorkBook | HSSFSheet | HSSFRow | HSSFCell | HSSFCellStyle |
| 07版本及以上 | XSSFWorkBook | XSSFSheet | XSSFRow | XSSFCell | XSSFCellStyle |

##### poi操作excel所需依赖

```xml
<!-- https://mvnrepository.com/artifact/org.apache.poi/poi -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>5.3.0</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.poi/poi-ooxml -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>5.3.0</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.poi/poi-ooxml-schemas -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml-schemas</artifactId>
    <version>4.1.2</version>
</dependency>

```

