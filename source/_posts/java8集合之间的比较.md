---
title: java8集合之间的比较
date: 2022-11-23 15:28:14
tags: java
categories: java相关

---

####                                             java两个集合之间的比较-java8流式写法

##### 1、ListUtils.java

```java
package org.example;

import java.util.List;
import java.util.stream.Collectors;

/**
 * @ClassName &{NAME}
 * @Description ListUtils
 * @Author code-ran
 * @Date 2022/6/6 08:54
 * @Version1.0
 */
public class ListUtils {
    //A 和 B 都有
    public static List<String> overlapA(List<String> listA, List<String> listB) {
        //交集
        List<String> collect1 = listA.stream().filter(num -> listB.contains(num))
                .collect(Collectors.toList());
//        System.out.println("A 和 B 都有: " + collect1);
        return collect1;
    }

    //A 有 B 没有
    public static List<String> overlapB(List<String> listA, List<String> listB) {
        List<String> collect2 = listA.stream().filter(num -> !listB.contains(num))
                .collect(Collectors.toList());
//        System.out.println("A 有 B 没有:" + collect2);
        return collect2;
    }

    //A 没有 B 有
    public static List<String> overlapC(List<String> listA, List<String> listB) {
        List<String> collect3 = listB.stream().filter(num -> !listA.contains(num))
                .collect(Collectors.toList());
//        System.out.println("A 没有 B 有: " + collect3);
        return collect3;
    }

}
```

##### 2、mainTest.java

```java
package org.example;

import java.util.ArrayList;
import java.util.List;

/**
 * @ClassName &{NAME}
 * @Description main
 * @Author code-ran
 * @Date 2022/6/16 17:02
 * @Version1.0
 */

public class mainTest {
    public static void main(String[] args) {
        List<String> list1 = new ArrayList<String>();
        list1.add("ceshi111");
        list1.add("ceshi222");
        list1.add("ceshi333");

        List<String> list2 = new ArrayList<String>();
        list2.add("ceshi111");
        list2.add("ceshi333");
        list2.add("ceshi444");

        //list1与list2的交集
        List<String> mixLst = new ArrayList<>();
        //list1 有 list2 没有
        List<String> complementLst = new ArrayList<>();
        //list1 没有 list2 有
        List<String> tocomplementLst = new ArrayList<>();

        mixLst = ListUtils.overlapA(list1, list2);
        System.out.println("两个集合的交集为：" + mixLst);
        complementLst = ListUtils.overlapB(list1, list2);
        System.out.println("list1 有 list2 没有：" + complementLst);
        tocomplementLst = ListUtils.overlapC(list1, list2);
        System.out.println("list1 没有 list2 有：" + tocomplementLst);
    }
}
```


