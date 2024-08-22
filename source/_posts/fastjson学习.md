---
title: fastjson学习
date: 2024-08-22 11:03:18
tags: fastjson
categories: java工具类相关
---

### JSON概述

```
json是一种轻量级的数据交换格式。它基于 ECMAScript(欧洲计算机协会制定的js规范)的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。
```

#### JSON的数据结构

```
值（value）可以是花括号{ }括起来的字符串（String）、数值(Number)、布尔值(true/false)、 null、对象（Object）或者数组（Array），这些结构可以嵌套。
```



##### 1、对象

```
对象（object）是由键值对组成的无序集合，键是字符串，值可以是任何类型，包括对象和数组。对象由一对花括号{ }包围，键和值之间用冒号:分隔。键值对之间用逗号,分隔。
```

```json
{
    "list" : [ 
        {
            "name": "rsb",
            "ID": 1
        }
    ] 
}
```

##### 2、数组

```
数组（array）是值（value）的有序集合，每个值可以是任何类型，包括对象和数组。数组由一对方括号[ ]包裹，值之间用逗号,分隔。
```

```json
[  "apple", 1,  "orange"]
```

##### 注意事项

```json
JSON的字符串必须使用双引号" "，而不能使用单引号' '。
JSON的值可以是字符串、数字、布尔值、null、对象或数组。
JSON的键必须是字符串，必须使用双引号" "。
JSON的格式必须严格遵守规定，否则可能会导致解析错误。
```

##### 优点

```java
与 JavaScript 完美兼容：JSON 格式源自 JavaScript，因此它与 JavaScript 非常兼容，并且易于在 JavaScript 应用程序中使用。
轻量级：JSON 格式非常轻量级，因此它非常适合在网络中传输大量数据。相比于 XML，它的数据量更小，传输速度更快。
多语言支持：JSON 格式是一种跨语言的格式，它可以被许多不同语言的程序支持，易于解析和生成，几乎所有编程语言都支持 JSON。
易于阅读和编辑：JSON 格式非常易于阅读和编写，因为它具有简单的语法和结构，格式清晰，具有良好的可读性。
```

##### 缺点

```java
不支持注释： JSON 格式不支持注释，这可能使得编写和维护复杂数据时变得有些困难。
无法表示二进制数据： JSON 格式无法直接表示二进制数据，这意味着它无法用于存储和传输图像、视频等二进制数据。
不支持命名空间： JSON 不支持命名空间，这可能会导致不同的应用程序之间发生命名冲突。
不支持DTD： JSON 不支持 DTD，这使得在验证 JSON 数据格式时不太方便。
```



### FastJson

#### 1、概述

```java
   FastJson 是阿里巴巴的开源JSON解析库,它可以解析 JSON 格式的字符串，支持将 Java Bean 序列化为 JSON 字符串，也可以从 JSON 字符串反序列化到 JavaBean。
```

#### 2、序列化API

```java
com.alibaba.fastjson.JSON
```

##### (1)对象序列化成json字符串

```java
package com.ransibi.fastjson.pojo;
import com.alibaba.fastjson.annotation.JSONField;
import lombok.Data;
import java.util.Date;

@Data
public class Student {
    private Integer id;

    //@JSONField 注解属性 name ,指定序列化后的名字
    //@JSONField 注解属性 ordinal ,指定序列化后的字段顺序  属性值越小,顺序靠前
    @JSONField(name = "studentName",ordinal = 1)
    private String name;

    @JSONField(ordinal = 2)
    private Integer age;

    //@JSONField 注解属性 serialize ,指定是否序列化该字段
    @JSONField(serialize = false)
    private String email;

    //@JSONField 注解属性 format 指定序列化后的格式
    @JSONField(format = "YYYY-MM-dd")
    private Date birthday;
    private Boolean flag;
}
```

```java
    public static void main(String[] args) {
        //对象转json字符串
        Student student = new Student();
        student.setId(1);
        student.setName("张三");
        student.setAge(20);
        student.setEmail("xxxx.com");
        String jsonString = JSON.toJSONString(student);
        System.out.println(jsonString);
    }
```

```json
{"id":1,"studentName":"张三","age":20}
```

##### (2)集合序列化成json字符串

```java
    public static void main(String[] args) {
        //List转json字符串
        Student student = new Student();
        student.setId(1);
        student.setName("张三");
        student.setAge(20);
        student.setEmail("1111.com");

        Student student2 = new Student();
        student2.setId(2);
        student2.setName("张三1");
        student2.setAge(22);
        student2.setEmail("2222.com");

        List<Student> list = new ArrayList<Student>();
        list.add(student);
        list.add(student2);
        String jsonString = JSON.toJSONString(list);
        System.out.println(jsonString);
    }
```

