---
title: java8Stream详解
date: 2022-11-23 15:28:14
tags: java8
categories: java8相关

---

# JDK8 Stream

## 概念

> Stream是Java8 API的新成员，它允许以声明性方式处理数据集合 。

## 特点

> （1）**代码简洁**：函数式编程写出的代码简洁且意图明确，使用*stream*接口让你从此告别*for*循环。 
>
> （2）**多核友好**：Java函数式编程使得编写并行程序从未如此简单，你需要的全部就是调用一下方法。 

## 流程

```
1）第一步：把集合转换为流stream
2）第二步：操作stream流
stream流在管道中经过中间操作（intermediate operation）的处理，最后由最终操作(terminal operation)得到前面处理的结果
```

## 操作符

> 两种：中间操作符、终止操作符

注意: 只有执行了Stream的终止操作，中间的过滤、查找等操作才会执行。

### 中间操作符

#### filter

用于通过设置的条件过滤出元素

```java
        //基本数据类型
        List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd", "", "jkl");
        List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
        System.out.println(filtered);
```

```
[abc, bc, efg, abcd, jkl]
```



```java
package com.ransibi.java8;

import lombok.Data;

@Data
public class TestBean {
    private String role;
    private String name;
    private String file;
    private String path;
}
```



```java
       //List<Bean>
       List<TestBean> testBeanList = new ArrayList<>();
        TestBean testBean = new TestBean();
        testBean.setRole("1");
        testBean.setName("测试1");
        testBean.setFile("***.xml");
        testBean.setPath("home/test/***.xml");
        testBeanList.add(testBean);
        TestBean testBean1 = new TestBean();
        testBean1.setRole("2");
        testBean1.setName("测试2");
        testBean1.setFile("***.xml");
        testBean1.setPath("home/test/***.xml");
        testBeanList.add(testBean1);
        TestBean testBean2 = new TestBean();
        testBean2.setRole("3");
        testBean2.setName("测试3");
        testBean2.setFile("111.xml");
        testBean2.setPath("home/test/***.xml");
        testBeanList.add(testBean2);

        List<TestBean> filterLst = testBeanList.stream()
                .filter(obj ->obj.getFile().equals("***.xml"))
                .collect(Collectors.toList());
        System.out.println(filterLst);
```

```
[TestBean(role=1, name=测试1, file=***.xml, path=home/test/***.xml), TestBean(role=2, name=测试2, file=***.xml, path=home/test/***.xml)]
```

#### distinct

返回一个元素各异（根据流所生成元素的hashCode和equals方法实现）的流。

```
        //基本数据类型
        List<String> strings = Arrays.asList("abc", "abc", "bc", "efg", "abcd","jkl", "jkl","");
        List<String> distincted = strings.stream().distinct().collect(Collectors.toList());
        System.out.println(distincted);
```

```java
        List<TestBean> testBeanList = new ArrayList<>();
        TestBean testBean = new TestBean();
        testBean.setRole("1");
        testBean.setName("测试1");
        testBean.setFile("***.xml");
        testBean.setPath("home/test/***.xml");
        testBeanList.add(testBean);
        TestBean testBean1 = new TestBean();
        testBean1.setRole("2");
        testBean1.setName("测试1");
        testBean1.setFile("***.xml");
        testBean1.setPath("home/test/***.xml");
        testBeanList.add(testBean1);
        TestBean testBean2 = new TestBean();
        testBean2.setRole("1");
        testBean2.setName("测试1");
        testBean2.setFile("***.xml");
        testBean2.setPath("home/test/***.xml");
        testBeanList.add(testBean2);
        List<TestBean> distinctCollect = testBeanList.stream().distinct().collect(Collectors.toList());
        System.out.println(distinctCollect);
```

```
[TestBean(role=1, name=测试1, file=***.xml, path=home/test/***.xml), TestBean(role=2, name=测试1, file=***.xml, path=home/test/***.xml)]
```

注意: 如何想按某几个属性进行去重，需要重写equals()和hashCode()方法，比如我想让name、file、path字段属性参与去重，那么就重写对应的equals()和hashCode()方法就行:

```java
package com.ransibi.java8;

import lombok.Data;

import java.util.Objects;

@Data
public class TestBean {
    private String role;
    private String name;
    private String file;
    private String path;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        TestBean testBean = (TestBean) o;
        return Objects.equals(name, testBean.name) && Objects.equals(file, testBean.file) && Objects.equals(path, testBean.path);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, file, path);
    }
}
```

```
[TestBean(role=1, name=测试1, file=***.xml, path=home/test/***.xml)]
```

#### limit

返回一个不超过给定长度的流。指定获取集合前x条数据，重新构造一个新的集合。

