---
title: java8Stream详解
date: 2022-11-23 15:28:14
tags: java8
categories: java8相关

---

# JDK8 Stream详解

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





> 
>
> 7）sorted
>
> ```java
> /**
>  * 功能描述 : 对集合进行排序
>  * @return : void
>  */
> @Test
> public void sorted(){
>     List<String> strings1 = Arrays.asList("abc", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     List<String> strings2 = Arrays.asList("张三", "李四", "王五", "赵柳", "张哥","李哥", "王哥");
>     List<Integer> strings3 = Arrays.asList(10, 2, 30, 22, 1,0, -9);
>     List<String> sorted1 = strings1.stream()
>                                    .sorted()
>                                   .collect(Collectors.toList());
>     List<String> sorted2 = strings2.stream()
>                                   .sorted(Collections.reverseOrder(Collator.getInstance(Locale.CHINA)))
>                                    .collect(Collectors.toList());
>     List<Integer> sorted3 = strings3.stream()
>                                     .sorted()
>                                     .collect(Collectors.toList());
>     System.out.println(sorted1);
>     System.out.println(sorted2);
>     System.out.println(sorted3);
> }
> ```
> 
>  Map、flatMap区别

```
map：对流中每一个元素进行处理
flatMap：流扁平化，让你把一个流中的“每个值”都换成另一个流，然后把所有的流连接起来成为一个流 
总结：map是对一级元素进行操作，flatmap是对二级元素操作。
```

`本质区别`：map返回一个值；flatmap返回一个流，多个值。

`应用场景`：map对集合中每个元素加工,返回加工后结果；flatmap对集合中每个元素加工后，
做扁平化处理后（拆分层级，放到同一层）然后返回

```java
/**
 * 方法一
 * 功能描述:  通过使用map、flatMap把字符串转换为字符输出对比区别
 * @return : void
 */
@Test
public void flatMap2Map(){
    List<String> strings = Arrays.asList("abc", "abc", "bc", "efg", "abcd","jkl", "jkl");
    final Stream<Character> flatMap = strings.stream()
                                             .flatMap(Java8StreamTest::getCharacterByString);
    flatMap.forEach(System.out::println);
    //----------------------------------------------
    final Stream<Stream<Character>> mapStream = strings.stream()
                                                       .map(Java8StreamTest::getCharacterByString);
    //mapStream.forEach(System.out::println);
    out.println("------------------------------------------------");
    mapStream.forEach(
        stream-> {stream.forEach(character->{System.out.println(character);});}
        );

}
```

> 公共方法（字符串转换为字符流）

```java
/**
* 功能描述:字符串转换为字符流
* @param str
* @return : java.util.stream.Stream<java.lang.Character>
*/
public static Stream<Character> getCharacterByString(String str) {
    List<Character> characterList = new ArrayList<>();
    for (Character character : str.toCharArray()) {
    	characterList.add(character);
    }
    return characterList.stream();
}
```

### 终止操作符

| 流方法    | 含义                                     | 示例                                                         |
| --------- | ---------------------------------------- | ------------------------------------------------------------ |
| anyMatch  | 检查是否至少匹配一个元素，返回boolean。  | List<String> strings = Arrays.asList("abc", "abd", "aba", "efg", "abcd","jkl", "jkl");boolean b = strings.stream().anyMatch(s -> s == "abc"); |
| allMatch  | 检查是否匹配所有元素，返回boolean。      | List<String> strings = Arrays.asList("abc", "abd", "aba", "efg", "abcd","jkl", "jkl");boolean b = strings.stream().allMatch(s -> s == "abc"); |
| noneMatch | 检查是否没有匹配所有元素，返回boolean。  | List<String> strings = Arrays.asList("abc", "abd", "aba", "efg", "abcd","jkl", "jkl");boolean b = strings.stream().noneMatch(s -> s == "abc"); |
| findAny   | 将返回当前流中的任意元素。               | List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");Optional<String> any = strings.stream().findAny(); |
| findFirst | 返回第一个元素                           | List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");Optional<String> first = strings.stream().findFirst(); |
| forEach   | 遍历流                                   | List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");strings.stream().forEach(s -> out.println(s)); |
| collect   | 收集器，将流转换为其他形式。             | List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");Set<String> set = strings.stream().collect(Collectors.toSet());List<String> list = strings.stream().collect(Collectors.toList());Map<String, String> map = strings.stream().collect(Collectors.toMap(v ->v.concat("_name"), v1 -> v1, (v1, v2) -> v1)); |
| reduce    | 可以将流中元素反复结合起来，得到一个值。 | List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");Optional<String> reduce = strings.stream().reduce((acc,item) -> {return acc+item;});if(reduce.isPresent())out.println(reduce.get()); |
| count     | 返回流中元素总数。                       | List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");long count = strings.stream().count(); |

