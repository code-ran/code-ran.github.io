---
title: java将字符串中的值和单位分开
date: 2024-07-10 15:51:38
tags: java
categories: java相关
---

1、字符串是由值和单位组成，将值和单位拆分。

```java
package com.ransibi;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class test {
    private static String unit = "A,V,W,kA,kV,kW,s,ms,Ω";

    public static void main(String[] args) {
        String valueTmp = "220kV";
        List<String> unitLst = new ArrayList<>(Arrays.asList(unit.split(",")));
        String unit = extractUnit(valueTmp, unitLst);
        if (unit != null && !unit.isEmpty()){
            String value = valueTmp.replace(unit, "");
            System.out.println("值:"+value);
            System.out.println("单位:"+unit);
        }else {
            System.out.println("值:"+valueTmp);
            System.out.println("单位:"+"");
        }
    }

    private static String extractUnit(String input, List<String> unitLst) {
        // 将所有单位用 "|" 分隔组成一个正则表达式
        String unitRegex = String.join("|", unitLst);
        Pattern pattern = Pattern.compile(unitRegex);
        Matcher matcher = pattern.matcher(input);
        if (matcher.find()) {
            return matcher.group();
        }
        return null;
    }
}
```

```
打印输出:
值:220
单位:kV
```

