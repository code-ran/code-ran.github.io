---
title: Arrays.asList方法
date: 2022-11-23 15:28:14
tags: java
categories: java相关

---

####                                    Java Arrays.asList()方法详解

1、使用该方法可以将一个变长参数或者数组转换成List

2、存在的问题：

（1）基本类型数组作为参数的情况：

```java
public class ArraysAsListTest {
	
	public static void main(String[] args) {
		
		int[] a = {1,2,3};
		Integer[] b = {1,2,3};
		
		List listA = Arrays.asList(a);
		List listA1 = Arrays.asList(1,2,3);
		List listB = Arrays.asList(b);
		
		System.out.println(listA.size());//out:1
		System.out.println(listA1.size());//out:3
		System.out.println(listB.size());//out:3
	}
}
```

用int类型的数组作为参数为什么输出size是1，使用Integer类型size就是3。

通过源码发现：

```java
public static <T> List<T> asList(T... a) {  
    return new ArrayList<T>(a);  
} 
```

```
    asList接收的是一个泛型变长参数，而我们知道基本类型是不能泛型化的，就是说8种基本类型不能作为泛型参数，要想作为泛型参数就要使用其所对应的包装类。
    但是listA的Size为什么是1呢，这是因为listA传递的是一个int类型的数组，数组是一个对象，它是可以泛型化的，也就是说例子中是把一个int类型的数组作为了T的类型，所以转换后在List中就只有一个类型为int数组的元素。
    listA1进行了自动打包，listB本来就是包装类型。
```

(2)asList()方法返回对象使用add（）方法抛出异常:

```java
public class ArraysAsListTest {
	
	public static void main(String[] args) {
		List<String> pets = Arrays.asList("cat","dog");
		pets.add("what");
	}
}
```

```
Exception in thread "main" java.lang.UnsupportedOperationException
	at java.util.AbstractList.add(Unknown Source)
	at java.util.AbstractList.add(Unknown Source)
	at ArraysAsListTest.main(ArraysAsListTest.java:9)
```

Arrays的asList方法使用的ArrayList类是一个内部定义的类，而不是java.util.ArrayList类。所以不能进行添加，删除等集合操作.

(3)改变list的顺序，然而数组顺序却也发生了变化:

```java
public class ArraysAsListTest {
	
	public static void main(String[] args) {
		String[] test = {"a","b","c","d","e"};
		List<String> testList = Arrays.asList(test);
		System.out.println("list原始顺序:"+testList);
		//洗牌打乱list中元素顺序 使用Collections.shuffled方法
		Collections.shuffle(testList, new Random(2));
		System.out.println("list打乱后顺序："+testList);
		//list顺序打乱后 原数组会发生什么？？？
		System.out.println("list打乱顺序后数组内容："+Arrays.toString(test));
		/*
		 * 	list原始顺序:[a, b, c, d, e]
			list打乱后顺序：[e, a, c, b, d]
			list打乱顺序后数组内容：[e, a, c, b, d]
		 */
	}
}
```

3、如果要对asList得到的对象使用add、remove方法可以使用如下解决办法:

```java
List<String> pets = new ArrayList<String>(Arrays.asList("a", "b", "c"));
```


