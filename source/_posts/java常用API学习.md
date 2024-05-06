---
title: java常用API学习
date: 2022-11-23 15:28:14
tags: java
categories: java相关
sticky: 1
---

####  java常用API学习

##### 1、Date日期类:

```java
Date类在Java中代表的是系统当前此刻日期时间对象;
Date类：
        包：java.util.Date。
        构造器：
            -- public Date():创建当前系统的此刻日期时间对象。
            -- public Date(long time):
        方法：
             -- public long getTime():返回自 1970 年 1 月 1 日 00:00:00 GMT 以来走过的总的毫秒数。

        时间记录的两种方式：
             a.Date日期对象。
             b.时间毫秒值：从1970-01-01 00:00:00开始走到此刻的总的毫秒值。 1s = 1000ms
code example:
        //创建一个日期对象代表了系统此刻日期时间对象
        Date d = new Date();
        System.out.println(d);

        // b.拿当前日期对象的时间毫秒值
        long time = d.getTime();
        System.out.println(time);
```

```java
拓展：时间毫秒值的作用。

时间毫秒值可以用于做时间的计算：例如代码的执行性能分析。
        // 1.拿到此刻日期时间对象的毫秒值,一般使用System.currentTimeMillis();
        long startTime = new Date().getTime();
        for(int i = 1; i < 1000000 ; i++ ){
            System.out.println("输出："+i);
        }
        // 2.拿到此刻日期时间对象的毫秒值
        long endTime = new Date().getTime();
        System.out.println( (endTime - startTime) / 1000.0 +"s");
```

```java
public Date():创建当前系统的此刻日期时间对象。
public Date(long time):把时间毫秒值转换成日期对象。
---
Date日期对象 -> getTime() -> 时间毫秒值
时间毫秒值 -> new Date(时间毫秒值) -> Date日期对象

```

##### 2、DateFormat时间格式化类:

```java
DateFormat作用：
    1.可以把“日期对象”或者“时间毫秒值”格式化成我们喜欢的时间形式。（格式化时间）
    2.可以把字符串的时间形式解析成日期对象。（解析字符串时间）
DateFormat是一个抽象类，不能直接使用，要找它的子类：SimpleDateFormat
```

```java
SimpleDateFormat简单日期格式化类：
    包：java.text.SimpleDateFormat
    构造器：public SimpleDateFormat(String pattern)：
           指定时间的格式创建简单日期格式化对象。
    方法：
        -- public String format(Date date):可以把日期对象格式化成我们喜欢的时间形式，返回的是字符串!
        -- public String format(Object time):可以把时间毫秒值格式化成我们喜欢的时间形式，返回的是字符串!
        -- public Date parse(String date) throws ParseException:把字符串的时间解析成日期对象
小结：
    简单日期格式化类SimpleDateFormat可以把日期对象格式化成我们喜欢的时间形式
        -- public String format(Date date):可以把日期对象格式化成我们喜欢的时间形式，返回的是字符串!
```

```java
// 需求：把此刻日期对象格式化成我们喜欢的形式。
// 1.得到此刻日期对象
Date d = new Date();
System.out.println(d);

// 2.创建一个简单日期格式化对象负责格式化日期对象
// 注意：参数是之间的格式。
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss EEE a");

// 3.开始调用方法格式化时间得到格式化的字符串时间形式
String rs = sdf.format(d);
System.out.println(rs);

----console:
Mon Nov 21 13:39:45 CST 2022
2022年11月21日 13:39:45 星期一 下午
```

##### 3、日历类Calendar的使用

```java
Calendar代表了系统此刻日期对应的日历对象。
Calendar是一个抽象类，不能直接创建对象。
Calendar日历类创建日历对象的语法：
    Calendar rightNow = Calendar.getInstance();
Calendar的方法：
    1.public static Calendar getInstance(): 返回一个日历类的对象。
    2.public int get(int field)：取日期中的某个字段信息。
    3.public void set(int field,int value)：修改日历的某个字段信息。
    4.public void add(int field,int amount)：为某个字段增加/减少指定的值
    5.public final Date getTime(): 拿到此刻日期对象。
    6.public long getTimeInMillis(): 拿到此刻时间毫秒值
```

##### 4、Math类的使用

```java
Math用于做数学运算。
Math类中的方法全部是静态方法，直接用类名调用即可。
方法：
      方法名                                        说明                
      public static int abs(int a)                  获取参数a的绝对值：
      public static double ceil(double a)           向上取整
      public static double floor(double a)          向下取整
      public static double pow(double a, double b)  获取a的b次幂        
      public static long round(double a)            四舍五入取整
```

