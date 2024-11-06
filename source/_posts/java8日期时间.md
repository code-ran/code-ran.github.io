---
title: jdk7和jdk8日期时间
date: 2024-10-14 14:15:14
tags:  jdk7和jdk8日期时间
categories: java8相关
---

### jdk7和jdk8日期时间类API

#### JDK7日期时间

##### Date类

表示特定的瞬间，精确到毫秒。使用无参构造，可以自动设置当前系统时间的毫秒时刻；指定long类型的构造参数，可以自定义毫秒时刻。在java.util下

```java
// 创建日期对象，把当前的时间
System.out.println(new Date()); // Tue Jan 16 14:37:35 CST 2020
// 创建日期对象，把当前的毫秒值转成日期对象
System.out.println(new Date(0L)); // Thu Jan 01 08:00:00 CST 1970 ---时间原点
```

常用方法

```java
public long getTime() 把日期对象转换成对应的时间毫秒值(时间戳)。获取的是日期对象从1970年1月1日 00:00:00到现在的毫秒值
public void setTime(long time) 把方法参数给定的毫秒值设置给日期对象
```

```java
//获取当前时间的时间戳(毫秒数)
  Date date = new Date();
  long nowDate = date.getTime();
  long time = System.currentTimeMillis();
  date.setTime(time);
//时间戳转Date
  long t = 1728960489590L;
  Date date2 = new Date(t);
  //Tue Oct 15 10:48:09 CST 2024
  System.out.println(date2.toString());
//将Date对象转换为 Instant 
  Instant instant = date2.toInstant();
  LocalDateTime localDateTime = LocalDateTime.ofInstant(instant, ZoneId.systemDefault());
```

```java
//昨天的时间
Date date1 = new Date(date.getTime() -(60*60*24*1000));
//after 之后
boolean after = date.after(date1);
System.out.println("今天是否在昨天之后:"+after);

//before 之前
boolean before = date1.before(date);
System.out.println("昨天是否在今天之前:"+before);

// -1:date1小,1:date1大,0:date2与date相等
int i = date1.compareTo(date);
System.out.println(i);
//比较日期是否相等
boolean equals = date1.equals(date);
System.out.println(equals);
```

##### SimpleDateFormat类

日期/时间格式化类，可以实现日期和字符串之间的转换和格式化。在java.text包下。

```java
格式化: 按照格式,Date -> String
解析:  按照格式,String -> Date
```

格式规则

| 标识字母（区分大小写） | 含义 |
| ---------------------- | ---- |
| y                      | 年   |
| M                      | 月   |
| d                      | 日   |
| H                      | 时   |
| m                      | 分   |
| s                      | 秒   |

常用方法

```java
public String format(Date date)：将Date对象格式化为字符串。
public Date parse(String source)：将字符串解析为Date对象。
```

```java
//时间格式化
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
try {
    //格式化Date日期
    String nowDateFormat = sdf.format(date);
    System.out.println("当前时间格式化之后:"+nowDateFormat);
    //解析字符串时间，将字符串时间转换成Date日期
    Date parse = sdf.parse(nowDateFormat);
    System.out.println(parse);
} catch (ParseException e) {
    throw new RuntimeException(e);
}
```

##### Calendar类

可以进行日期运算。它是一个抽象类，不能创建对象，我们可以使用它的子类：java.util.GregorianCalendar类。

获取GregorianCalendar对象两种方式:

```java
(1)直接创建GregorianCalendar对象。
(2)通过Calendar的静态方法getInstance()方法获取GregorianCalendar对象
```

常用方法

```java
//获取一个它的子类GregorianCalendar对象。
public static Calendar getInstance()

//获取某个字段的值。field参数表示获取哪个字段的值
public int get(int field)

//设置某个字段的值
public void set(int field,int value)

//为某个字段增加/减少指定的值
public void add(int field,int amount)
```

```java
public class TestDemo01 {
    public static void main(String[] args) {
        //1.获取一个GregorianCalendar对象
        //获取子类对象
        Calendar instance = Calendar.getInstance();

        //2.打印子类对象
        System.out.println(instance);

        /**
         * get方法
         */
        //3.获取属性
        int year = instance.get(Calendar.YEAR);
        //Calendar的月份值是0-11
        int month = instance.get(Calendar.MONTH) + 1;
        int day = instance.get(Calendar.DAY_OF_MONTH);

        int hour = instance.get(Calendar.HOUR);
        int minute = instance.get(Calendar.MINUTE);
        int second = instance.get(Calendar.SECOND);

        //返回值范围：1--7，分别表示："星期日","星期一","星期二",...,"星期六"
        int week = instance.get(Calendar.DAY_OF_WEEK);

        System.out.println(year + "年" + month + "月" + day + "日" +
                hour + ":" + minute + ":" + second);
        System.out.println(getWeek(week));

        /**
         * set方法
         */
        //设置属性——set(int field,int value):

        //计算班长出生那天是星期几(假如班长出生日期为：1998年3月18日)
        instance.set(Calendar.YEAR, 1998);
        instance.set(Calendar.MONTH, 3 - 1);//转换为Calendar内部的月份值
        instance.set(Calendar.DAY_OF_MONTH, 18);

        int w = instance.get(Calendar.DAY_OF_WEEK);
        System.out.println("班长出生那天是：" + getWeek(w));

        /**
         * add方法
         */
        //计算100天以后是哪年哪月哪日，星期几？
        instance.add(Calendar.DAY_OF_MONTH, 100);

        int y = instance.get(Calendar.YEAR);
        int m = instance.get(Calendar.MONTH) + 1;
        int d = instance.get(Calendar.DAY_OF_MONTH);

        int wk = instance.get(Calendar.DAY_OF_WEEK);
        System.out.println("100天后是：" + y + "年" + m + "月" + d + "日" + getWeek(wk));

    }

    public static String getWeek(int w) {
        //w = 1 --- 7
        String[] weekArray = {"星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六"};
        return weekArray[w - 1];
    }
}
```

#### JDK8新特性

旧版日期时间存在的问题:

1、设计很差：在java.util和java.sql的包中都有日期类，java.util.Date同时包含日期和时间，而java.sql.Date仅包
含日期。此外用于格式化和解析的类在java.text包中定义

2、非线程安全：java.util.Date是非线程安全的，所有的日期类都是可变的，这是java日期类最大的问题之一。

3、时区处理麻烦：日期类并不提供国际化，没有时区支持，因此java引入了java.util.Calendar和java.util.TimeZone类，但他们同样存在上述所有的问题。

##### 概述

jdk8新增的日期时间api位于java.time包下，并且是线程安全的。

##### LocalDate

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

##### LocalTime

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

##### LocalDateTime

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

##### 对日期时间的修改

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

##### 时间比较

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

##### 日期时间格式化

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

##### Instant类 

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

##### 计算日期的差

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

##### 时间校正器

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

##### 设置日期时间时区

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