```java
        
        List<String> strings = Arrays.asList("abc", "abc", "bc", "efg", "abcd","jkl", "jkl");
        List<String> limited = strings.stream().limit(3).collect(Collectors.toList());
        System.out.println(limited);
```

```
[abc, abc, bc]
```

#### skip

返回一个扔掉了前n个元素的流。排除集合前x条数据，把后面的数据重新构造一个新的集合。

```java
        List<String> strings = Arrays.asList("abc", "abc", "bc", "efg", "abcd","jkl", "jkl");
        List<String> skiped = strings.stream().skip(3).collect(Collectors.toList());
        System.out.println(skiped);
```

```
[efg, abcd, jkl, jkl]
```

#### map

接收一个函数作为参数。这个函数会被应用到每个元素上，并将其映射成一个新的元素（使用映射一词，是因为它和转换类似，但其中的细微差别在于它是“创建一个新版本”而不是去“修改”）。对集合中所有元素统一处理。

```java
        //基本数据类型
        List<String> strings = Arrays.asList("abc", "abc", "bc", "efg", "abcd","jkl", "jkl");
        List<String> mapped = strings.stream().map(str->str+"-测试").collect(Collectors.toList());
        System.out.println(mapped);
```

```
[abc-测试, abc-测试, bc-测试, efg-测试, abcd-测试, jkl-测试, jkl-测试]
```

```java
        List<TestBean> testBeanList = new ArrayList<>();
        TestBean testBean = new TestBean();
        testBean.setRole("1");
        testBean.setName("测试1");
        testBean.setFile("***.xml");
        testBean.setPath("home/test/***.xml");
        testBeanList.add(testBean);
        TestBean testBean1 = new TestBean();
        testBean1.setRole("2");
        testBean1.setName("测试2");
        testBean1.setFile("***.xml");
        testBean1.setPath("home/test/***.xml");
        testBeanList.add(testBean1);
        TestBean testBean2 = new TestBean();
        testBean2.setRole("3");
        testBean2.setName("测试3");
        testBean2.setFile("***.xml");
        testBean2.setPath("home/test/***.xml");
        testBeanList.add(testBean2);

        List<String> collect = testBeanList.stream().map(TestBean::getName).collect(Collectors.toList());
        System.out.println(collect);
```

```
[测试1, 测试2, 测试3]
```



#### flatMap

使用flatMap方法的效果是，各个数组并不是分别映射成一个流，而是映射成流的内容。所有使用map(Arrays::stream)时生成的单个流都被合并起来，即扁平化为一个流。

应用场景: 

- 嵌套集合展平：将嵌套的集合转换为单一的流。
- 一对多映射：将流中的每个元素转换为多个元素的流。

(1)嵌套集合展平

```java
        List<List<String>> nestedList = Arrays.asList(
                Arrays.asList("apple", "banana"),
                Arrays.asList("cherry", "date"),
                Arrays.asList("elderberry", "fig", "grape")
        );
        //将嵌套的列表展平为一个包含所有元素的单一列表
        List<String> flatList = nestedList.stream()
                .flatMap(List::stream)
                .collect(Collectors.toList());
        System.out.println(flatList);
```

```
[apple, banana, cherry, date, elderberry, fig, grape]
```

(2)一对多映射

```java
        List<String> words = Arrays.asList("apple", "banana", "cherry");
        List<String> characters = words.stream()
                .flatMap(word -> Arrays.stream(word.split("")))
                .collect(Collectors.toList());
        System.out.println(characters);
```

```
[a, p, p, l, e, b, a, n, a, n, a, c, h, e, r, r, y]
```

(3)处理复杂对象

​          假设有一个包含用户对象的列表，每个用户都有一个包含多个地址的列表，我们希望提取所有用户的所有地址，并将结果收集到一个

新的列表中。

```java
package com.ransibi.java8;

import lombok.Data;

import java.util.List;

@Data
public class UserBean {
    private String name;
    private List<String> address;

    public UserBean(String name, List<String> address) {
        this.name = name;
        this.address = address;
    }
}
```

```java
        List<UserBean> userBeanList = new ArrayList<>();
        UserBean userBean = new UserBean("小美", Arrays.asList("北京","贵州"));
        UserBean userBean1 = new UserBean("小王", Arrays.asList("上海","湖南"));
        UserBean userBean2 = new UserBean("小明", Arrays.asList("浙江","广东"));
        userBeanList.add(userBean);
        userBeanList.add(userBean1);
        userBeanList.add(userBean2);
        List<String> collect = userBeanList.stream().flatMap(user -> user.getAddress().stream()).collect(Collectors.toList());
        System.out.println(collect);
```