##### 5、BigDecimal大数据类

```
浮点型运算的时候直接进行 +  * / 可能会出现数据失真（精度问题）。
BigDecimal可以解决浮点型运算数据失真的问题。
```

```java
BigDicimal类：
    包：java.math.
    创建对象的方式（最好的方式：）
          public static BigDecimal valueOf(double val) :包装浮点数成为大数据对象。
    方法声明
          public BigDecimal add(BigDecimal value)       加法运算
          public BigDecimal subtract(BigDecimal value)  减法运算 
          public BigDecimal multiply(BigDecimal value)  乘法运算 
          public BigDecimal divide(BigDecimal value)    除法运算
          public double doubleValue():把BigDecimal转换成double类型。
```

```java
// 浮点型运算的时候直接+  * / 可能会出现数据失真（精度问题）。
System.out.println(0.1 + 0.2);
System.out.println(0.09 + 0.01);
System.out.println(1.0  - 0.32);
System.out.println(1.015 * 100);
System.out.println(1.301 / 100);

System.out.println("-------------------------");
double a = 0.1 ;
double b = 0.2 ;
double c = a + b ;
System.out.println(c);

// 1.把浮点数转换成大数据对象运算
BigDecimal a1 = BigDecimal.valueOf(a);
BigDecimal b1 = BigDecimal.valueOf(b);
//BigDecimal c1 = a1.add(b1);  // 加法
BigDecimal c1 = a1.divide(b1); // 除法
System.out.println(c1);

// 结果可能需要继续使用!!!
// BigDecimal只是解决精度问题的手段，double数据才是我们的目的！！
double rs = c1.doubleValue();
System.out.println(rs);
```

##### 6、包装类的使用

```
包装类可以把字符串类型的数值转换成对应的基本数据类型的值（真的很有用）
            -- Xxx.parseXxx("字符串类型的数值")
            -- Xxx.valueOf("字符串类型的数值"):推荐使用!
```

```java
//.把字符串类型的数值转换成对应的基本数据类型的值。（真的很有用）
String numStr = "23";
//int numInt = Integer.parseInt(numStr);
int numInt = Integer.valueOf(numStr);
System.out.println(numInt+1);

String doubleStr = "99.9";
//double doubleDb = Double.parseDouble(doubleStr);
double doubleDb = Double.valueOf(doubleStr);
System.out.println(doubleDb+0.1);
```

```java
    Java认为一切皆对象。引用数据类型就是对象了。
    但是在Java中8基本数据类型不是对象，只是表示一种数据的类型形式,这8种数据类型显得很突兀。
    Java为了一切皆对象的思想统一，把8种基本数据类型转换成对应的类，这个类称为
    基本数据类型的包装类。

基本数据类型                    包装类（引用数据类型）
     byte                      Byte
     short                     Short
     int                       Integer(特殊)
     long                      Long

     float                     Float
     double                    Double
     char                      Character(特殊)
     boolean                   Boolean

自动装箱：可以直接把基本数据类型的值或者变量赋值给包装类。
自动拆箱：可以把包装类的变量直接赋值给基本数据类型。

为什么需要基本数据类型的包装类？
（1）Java是面向对象的语言，很多地方需要使用的是对象而不是基本数据类型。例如，List，Map等容器类中基本类型是放不进去的。
（2）包装类在原先基本数据类型上，新增加了很多方法，如Integer.valueOf(String s)


包装后的类型和原数据类型有什么区别呢？ 
拿Int 和Integer来说
一、int 是基本类型，直接存数值；而integer引用数据类型。
二、Int的声明不需要实例化，且变量声明后的初始值为0；Integer的是一个类，初始值为null，需要进行实例化，才能对变量数据进行处理。
三、Integer类是int的包装类，实际开发中Integer被看成一个对象，可以进行数据转换等操作。
```

```java
int a = 12 ;
Integer a1 = 12 ;  // 自动装箱
Integer a2 = a ;   // 自动装箱

double b = 99.9;
Double b1 = 99.9; // 自动装箱
Double b2 = b ;   // 自动装箱

Integer c = 100 ;
int c1 = c ;      // 自动拆箱

int d = 12;
Integer d1 = null; // 引用数据类型的默认值可以为null
Integer d2 = 0;

System.out.println("-----------------");
Integer it = Integer.valueOf(12);  // 手工装箱！
// Integer it1 = new Integer(12); // 手工装箱！
Integer it2 = 12;


Integer it3 = 111 ;
int it33 = it3.intValue(); // 手工拆箱
int it333 = it3;
```

