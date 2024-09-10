---
title: springmvc
date: 2024-08-29 07:22:47
tags: springmvc
categories: springmvc相关
---

### SpringMVC概述

​       SpringMVC是处于Web层的框架，所以其主要的作用就是用来接收前端发过来的请求和数据，然后经过处理并将处理的结果响应给前端。SpringMVC是隶属于Spring框架的一部分，主要是用来进行Web开发，是对Servlet进行了封装。

### 相关注解

| 名称 | @Controller                   |
| ---- | ----------------------------- |
| 类型 | 类注解                        |
| 位置 | SpringMVC控制器类定义上方     |
| 作用 | 设定SpringMVC的核心控制器bean |

| 名称     | @RequestMapping                 |
| -------- | ------------------------------- |
| 类型     | 类注解或方法注解                |
| 位置     | SpringMVC控制器类或方法定义上方 |
| 作用     | 设置当前控制器方法请求访问路径  |
| 相关属性 | value(默认)，请求访问路径       |

| 名称 | @ResponseBody                                    |
| ---- | ------------------------------------------------ |
| 类型 | 类注解或方法注解                                 |
| 位置 | SpringMVC控制器类或方法定义上方                  |
| 作用 | 设置当前控制器方法响应内容为当前返回值，无需解析 |

### 请求与响应

#### 请求映射路径

使用@RequestMapping()去设置接口的映射路径。

```java
@Controller
@RequestMapping("/user")
public class UserController {

    @RequestMapping("/save")
    @ResponseBody
    public String save(){
        System.out.println("user save ...");
        return "{'module':'user save'}";
    }
    
    @RequestMapping("/delete")
    @ResponseBody
    public String save(){
        System.out.println("user delete ...");
        return "{'module':'user delete'}";
    }
 }
}
```

#### 请求参数

@RequestParam

| 名称     | @RequestParam                                      |
| -------- | -------------------------------------------------- |
| 类型     | 形参注解                                           |
| 位置     | SpringMVC控制器方法形参定义前面                    |
| 作用     | 绑定请求参数与处理器方法形参间的关系               |
| 相关参数 | required：是否为必传参数  defaultValue：参数默认值 |

#### @RequestBody与@RequestParam区别

* 区别
  * @RequestParam用于接收url地址传参，表单传参【application/x-www-form-urlencoded】
  * @RequestBody用于接收json数据【application/json】

* 应用
  * 开发中，发送json格式数据为主，@RequestBody应用较广
  * 如果发送非json格式数据，选用@RequestParam接收请求参数

#### 日期类型参数传递

| 名称     | @DateTimeFormat                 |
| -------- | ------------------------------- |
| 类型     | 形参注解                        |
| 位置     | SpringMVC控制器方法形参前面     |
| 作用     | 设定日期时间型数据格式          |
| 相关属性 | pattern：指定日期时间格式字符串 |

#### 响应json数据

| 名称 | @ResponseBody                                                |
| ---- | ------------------------------------------------------------ |
| 类型 | 方法\类注解                                                  |
| 位置 | SpringMVC控制器方法定义上方和控制类上                        |
| 作用 | 设置当前控制器返回值作为响应体, 写在类上，该类的所有方法都有该注解功能 |

### RestFul风格

#### 介绍: https://blog.csdn.net/qq_55806761/article/details/135425645

| 名称 | @RestController                                              |
| ---- | ------------------------------------------------------------ |
| 类型 | 类注解                                                       |
| 位置 | 基于SpringMVC的RESTful开发控制器类定义上方                   |
| 作用 | 设置当前控制器类为RESTful风格， 等同于@Controller与@ResponseBody两个注解组合功能 |



| 名称     | @GetMapping @PostMapping @PutMapping @DeleteMapping          |
| -------- | ------------------------------------------------------------ |
| 类型     | 方法注解                                                     |
| 位置     | 基于SpringMVC的RESTful开发控制器方法定义上方                 |
| 作用     | 设置当前控制器方法请求访问路径与请求动作，每种对应一个请求动作， 例如@GetMapping对应GET请求 |
| 相关属性 | value（默认）：请求访问路径                                  |





