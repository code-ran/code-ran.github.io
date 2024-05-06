---
title: springboot读取yml配置文件的几种方式
date: 2022-11-23 15:28:14
tags: springboot
categories: springboot相关

---

#### 1、springboot读取yml配置文件的几种方式:

##### 一、yml文件YAML语法基本介绍

```
        1、以缩进代表层级关系
        2、缩进不能使用tab，只能用空格
        3、空格个数不重要，但是同一层级必须左对齐
        4、大小写敏感
        5、数据格式为，名称:(空格)值
        6、注释单行用#，只能注释单行
```



##### 各类数据书写形式：

###### 1、数字，字符串，布尔等

```yml
# key: value形式
name: zs
age: 20
enable: true
```

###### 2、数组，集合（List，Set）

```yml
# 方式一：换行缩进，使用-表示一个元素（常用）
# 如Java bean中的List<String> tels 或者 String[] tels
tels: 
  - 110
  - 120
# List<Users> users
users: 
  - name: zs
    age: 20
  - name: ls
    age: 30
# 方式二：单行配置
tels: 110, 120
users: {name: zs, age: 20}, {name: ls, age: 30}
```

###### 3、Map和对象

```yml
# 方式一：多行配置
# 如Map<String, Integer> score或者Score score
score:
  chinese: 95
  english: 90
# 方式二：单行配置
score: {chinese: 95, english: 90}
```



##### 二、读取yml文件：

先在application.yml中写好相关配置：

```yml
user:
  user-name: Mr.Zhang
  age: 38
  tels:
    - 110
    - 120
  score:
    chinese: 95
    english: 90
  users:
    - user-name: zs
      age: 5
    - user-name: ls
      age: 12
```

###### 方式一： @Value注解（值绑定）

定义一个JavaBean，提供get和set方法（必须），用于接收从yml读取的数据

```java
@Data
@Component
public class Users {

    /**
     * @Value 值绑定
     * 在每个对应的属性上加@Value注解，注解中的内容填写yml中对应的属性，
     * 使用@Value注解时，yml中的属性名和JavaBean中的属性名可以不对应，但是数据类型需要对应
     */
    @Value("${user.userName}")
    private String name;
    @Value("${user.age}")
    private Integer age;
    @Value("${user.tesl}")
    private String[] tels;
    @Value("${user.score}")
    private Map<String, Integer> score;
    @Value("${user.users}")
    private List<Users> children;
    
}
```



###### 方式二： @ConfigurationProperties注解（松散绑定）

定义一个JavaBean，提供get和set方法，用于接收从yml读取的数据

```java
/**
 * ConfigurationProperties 松散绑定，只需要通过prefix属性指定属性参数名的前缀即可
 * 松散绑定的bean属性名需要跟yml中的属性名对应，但是不需要精确匹配，
 * yml可依据约定使用驼峰（如：userName）、"-"线（如：user-name）、全大写加上"_"（如：USER_NAME）
 */
@Data
@Component
@ConfigurationProperties(prefix = "user")
public class Users {

    private String userName;
    private Integer age;
    private String[] tels;
    private Map<String, Integer> score;
    private List<Teacher> oldTeachers;
    private List<Teacher> newTeachers;

}
```

#### 单元测试：

```java
// 在单元测试中注入这个bean
@Autowired
private Users user;

@Test
void testUsers(){
    System.out.println(user.getName());
    System.out.println(user.getAge());
}
```

结果：

```
Mr.Zhang
38
```

###### 方式三： YamlMapFactoryBean

通过这个factoryBean可以直接返回一个Map，而不需要使用JavaBean去接收yml读取的值

```java
public static void main(String[] arg0){
    YamlMapFactoryBean yamlMapFactoryBean = new YamlMapFactoryBean();
    //可以加载多个yml文件
    yamlMapFactoryBean.setResources(new ClassPathResource("application.yml"));
    //通过getObject()方法获取Map对象
    Map<String, Object> map = yamlMapFactoryBean.getObject();
    System.out.println(map);
    map.keySet().forEach(item -> {
        //可以将map中的值强转为LinkedHashMap对象
        LinkedHashMap<String, Object> linkedHashMap = (LinkedHashMap<String, Object>) (map.get(item));
        System.out.println(linkedHashMap.get("tels"));
    });
}
```

输出：

```
{user={user-name=zhangsan, age=18, tels=[13479990123, 13567890987, null], score={chinese=95, english=90}, users=[{name=Mr.Zhang, age=29}, {name=Mr.Li, age=39}]}}
[13479990123, 13567890987]
```

###### 方式四：YamlPropertiesFactoryBean

通过这个类可以直接返回Properties对象

```java
public static void main(String[] arg0){
    YamlPropertiesFactoryBean factoryBean = new YamlPropertiesFactoryBean();
    factoryBean.setResources(new ClassPathResource("application.yml"));
    Properties properties = factoryBean.getObject();
    properties.keySet().forEach(key -> System.out.println(key + "=====" + properties.get(key)));
}
```

输出：

```
user.age=====18
user.users[0].age=====5
user.users[0].user-name=====zs
user.users[1].age=====15
user.users[1].user-name=====ls
user.score.chinese=====95
user.score.english=====90
user.tels[0]=====13479990123
user.tels[1]=====13567890987
user.user-name=====zhangsan
```