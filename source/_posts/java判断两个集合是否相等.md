---
title: java判断两个集合是否相等
date: 2023-06-19 10:10:48
tags: java
categories: java相关
---

```java
import java.io.Serializable;
import java.util.*;
import java.util.stream.Collectors;

/**
 * 判断两个集合是否相等-不考虑顺序
 */
public class Demo02 implements Serializable {
    public static void main(String[] args) {
        List<String> listA = new ArrayList<>(Arrays.asList("你好a", "完美", "c", "d"));
        List<String> listB = new ArrayList<>(Arrays.asList("d", "完美", "c", "你好a"));
        if (checkDiffrent2(listA, listB)) {
            System.out.println("两个集合相等");
        } else {
            System.out.println("两个集合不相等");
        }
    }

    /**
     * 使用循环遍历
     */
    private static boolean checkDiffrent(List<String> list, List<String> list1) {
        long st = System.nanoTime();
        if(list.size() != list1.size()) {
            System.out.println("消耗时间：" + (System.nanoTime() - st));
            return false;
        }
        for(String str : list) {
            if(!list1.contains(str)) {
                System.out.println("消耗时间：" + (System.nanoTime() - st));
                return false;
            }
        }
        System.out.println("消耗时间：" + (System.nanoTime() - st));
        return true;
    }

    /**
     * 利用List的 retainAll的方法进行判断
     */
    private static boolean checkDiffrent1(List<String> list, List<String> list1) {
        long st = System.nanoTime();
        System.out.println("消耗时间为：" + (System.nanoTime() - st));
        return !list.retainAll(list1);
    }

    /**
     * 利用HashMap key唯一，value可以重复的特点，把list中各种元素放到hashMap中
     */
    private static boolean checkDiffrent2(List<String> list, List<String> list1) {
        long st = System.nanoTime();
        Map<String, Integer> map = new HashMap<>(list.size() + list1.size());
        if (list.size() != list1.size()) {
            System.out.println("消耗时间为： " + (System.nanoTime() - st));
            return false;
        }
        for (String str : list) {
            map.put(str, 1);
        }
        for (String str : list1) {
            Integer cc = map.get(str);
            if (null != cc) {
                continue;
            }
            System.out.println("消耗时间为： " + (System.nanoTime() - st));
            return false;
        }
        System.out.println("消耗时间为： " + (System.nanoTime() - st));
        return true;
    }

    /**
     * 使用stream流去比较两个数组是否相等
     */
    private static boolean checkDiffrent4(List<String> list, List<String> list1) {
        long st = System.nanoTime();
        System.out.println("消耗时间为： " + (System.nanoTime() - st));
        /** 先将集合转成stream流进行排序然后转成字符串进行比较 */
        return list.stream().sorted().collect(Collectors.joining())
                .equals(list1.stream().sorted().collect(Collectors.joining()));
    }

    /**
     * 使用list自带的sort方法先进性排序，然后转成toString去判断两个集合是否相等 --推荐
     */
    private static boolean checkDiffrent5(List<String> list, List<String> list1) {
        long st = System.nanoTime();
        System.out.println("消耗时间为： " + (System.nanoTime() - st));
        list.sort(Comparator.comparing(String::hashCode));
        list1.sort(Comparator.comparing(String::hashCode));
        return list.toString().equals(list1.toString());
    }
    //如果顺序也得一致，使用listA.eauals(listB)即可
}
```

转载自:https://blog.csdn.net/Ecloss/article/details/86154344c
