---
title: java8日期时间
date: 2024-10-14 14:15:14
tags:  java8日期时间
categories: java8相关
---

### jdk8日期时间类API

旧版日期时间存在的问题:

1、设计很差：在java.util和java.sql的包中都有日期类，java.util.Date同时包含日期和时间，而ava.sql.Date仅包
含日期。此外用于格式化和解析的类在java.text包中定义

2、非线程安全：java.util.Date是非线程安全的，所有的日期类都是可变的，这是java日期类最大的问题之一。

3、时区处理麻烦：日期类并不提供国际化，没有时区支持，因此java引入了java.util.Calendar和java.util.TimeZone类，但他们同样存在上述所有的问题。

#### 概述

jdk新增的日期时间api位于java.time包下，并且是线程安全的。

#### LocalDate

```java
    /**
     * LocalDate 获取日期信息，格式:1997-09-25
     */
    public static void localDateLearn() {
        //创建指定日期1997-09-25
        LocalDate localDate = LocalDate.of(1997, 9, 25);
        System.out.println("localDate = " + localDate);
        //当前日期
        LocalDate nowDate = LocalDate.now();
        System.out.println("nowDate = " + nowDate);
        //获取日期信息
        System.out.println("当前年 = " + nowDate.getYear());
        System.out.println("当前月 = " + nowDate.getMonthValue());
        System.out.println("日 = " + nowDate.getDayOfMonth());
        System.out.println("星期 = " + nowDate.getDayOfWeek());
    }
```

#### LocalTime

```java
    /**
     * LocalTime 获取时间信息,格式: 14:56:57.721
     */
    public static void localTimeLearn() {
        //指定时间
        LocalTime localTime = LocalTime.of(14, 59, 54, 721);
        System.out.println("localTime = " + localTime);
        //当前时间
        LocalTime nowTime = LocalTime.now();
        System.out.println("nowTime = " + nowTime);
        //获取时间信息
        System.out.println("时 = " + nowTime.getHour());
        System.out.println("分 = " + nowTime.getMinute());
        System.out.println("秒 = " + nowTime.getSecond());
        System.out.println("纳秒 = " + nowTime.getNano());
    }
```

#### LocalDateTime

```java
    /**
     * LocalDateTime  获取日期时间,格式: 2024-10-14T01:05:51.797
     */
    private static void localDateTimeLearn() {
        //指定日期时间
        LocalDateTime localDateTime = LocalDateTime.of(1997, 9, 25, 1, 10, 20);
        System.out.println("localDateTime = " + localDateTime);
        //当前日期时间
        LocalDateTime nowDateTime = LocalDateTime.now();
        System.out.println("nowDateTime = " + nowDateTime);
        //日期时间信息
        System.out.println("年 = " + nowDateTime.getYear());
        System.out.println("月 = " + nowDateTime.getMonthValue());
        System.out.println("日 = " + nowDateTime.getDayOfMonth());
        System.out.println("时 = " + nowDateTime.getHour());
        System.out.println("分 = " + nowDateTime.getMinute());
        System.out.println("秒 = " + nowDateTime.getSecond());
        System.out.println("纳米 = " + nowDateTime.getNano());

        //LocalDateTime转时间戳
        ZoneId zoneId = ZoneId.systemDefault();
        Instant instant = LocalDateTime.now().atZone(zoneId).toInstant();
        //1728961645113
        System.out.println("转成时间戳后 = "+instant.toEpochMilli());

        //时间戳转LocalDateTime
        Instant instant1 = Instant.ofEpochMilli(1728961645113L);
        LocalDateTime localDateTime1 = LocalDateTime.ofInstant(instant1, ZoneId.systemDefault());
        System.out.println("时间戳转成LocalDateTime = "+localDateTime1);
    }
```

#### 对日期时间的修改

