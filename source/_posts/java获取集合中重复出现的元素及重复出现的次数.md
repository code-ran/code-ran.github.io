---
title: java获取集合中重复出现的元素及重复出现的次数
date: 2023-02-14 14:41:27
tags: java
categories: java相关
---

1、java获取集合中重复出现的元素及重复出现的次数

```java
        List<String> list = new ArrayList<>();
        list.add("小明");
        list.add("小明");
        list.add("小美");
        list.add("李华");
        list.add("小乔");
        list.add("李信");
        list.add("李白");
        Map<String,Integer> map = new HashMap<>();
        for (String item : list) {
            //定义一个计数器，记录当前元素出现的次数
            Integer count = 1;
            //如果当前元素在map容器中已存在，计数器+1
            if (map.get(item)!=null) {
                count = map.get(item) + 1;
            }
            //往map容器里存数据
            map.put(item,count);
        }
        for (String key : map.keySet()) {
            System.out.println("元素"+key+"出现的次数为"+map.get(key));
        }
```

