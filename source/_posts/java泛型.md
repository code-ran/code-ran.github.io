---
title: java泛型
date: 2022-11-23 15:32:14
tags: java
categories: java相关

---

#### java泛型

##### 1、泛型类  

```
使用了泛型定义的类就是泛型类    
泛型类的格式：
修饰符 class 类名<泛型变量>{
        }
泛型变量建议使用 E , T , K , V
泛型类的核心思想：是把出现泛型变量的地方全部替换成传输的真实数据类型。
```

```java
public class GenericDemo {
    public static void main(String[] args) {
        MyArrayList<String> lists = new MyArrayList<String>();
        MyArrayList<String> lists1 = new MyArrayList<>();
        lists1.add("java");
        lists1.add("mysql");
        lists1.remove("java");
        System.out.println(lists1);
    }
}

class MyArrayList<E>{
    private ArrayList lists = new ArrayList();
    public void add(E e){
        lists.add(e);
    }
    public void remove(E e){
        lists.remove(e);
    }
    @Override
    public String toString() {
        return lists.toString();
    }
}
```

##### 2、自定义泛型方法。

```
泛型方法的定义格式:
    修饰符 <泛型变量> 返回值类型 方法名称(形参列表){

    }
泛型方法和泛型类可以做通用技术架构
```

```java
public class GenericDemo {
    public static void main(String[] args) {
        Integer[] nums = {10 , 20 , 30 , 40 , 50};
        String rs1  = arrToString(nums);
        System.out.println(rs1);

        String[] names = {"小明","李华","小美"};
        String rs2  = arrToString(names);
        System.out.println(rs2);
    }

    public static <T> String arrToString(T[] nums){
        StringBuilder sb = new StringBuilder();
        sb.append("[");
        if(nums!=null && nums.length > 0){
            for(int i = 0 ; i < nums.length ; i++ ){
                T ele = nums[i];
                sb.append(i == nums.length-1 ? ele : ele+", ");
            }
        }
        sb.append("]");
        return sb.toString();
    }
}
```