```java
    /**
     * jdk8对日期时间的修改,以LocalDateTime为例，LocalDate和LocalTime类似
     */
    public static void editLocalDateTime() {
        LocalDateTime nowDateTime = LocalDateTime.now();
        System.out.println("nowDateTime = " + nowDateTime);
        //修改年份 withYear()
        LocalDateTime editYear = nowDateTime.withYear(2023);
        System.out.println("editYear = " + editYear);
        //修改月份
        LocalDateTime editMonth = nowDateTime.withMonth(7);
        System.out.println("editMonth = " + editMonth);
        //修改月份
        LocalDateTime editDay = nowDateTime.withDayOfMonth(10);
        System.out.println("editDay = " + editDay);
        //修改小时
        LocalDateTime editHour = nowDateTime.withHour(12);
        System.out.println("editHour = " + editHour);
        //修改分钟
        LocalDateTime editMinute = nowDateTime.withMinute(12);
        System.out.println("editMinute = " + editMinute);
        //修改秒
        LocalDateTime editSecond = nowDateTime.withSecond(55);
        System.out.println("editSecond = " + editSecond);
        //修改纳秒
        LocalDateTime editNano = nowDateTime.withNano(666);
        System.out.println("editNano = " + editNano);

        //时间偏移--当前时间之后
        System.out.println("3天后 = " + nowDateTime.plusDays(3));
        System.out.println("3个月后 = " + nowDateTime.plusMonths(3));
        System.out.println("3年后 = " + nowDateTime.plusYears(3));
        System.out.println("1星期后 = " + nowDateTime.plusWeeks(1));
        //还支持时、分、秒、纳秒......

        //时间偏移--当前时间之前
        System.out.println("3天前 = " + nowDateTime.minusDays(3));
        System.out.println("3个月前 = " + nowDateTime.minusMonths(3));
        System.out.println("3年前 = " + nowDateTime.minusYears(3));
        System.out.println("1星期前 = " + nowDateTime.minusWeeks(1));
        //还支持时、分、秒、纳秒......
    }
```

#### 时间比较

```java
    /**
     * jdk8日期时间比较,以LocalDate为例,LocalDateTime与LocalTime类似
     */
    public static void compareDate() {
        LocalDate t1 = LocalDate.now();
        LocalDate t2 = LocalDate.of(2023, 10, 14);
        if (t1.equals(t2)) {
            System.out.println("相等");
        } else {
            System.out.println("不相等");
        }
        //在日期之前
        if (t1.isBefore(t2)) {
            System.out.println("t1在t2之前");
        }
        //在日期之后
        if (t1.isAfter(t2)) {
            System.out.println("t1在t2之之后");
        }
    }
```

#### 日期时间格式化

```java
/**
 * jdk8日期时间的格式化与解析
 */
public static void formatOrParseDate() {
    LocalDateTime nowDateTime = LocalDateTime.now();
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
    //将日期格式化成字符串
    String format = nowDateTime.format(formatter);
    System.out.println("格式化后的时间 = " + format);

    //将字符串解析为日期时间
    String strTime = "2023-10-14 01:50:48";
    LocalDateTime parseDateTime = LocalDateTime.parse(strTime, formatter);
    //2023-10-14T01:50:48
    System.out.println("解析后的时间 = " + parseDateTime);
}
```

#### Instant类 

```java
   /**
     * jdk8 Instant,获取时间戳/时间线
     */
    public static void instantLearn() {
        //获取本初子午线（UTC时区）对应的标准时间,其时间与北京时间相差八个小时
        //Instant内部保存了秒和纳秒，一般用于程序统计,T有着分割日期、时间的作用
        Instant instant = Instant.now();
        //2024-10-15T01:37:47.294Z
        System.out.println("当前标准时间：" + instant);
        //添加时间的偏移量
        //偏移八个小时，对应北京时间
        OffsetDateTime offsetDateTime = instant.atOffset(ZoneOffset.ofHours(8));
        System.out.println(offsetDateTime);//2019-02-18T15:32:50.611+08:00
        System.out.println(instant.getNano());
        System.out.println(instant.getEpochSecond());
        //获取自1970年1月1日0时0分0秒（UTC）开始的毫秒数  ---> Date类的getTime()
        //1728956267294
        System.out.println("当前时间戳 = " + instant.toEpochMilli());
        long time = 1728956267294L;
        Instant instant1 = Instant.ofEpochMilli(time);
        System.out.println("时间戳转成instant后：" + instant1);

        DateTimeFormatter dateTimeFormatter6 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String format6 = dateTimeFormatter6.format(LocalDateTime.ofInstant(Instant.ofEpochMilli(1728960489590L), ZoneId.systemDefault()));
        System.out.println("时间戳转成String后：" + format6);

        long timestamp = 1728960489590L;
        Instant instant2 = Instant.ofEpochMilli(timestamp);
        //增加一小时
        Instant newInstant = instant2.plus(Duration.ofHours(1));
        long newTimestamp = newInstant.toEpochMilli();
        //或者 long newTimestamp = timestamp + (60 * 60 * 1000);
        System.out.println("增加一小时后的时间戳 = " + newTimestamp);
    }
```