```
[北京, 贵州, 上海, 湖南, 浙江, 广东]
```

注意事项:

```
Function 的实现：flatMap 方法依赖于 Function 接口的实现，因此实现的逻辑需要准确无误，以确保转换结果正确。
流的扁平化：flatMap 方法不仅会转换流中的元素，还会将多个流合并为一个单一的流，从而消除嵌套结构。
性能影响：对于大型数据集，频繁使用复杂的 flatMap 操作可能影响性能，应尽量优化转换逻辑。
```



#### sorted

返回排序后的流

```java
        List<String> strings1 = Arrays.asList("abc", "abd", "aba", "efg", "abcd", "jkl", "jkl");
        List<String> strings2 = Arrays.asList("张三", "李四", "王五", "赵柳", "张哥", "李哥", "王哥");
        List<Integer> strings3 = Arrays.asList(10, 2, 30, 22, 1, 0, -9);
        List<String> sorted1 = strings1.stream()
                .sorted()
                .collect(Collectors.toList());
        List<String> sorted2 = strings2.stream()
                .sorted(Collections.reverseOrder(Collator.getInstance(Locale.CHINA)))
                .collect(Collectors.toList());
        List<Integer> sorted3 = strings3.stream()
                .sorted()
                .collect(Collectors.toList());
        System.out.println(sorted1);
        System.out.println(sorted2);
        System.out.println(sorted3);
```

```
[aba, abc, abcd, abd, efg, jkl, jkl]
[赵柳, 张三, 张哥, 王五, 王哥, 李四, 李哥]
[-9, 0, 1, 2, 10, 22, 30]
```

#### Map与flatMap区别

```
map：对流中每一个元素进行处理
flatMap：流扁平化，让你把一个流中的“每个值”都换成另一个流，然后把所有的流连接起来成为一个流 
总结：map是对一级元素进行操作，flatmap是对二级元素操作。
```

`本质区别`：map返回一个值；flatmap返回一个流，多个值。

`应用场景`：map对集合中每个元素加工,返回加工后结果；flatmap对集合中每个元素加工后，做扁平化处理（拆分层级，放到同一层）然后返回。

### 终止操作符

#### anyMatch

判断集合中是否至少存在一个元素满足条件，返回boolean。

```java
        List<String> strings = Arrays.asList("abc", "abd", "aba", "efg", "abcd","jkl", "jkl");
        boolean b = strings.stream().anyMatch(s -> s.equals( "abc"));
        System.out.println(b);
```

```
true
```

#### allMatch

判断集合中是否所有元素都满足条件，返回boolean。

```java
        List<String> strings = Arrays.asList("abc", "abd", "aba", "efg", "abcd","jkl", "jkl");
        boolean b = strings.stream().allMatch(s -> s.equals( "abc"));
        System.out.println(b);
```

```
false
```

#### noneMatch

判断集合中是否所有元素都不满足条件，返回boolean。

```java
        List<String> strings = Arrays.asList("abc", "abd", "aba", "efg", "abcd", "jkl", "jkl");
        boolean b = strings.stream().noneMatch(s -> s.equals("abc"));
        System.out.println(b);
```

```
false
```

#### findAny

返回当前流中任意元素。

```java
        List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
        Optional<String> any = strings.stream().findAny();
        if(any.isPresent()){
            System.out.println(any.get());
        }
```

```
cv
```

#### findFirst

返回当前流中第一个元素。

```java
        List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
        Optional<String> first = strings.stream().findFirst();
        if(first.isPresent()){
            System.out.println(first.get());
        }
```

```
cv
```

#### forEach

遍历流。

```java
        List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
        strings.stream().forEach(s -> System.out.println(s));
```

```
cv
abd
aba
efg
abcd
jkl
jkl
```

#### collect

收集器，将流转换为其他形式。

```java
        List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
        Set<String> set = strings.stream().collect(Collectors.toSet());
        List<String> list = strings.stream().collect(Collectors.toList());
        Map<String, String> map = strings.stream().collect(Collectors.toMap(v ->v.concat("_name"), v1 -> v1, (v1, v2) -> v1));
        System.out.println(set);
        System.out.println(list);
        System.out.println(map);
```

```
[aba, abd, cv, efg, jkl, abcd]
[cv, abd, aba, efg, abcd, jkl, jkl]
{abd_name=abd, aba_name=aba, abcd_name=abcd, cv_name=cv, efg_name=efg, jkl_name=jkl}
```

#### reduce

将流中元素反复结合起来，得到一个值。

