---
title: lombok学习
date: 2024-08-25 01:34:31
tags: lombok
categories: java工具类相关
---

### Lombok

#### 1.概述

​         Project Lombok 是一个 Java 库，可自动插入您的编辑器和构建工具，为您的 Java 增添趣味。再也不用编写另一个 getter 或 equals 方法，只需一个注解，您的类就拥有了一个功能齐全的构建器、自动化您的日志记录变量等等。

#### 2.命名规范

​      一般JavaBean属性以小写字母开头，驼峰命名格式，相应的 getter/setter 方法是 get/set 接上首字母大写的属性名。例如：属性名为userName，其对应的getter/setter 方法是 getUserName/setUserName。

特殊情况:

- 如果属性名的第二个字母大写，那么该属性名直接用作 getter/setter 方法中 get/set 的后部分，就是说大小写不变。例如属性名为uName，方法是getuName/setuName。

- 如果属性名的前两个字母是大写（一般的专有名词和缩略词都会大写），也是属性名直接用作 getter/setter 方法中 get/set 的后部分。例如属性名为URL，方法是getURL/setURL。

- 如果属性名以"is"开头，则getter方法会省掉get，set方法会去掉is。例如属性名为isExist，方法是isExist/setExist。需要注意的是有些开发工具自动生成的getter/setter方法，并没有考虑到上面所说的特例情况，会导致bug的产生。

  ![image-20240825012740592](D:\github\code-ran.github.io\source\_posts\lombok学习.assets\image-20240825012740592.png)

```
log.info("get方法:{}",lomTestBean.isExist());
```

#### 3.原理

![image-20240824235219810](D:\github\code-ran.github.io\source\_posts\lombok学习.assets\image-20240824235219810.png)

例子说明:

依赖

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version>
    <scope>provided</scope>
</dependency>
```

```java
package com.ransibi.lombok;

import lombok.Data;

@Data
public class LomTestBean {
    private String name;
    private Integer age;
    private String sex;
}
```

```java
    public static void main(String[] args) {
        LomTestBean lomTestBean = new LomTestBean();
        lomTestBean.setName("张三");
        lomTestBean.setAge(20);
        lomTestBean.setSex("男");
        System.out.println(lomTestBean);
    }
```

```
LomTestBean(name=张三, age=20, sex=男)
```

@Data注解自动生成了Get、Set、toString、hashCode、equals方法

![image-20240824233620569](D:\github\code-ran.github.io\source\_posts\lombok学习.assets\image-20240824233620569.png)

在编译阶段生效

![image-20240824235112788](D:\github\code-ran.github.io\source\_posts\lombok学习.assets\image-20240824235112788.png)

#### 4.常用注解

#### @Data

用来生成对象中Get、Set、toString、hashCode、equals方法。

#### @Getter

用来生成对于的get方法

#### @Setter

用来生成对于的set方法

#### @ToString

用来生成toString方法@EqualsAndHashCode

#### @AllArgsConstructor和@NoArgsConstructor

用来生成所有属性的有参构造方法和无参构造方法,一般组合使用.

![image-20240825000235770](D:\github\code-ran.github.io\source\_posts\lombok学习.assets\image-20240825000235770.png)

```java
//        LomTestBean lomTestBean = new LomTestBean();
//        lomTestBean.setName("张三");
//        lomTestBean.setAge(20);
//        lomTestBean.setSex("男");
        LomTestBean lomTestBean = new LomTestBean("张三",20,"男");
        System.out.println(lomTestBean);
```

注意:如果想创建指定的某几个函数的有参构造方法，需要自己重写有参构造方法

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class LomTestBean {
    private String name;
    private Integer age;
    private String sex;
    //重写仅包含name和age的有参构造方法
    public LomTestBean(String name, Integer age) {
        this.name = name;
        this.age = age;
    }
}
```

![image-20240825000541891](D:\github\code-ran.github.io\source\_posts\lombok学习.assets\image-20240825000541891.png)

#### @Builder

可以自动生成一个内部 Builder 类，以及对应的构建方法，从而简化代码的编写。

```
链式调用：通过链式调用的方式设置属性值，代码更简洁明了。
可选参数：可以选择性地设置参数，未设置的参数将使用默认值。
不可变对象：可以轻松创建不可变对象，通过 Builder 模式创建的对象是线程安全的。
```

当使用 `@Builder` 时，默认值可能会被忽略。可以使用 `@Builder.Default` 注解来指定默认值

```java
@Builder
@ToString
public class LomTestBean {
    private String name;
    private Integer age;
    @Builder.Default()
    private String sex = "男";
}
```

```java
    public static void main(String[] args) {
        //使用 @Builder 自动生成的 Builder 类来创建对象
        LomTestBean lomTestBean = LomTestBean.builder().name("张三").age(20).build();
        log.info("信息输出:{}", lomTestBean);
    }
```

```
01:06:22.248 [main] INFO com.ransibi.lombok.LomTest - 信息输出:LomTestBean(name=张三, age=20, sex=男)
```

注意:

```
(1)当我们使用@Builder时，Lombok不会自动生成无参构造函数，这可能在某些情况下导致反射调用失败。当你使用Lombok的@Builder注解时，它会自动生成一个静态的内部类，通常被称为Builder类，这个Builder类提供了一种构建对象的方法。但是，@Builder不会自动生成无参构造函数，这可能会导致在通过反射实例化类时出现问题。建议在使用@Builder时总是加上@NoArgsConstructor和@AllArgsConstructor注解

```

#### @Accessors

用来给类中的set方法开启链式调用chain属性,用来指定是否开启set方法链式调用，true-开启，false-关闭

```java
import lombok.Data;
import lombok.experimental.Accessors;

@Data
@Accessors(chain = true)
public class LomTestBean {
    private String name;
    private Integer age;
    private String sex;
}
```

```java
    public static void main(String[] args) {
        LomTestBean lomTestBean = new LomTestBean();
        lomTestBean.setName("张三").setAge(20).setSex("男");
        System.out.println(lomTestBean);
    }
```

#### @Slf4j

用来快速给类中定义一个日志变量。

使用这个注解相当于在这个类中声明了一个日志对象

```java
private static final Logger log = LoggerFactory.getLogger(LomTest.class);
```

```java
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class LomTest {
    public static void main(String[] args) {
        LomTestBean lomTestBean = new LomTestBean();
        lomTestBean.setName("张三").setAge(20).setSex("男");
        log.info("信息输出:{}",lomTestBean);
    }
}
```

```
00:20:56.553 [main] INFO com.ransibi.lombok.LomTest - 信息输出:LomTestBean(name=张三, age=20, sex=男)
```

![image-20240825002146626](D:\github\code-ran.github.io\source\_posts\lombok学习.assets\image-20240825002146626.png)

支持通过{}进行占位。

#### 5.不常用注解

#### @EqualsAndHashCode

用来生成equals和hashCode方法

#### @NonNull

用来设置该字段不可为空，如果为空，会报空指针异常
