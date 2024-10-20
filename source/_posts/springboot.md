---
title: springboot
date: 2024-10-20 22:01:44
tags: springboot
categories: springboot相关
---

### 基础配置

官方在线文档: https://docs.spring.io/spring-boot/appendix/application-properties/index.html

### 配置文件类型

#### 优先级

```
properties > yml > yaml
```

#### properties(传统/默认)

properties 是以键值的形式配置的，key（单词之间使用`.`进行分割，字母小写） 和 value 之间是以"="连接

```properties
server.port=8080
```

#### yml(推荐)

```yaml
server:
  port: 8088
```

#### yaml与yml格式一样

