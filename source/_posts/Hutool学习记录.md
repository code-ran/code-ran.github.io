---
title: Hutool学习记录
date: 2022-12-20 16:41:16
tags: hutool
categories: hutool相关
---

##### 1、官网介绍

```
   Hutool是一个小而全的Java工具类库，通过静态方法封装，降低相关API的学习成本，提高工作效率，使Java拥有函数式语言般的优雅，让Java语言也可以“甜甜的”。
   Hutool中的工具方法来自每个用户的精雕细琢，它涵盖了Java开发底层代码中的方方面面，它既是大型项目开发中解决小问题的利器，也是小型项目中的效率担当；
   Hutool是项目中“util”包友好的替代，它节省了开发人员对项目中公用类和公用工具方法的封装时间，使开发专注于业务，同时可以最大限度的避免封装不完善带来的bug。
```

地址： https://www.hutool.cn/docs/#/

##### 2、学习版本

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.8.10</version>
</dependency>
```

##### 练习后的代码仓库地址：

https://github.com/code-ran/hutool_study

##### 3、类型转换

```xml
    在Java开发中我们要面对各种各样的类型转换问题，尤其是从命令行获取的用户参数、从HttpRequest获取的Parameter等等，这些参数类型多种多样，我们怎么去转换他们呢？常用的办法是先整成String，然后调用XXX.parseXXX方法，还要承受转换失败的风险，不得不加一层try catch，这个小小的过程混迹在业务代码中会显得非常难看和臃肿。hutool到的Convert类里面封装了针对Java常见类型的转换，用于简化类型转换。Convert类中大部分方法为toXXX，参数为Object，可以实现将任意可能的类型转换为指定类型。同时支持第二个参数defaultValue用于在转换失败时返回一个默认值。
```

###### (1)基本数据类型转字符串

```java
int a = 1;
//结果为："1"
String aStr = Convert.toStr(a);
//基本数据类型数组转字符串数组
long[] b = {1, 2, 3, 4, 5};
//结果为："[1, 2, 3, 4, 5]"
String bStr = Convert.toStr(b);
```

转换为指定类型数组：

```java
//字符串数组转Integer类型数组
String[] c = { "1", "2", "3", "4" };
//结果为："["1", "2", "3", "4"]"
Integer[] intArray = Convert.toIntArray(c);
//long类型的数组转Integer类型数组
long[] d = {1,2,3,4,5};
//结果为：[1,2,3,4,5]
Integer[] intArray2 = Convert.toIntArray(d);
```

转换为日期对象

```java
String e = "2022-12-20 15:34:13";
//结果为：Tue Dec 20 15:34:13 CST 2022
Date date = Convert.toDate(e);
```

转换为集合

```java
Object[] f = {"a", "你", "好", "", 1};
//结果为：
List<?> list = Convert.convert(List.class, f);
//从4.1.11开始可以这么用
List<?> list1 = Convert.toList(f);
```

注意:通过convert(TypeReference<T> reference, Object value)方法，自行new一个TypeReference对象可以对嵌套泛型进行类型转换，例如：

```java
Object[] g = {"a", "你", "好", "", 1};
Object[] h = {"1", "2", "3", "", 4};
List<String> list2 = Convert.convert(new TypeReference<List<String>>() {},g);
List<Integer> list3 = Convert.convert(new TypeReference<List<Integer>>() {},h);
```

半角转全角

```java
String i = "123456789";
//结果为："１２３４５６７８９"
String sbc = Convert.toSBC(i);
```

全角转半角

```java
String j = "１２３４５６７８９";
//结果为"123456789"
String dbc = Convert.toDBC(j);

String j1 = "１     ２３    ４５６  ７８９";
//结果为: "１     ２３    ４５６  ７８９"
String dbc1 = Convert.toDBC(j1);
```