```json
[{"id":1,"studentName":"张三","age":20},{"id":2,"studentName":"张三1","age":22}]
```

```json
{"s1":{"id":1,"studentName":"张三","age":20},"s2":{"id":2,"studentName":"张三1","age":22}}
```



#### 3、反序列化API

##### 1、字符串反序列化成对象

```java
        String jsonString = "{\"address\":\"北京市\",\"age\":20,\"email\":\"xxxx.com\",\"id\":1,\"name\":\"张三\"}";
        FStudent student = JSON.parseObject(jsonString, FStudent.class);
        System.out.println(student);
```

```json
Student(id=1, name=张三, age=20, email=xxxx.com, birthday=null, flag=null)
```

##### 2、字符串反序列化成集合

```java
        String jsonString = "[{\"address\":\"北京市\",\"age\":20,\"email\":\"zs@sina.com\",\"id\":1,\"name\":\"张三\"},{\"address\":\"北京市2\",\"age\":22,\"email\":\"zs2@sina.com\",\"id\":2,\"name\":\"张三2\"}]";
        List<FStudent> list = JSON.parseArray(jsonString,FStudent.class);
        System.out.println(list);
```

```json
[FStudent(id=1, name=张三, age=20, email=zs@sina.com, birthday=null, flag=null), FStudent(id=2, name=张三2, age=22, email=zs2@sina.com, birthday=null, flag=null)]
```

```java
        String jsonString = "{\"s1\":{\"address\":\"北京市\",\"age\":20,\"email\":\"zs@sina.com\",\"id\":1,\"name\":\"张三\"},\"s2\":{\"address\":\"北京市2\",\"age\":22,\"email\":\"zs2@sina.com\",\"id\":2,\"name\":\"张三2\"}}";
        Map<String, FStudent> map = JSON.parseObject(jsonString,new TypeReference<Map<String,FStudent>>(){});
        System.out.println(map);
```

```json
{s1=FStudent(id=1, name=张三, age=20, email=zs@sina.com, birthday=null, flag=null), s2=FStudent(id=2, name=张三2, age=22, email=zs2@sina.com, birthday=null, flag=null)}
```



#### SerializerFeature枚举

(1)枚举常量 WriteMapNullValue 序列化为null的字段

(2)枚举常量 WriteNullStringAsEmpty 字段为null,序列化为""

```JAVA
        FStudent student = new FStudent();
        student.setId(1);
//        student.setName("张三");
        student.setAge(20);
        student.setEmail("zs@sina.com");
        String jsonString = JSON.toJSONString(student, SerializerFeature.WriteNullStringAsEmpty);
        System.out.println(jsonString);
```

```
{"age":20,"id":1,"name":""}
```

(3)枚举常量 WriteNullNumberAsZero 字段为null,序列化为0

```java
        FStudent student = new FStudent();
        student.setId(1);
        student.setName("张三");
        //student.setAge(20);
        student.setEmail("zs@sina.com");
        String jsonString = JSON.toJSONString(student, SerializerFeature.WriteNullNumberAsZero);
        System.out.println(jsonString);
```

```
{"age":0,"id":1,"name":"张三"}
```

(4)枚举常量 WriteNullBooleanAsFalse 字段值为null 输出false

(5)枚举常量 WriteDateUseDateFormat  格式化日期格式

(6)枚举常量 PrettyFormat格式化输出

```java
        String jsonString = JSON.toJSONString(person,SerializerFeature.WriteNullBooleanAsFalse,
                SerializerFeature.WriteDateUseDateFormat,SerializerFeature.PrettyFormat);
```

#### 注解

##### @JSonField注解

```
该注解作用于方法上,字段上和参数上.可在序列化和反序列化时进行特性功能定制.
- 注解属性 : name 序列化后的名字
- 注解属性 : ordinal序列化后的顺序
- 注解属性 :  format 序列化后的格式
- 注解属性 :  serialize 是否序列化该字段
- 注解属性 : deserialize 是否反序列化该字段
- 注解属性 : serialzeFeatures 序列化时的特性定义
```

##### @ JSonType注解

```
该注解作用于类上,对该类的字段进行序列化和反序列化时的特性功能定制.
- 注解属性 : includes 要被序列化的字段.
- 注解属性 : orders 序列化后的顺序.
- 注解属性 : serialzeFeatures 序列化时的特性定义.
```