> 示例代码
>
> 1）anyMatch
>
> ```java
> /**
>  * 功能描述 : 判断集合中是否至少存在一个元素满足条件
>  * @return : void
>  */
> @Test
> public void anyMatch(){
>     List<String> strings = Arrays.asList("abc", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     boolean b = strings.stream().anyMatch(s -> s == "abc");
>     out.println(b);
> }
> ```
>
> 2）allMatch
>
> ```java
> /**
>  * 功能描述 : 判断集合中是否所有元素都满足条件
>  * @return : void
>  */
> @Test
> public void allMatch(){
>     List<String> strings = Arrays.asList("abc", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     boolean b = strings.stream().allMatch(s -> s == "abc");
>     out.println(b);
> }
> ```
>
> 3）noneMatch
>
> ```java
> /**
>  * 功能描述 : 判断集合中是否所有元素都不满足条件
>  * @return : void
>  */
> @Test
> public void noneMatch(){
>     List<String> strings = Arrays.asList("abc", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     boolean b = strings.stream().noneMatch(s -> s == "abc");
>     out.println(b);
> }
> ```
>
> 4）findAny
>
> ```java
> /**
>  * 功能描述 : 返回当前流中任意元素
>  * @return : void
>  */
> @Test
> public void findAny(){
>     List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     Optional<String> any = strings.stream().findAny();
>     if(any.isPresent()) out.println(any.get());
> }
> ```
>
> 5）findFirst
>
> ```java
> /**
>  * 功能描述 : 返回当前流中第一个元素
>  * @return : void
>  */
> @Test
> public void findFirst(){
>     List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     Optional<String> first = strings.stream().findFirst();
>     if(first.isPresent()) out.println(first.get());
> }
> ```
>
> 6）forEach java 
>
> ```java
> /**
>  * 功能描述 : 遍历流
>  * @return : void
>  */
> @Test
> public void foreach(){
>     List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     strings.stream().forEach(s -> out.println(s));
> }
> ```
>
> 7）collect
>
> ```java
> /**
>  * 功能描述 : 流转换为其他形式
>  * @return : void
>  */
> @Test
> public void collect(){
>     List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     Set<String> set = strings.stream().collect(Collectors.toSet());
>     List<String> list = strings.stream().collect(Collectors.toList());
>     Map<String, String> map = strings.stream().collect(Collectors.toMap(v ->v.concat("_name"), v1 -> v1, (v1, v2) -> v1));
>     out.println(set);
>     out.println(list);
>     out.println(map);
> }
> ```
>
> 8）reduce
>
> ```java
> /**
>  * 功能描述 : 将流中元素反复结合起来，得到一个值
>  * @return : void
>  */
> @Test
> public void reduce(){
>     List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     //reduce方法一
>     Optional<String> reduce1 = strings.stream().reduce((acc,item) -> {return acc+item;});
>     //reduce方法二
>     String reduce2 = strings.stream().reduce("itcast", (acc, item) -> {
>     	return acc + item;
>     });
>     //reduce方法三
>     ArrayList<String> reduce3 = strings.stream().reduce(
>         new ArrayList<String>(),
>    		new BiFunction<ArrayList<String>, String, ArrayList<String>>() {
>     		@Override
>     		public ArrayList<String> apply(ArrayList<String> acc, String item) {
>     			acc.add(item);
>     			return acc;
>     		}
>     	}, 
>         new BinaryOperator<ArrayList<String>>() {
>    			@Override
>     		public ArrayList<String> apply(ArrayList<String> acc, ArrayList<String> item) {
>     		return acc;
>     		}
>     	}
>     );
>     if(reduce1.isPresent())out.println(reduce1.get());
>     out.println(reduce2);
>     out.println(reduce3);
> }
> ```
>
> 9）count
>
> ```java
> /**
> * 功能描述 : 返回流中元素总数
> * @return : void
> */
> @Test
> public void count(){
>     List<String> strings = Arrays.asList("cv", "abd", "aba", "efg", "abcd","jkl", "jkl");
>     long count = strings.stream().count();
>     out.println(count);
> }
> ```

注意：文章中因排序部分用到外部比较器，需要导入外部jar包

```xml
<!--apache集合操作工具包-->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-collections4</artifactId>
    <version>4.4</version>
</dependency>
```