#### 计算日期的差

```java
    /**
     * jdk8计算日期时间差
     * Duration: 计算两个LocalTime(时分秒)的距离
     * Period: 计算两个LocalDate(年月日)的距离
     */
    public static void compute() {
        LocalTime t1 = LocalTime.now();
        LocalTime t2 = LocalTime.of(15, 6, 50);
        Duration between = Duration.between(t2, t1);
        System.out.println("相差的天数:" + between.toDays());
        System.out.println("相差的小时数:" + between.toHours());
        System.out.println("相差的分钟数:" + between.toMinutes());
    }
```

#### 时间校正器

```java
    /**
     * jdk8时间校正器
     */
    public static void temporalAdjuster() {
        LocalDateTime nowDateTime = LocalDateTime.now();
        System.out.println("当前日期时间:" + nowDateTime);
        LocalDateTime newDateTime = nowDateTime.with(TemporalAdjusters.firstDayOfNextMonth());
        //下个月的第一天的当前时间
        System.out.println("下个月的第一天:" + newDateTime);
        //firstDayOfMonth() 当月第一天
        //firstDayOfNextYear() ...... 其它见api文档TemporalAdjusters类
    }
```

#### 设置日期时间时区

```java
    /**
     * jdk8设置日期时间的时区
     * LocalDate、LocalTime、LocalDateTime是不带时区的,带时区的日期时间类分别是: ZonedDate、ZonedTime、ZonedDateTime
     * 注意: 每个时区对应成id,格式为: "区域/城市"
     * ZoneId类包含了所有的时区信息
     */
    public static void zoneDateLearn() {
        //获取所有的时区id
//        ZoneId.getAvailableZoneIds().forEach(System.out::println);
        //中国使用的是东八区的时区，比标准时间早八小时
        LocalDateTime now = LocalDateTime.now();
        //当前时间 = 2024-10-15T10:26:11.767
        System.out.println("当前时间 = " + now);

        //创建世界标准时间(格林威治时间) 2024-10-12T02:14:07.501Z
        ZonedDateTime zonedDateTime = ZonedDateTime.now(Clock.systemUTC());
        //世界标准时间(格林威治时间) = 2024-10-15T02:26:11.769Z
        System.out.println("世界标准时间(格林威治时间) = " + zonedDateTime);

        //使用计算机本地默认的时区
        ZonedDateTime localZonedT = ZonedDateTime.now();
        //本地时区时间 = 2024-10-15T10:26:11.769+08:00[Asia/Shanghai]
        System.out.println("本地时区时间 = " + localZonedT);

        //创建指定时区的时间
        ZonedDateTime zonedDateTime1 = ZonedDateTime.now(ZoneId.of("America/New_York"));
        //美国纽约当前时间带时区 = 2024-10-14T22:26:11.771-04:00[America/New_York]
        System.out.println("美国纽约当前时间带时区 = " + zonedDateTime1);

        //调整时区
        ZonedDateTime zonedDateTime2 = zonedDateTime1.withZoneSameInstant(ZoneId.of("Asia/Shanghai"));
        //调整时区后 = 2024-10-15T10:32:02.744+08:00[Asia/Shanghai]
        System.out.println("调整时区后 = " + zonedDateTime2);
    }
```