```java
        List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd", "jkl", "jkl");
        //reduce方法一
        Optional<String> reduce1 = strings.stream().reduce((acc, item) -> {
            return acc + item;
        });
        //reduce方法二
        String reduce2 = strings.stream().reduce("test", (acc, item) -> {
            return acc + item;
        });
        System.out.println(reduce1);
        System.out.println(reduce2);
```

```
Optional[cvabdabaefgabcdjkljkl]
testcvabdabaefgabcdjkljkl
```

#### count

返回流中元素总数。

```java
        List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd", "jkl", "jkl");
        long count = strings.stream().count();
        System.out.println(count);
```

```
7
```

## 常用技巧

### List转Map

使用Collectors.toMap()方法，可以将List转成Map。

```java
        List<User> userList = new ArrayList<>();
        userList.add(new User("张三","男", 20));
        userList.add(new User("李四","女", 25));
        userList.add(new User("王五","女", 28));
        userList.add(new User("王五","男", 28));

        /**
         * List转Map
         */
        //适用于name唯一的情况，如果key有重复的，转换回出错
//        Map<String, User> map = userList.stream().collect(Collectors.toMap(User::getName, k -> k));
        //如果有重复的，使用需要加(k1, k2) -> k1 , 表示，如果有重复的key,则保留第一个，舍弃第二个
        Map<String, User> map1 = userList.stream().collect(Collectors.toMap(User::getName, user -> user, (k1, k2) -> k1));
        System.out.println(map1);
```

### groupingBy分组

使用Collectors.groupingBy()方法，对List进行分组。

```java
List<User> userList = new ArrayList<>();
userList.add(new User("张三", "男", 20));
userList.add(new User("李四", "女", 25));
userList.add(new User("王五", "女", 20));

//按性别进行分组
Map<String, List<User>> groupBySexMap = userList.stream().collect(Collectors.groupingBy(User::getSex));
//{女=[User{name='李四', sex='女', age=25}, User{name='王五', sex='女', age=28}], 男=[User{name='张三', sex='男', age=20}]}
System.out.println(groupBySexMap);

Map<Integer, List<User>> groupByAgeMap = userList.stream().collect(Collectors.groupingBy(User::getAge));
//{20=[User{name='张三', sex='男', age=20}, User{name='王五', sex='女', age=20}], 25=[User{name='李四', sex='女', age=25}]}
System.out.println(groupByAgeMap);
```

### sorted+Comparator排序

```java
List<User> userList = new ArrayList<>();
userList.add(new User("张三", "男", 20));
userList.add(new User("李四", "女", 25));
userList.add(new User("王五", "女", 20));

/**
 * sorted + Comparator实现排序
 */
//默认是升序
userList = userList.stream().sorted(Comparator.comparing(User::getAge)).collect(Collectors.toList());
//[User{name='张三', sex='男', age=20}, User{name='王五', sex='女', age=20}, User{name='李四', sex='女', age=25}]
System.out.println(userList);
//降序
userList = userList.stream().sorted(Comparator.comparing(User::getAge, Comparator.reverseOrder())).collect(Collectors.toList());
//[User{name='李四', sex='女', age=25}, User{name='张三', sex='男', age=20}, User{name='王五', sex='女', age=20}]
System.out.println(userList);

//按中文拼音首字母排序a(A) -> z(Z)
Collator collator = Collator.getInstance(Locale.CHINA);
userList = userList.stream().sorted(Comparator.comparing(User::getName, collator)).collect(Collectors.toList());
//[User{name='李四', sex='女', age=25}, User{name='王五', sex='女', age=20}, User{name='张三', sex='男', age=20}]
System.out.println(userList);

//按中文拼音首字母排序z(Z) -> a(A)
userList = userList.stream().sorted(Comparator.comparing(User::getName, Collections.reverseOrder(collator))).collect(Collectors.toList());
//[User{name='张三', sex='男', age=20}, User{name='王五', sex='女', age=20}, User{name='李四', sex='女', age=25}]
System.out.println(userList);

//按多个属性排序
userList = userList.stream().sorted(Comparator.comparing(User::getAge).thenComparing(User::getName, collator)).collect(Collectors.toList());
//[User{name='王五', sex='女', age=20}, User{name='张三', sex='男', age=20}, User{name='李四', sex='女', age=25}]
System.out.println(userList);
```

### Max+Min获取最值

```java
//求最大值
Optional<User> max = userList.stream().max(Comparator.comparing(User::getAge));
//User{name='李四', sex='女', age=25}
max.ifPresent(obj -> System.out.println(obj));

//求最小值
Optional<User> min = userList.stream().min(Comparator.comparing(User::getAge));
//User{name='王五', sex='女', age=20}
min.ifPresent(obj -> System.out.println(obj));
```
