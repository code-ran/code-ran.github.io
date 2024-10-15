---
title: java常用API学习
date: 2022-11-23 15:28:14
tags: java
categories: java相关
sticky: 1
---

####  java常用API学习

##### 1、Date日期类:

Date类在Java中代表的是系统当前此刻日期时间对象，java.util.Date包下。

```java
        //当前的时间
        Date date = new Date();
        System.out.println(date.toString());

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

```java
//获取当前时间的时间戳(毫秒数)
  Date date = new Date();
  long nowDate = date.getTime();
//时间戳转Date
  long t = 1728960489590L;
  Date date2 = new Date(t);
  //Tue Oct 15 10:48:09 CST 2024
  System.out.println(date2.toString());
//将Date对象转换为 Instant 
  Instant instant = date2.toInstant();
  LocalDateTime localDateTime = LocalDateTime.ofInstant(instant, ZoneId.systemDefault());
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

```java
        Calendar calendar = Calendar.getInstance();
        //输出现在的时间
        Date calendarDate =  calendar.getTime();
        System.out.println(calendarDate.toString());
        //返回1970年1月1日到现在的毫秒数
        System.out.println(calendar.getTimeInMillis());
        //获取时间信息
        //月MONTH  天： DAY_OF_MONTH  小时： HOUR_OF_DAY（24）HOUR12(12) 分钟：MINUTE 秒：SECOND。
        int year = calendar.get(Calendar.YEAR);
        System.out.println("year:"+year);
        //其中MONTH 范围是0-11，所以用的时候MONTH+1。
        //修改时间
        Calendar calendar2 = Calendar.getInstance();

        //表示将天数改为5.
        calendar2.set(Calendar.DAY_OF_MONTH,5);
        Date time = calendar2.getTime();
        System.out.println("calendar2:"+sdf.format(time));
        //add方法修改时间
        // 加一个小时。
        calendar2.add(Calendar.HOUR,1);
        //获取当前月份中的最大天数和最小天数。
        int max = calendar2.getActualMaximum(Calendar.DAY_OF_MONTH);
        int min  = calendar2.getActualMinimum(Calendar.DAY_OF_MONTH);
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

浮点型(float、double)运算的时候直接进行 +  * / 可能会出现数据失真（精度问题）。
BigDecimal可以解决浮点型运算数据失真的问题。

```java
        BigDecimal bd1 = new BigDecimal("0.9");
        BigDecimal bd2 = new BigDecimal("1.0");
        System.out.println("b1:" + bd1);
        System.out.println("b2:" + bd2);

        //减法：subtract
        BigDecimal r1 = bd2.subtract(bd1);
        System.out.println("b2-b1:" + r1);

        //加法：add
        BigDecimal r2 = bd2.add(bd1);
        System.out.println("b2+b1:" + r2);

        //乘法：multiply
        BigDecimal r3 = bd2.multiply(bd1);
        System.out.println("b2*b1:" + r3);

        /**
         特殊：除法：divide 如果不能整除会报错 必须指明保留几位小数
         */
        //除法: divide
        //指定小数部分的取舍模式，通常采用四舍五入的模式
        //取值为BigDecimal.ROUND_HALF_UP
        //保留两位小数，且用四舍五入的方法
        BigDecimal r4 = bd1.divide(bd2, 2, RoundingMode.HALF_UP);
        System.out.println("b1/b2保留两位小数:" + r4);
```

```
b1:0.9
b2:1.0
b2-b1:0.1
b2+b1:1.9
b2*b1:0.90
b1/b2保留两位小数:0.90
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

##### 7、String类

```java
String str = new String(" Hello World ");
System.out.println("str:"+str);

//返回字符串的长度
System.out.println("str长度为:"+str.length());

//根据下标获取字符
System.out.println("str第四个字符为:"+ str.charAt(4));

//判断当前字符串是否包含字符
System.out.println("str是否包含e:"+ str.contains("e"));

//将字符串转换成数组
char[] a = str.toCharArray();
System.out.println("=========转换成字符数组后==========");
for (char c : a) {
    System.out.println(c);
}

//查找str首次出现的下标，存在，则返回该下标，不存在则返回-1
int index = str.indexOf("l");
System.out.println("l首次出现的下标为:"+index);

//查找字符串在当前字符串中最后一次出现的下标索引。
System.out.println("l最后一次出现的下标为:"+str.lastIndexOf("l"));
//去掉字符串前、后的空格
System.out.println("str去掉空格后为:"+str.trim());
//将小写转成大写
System.out.println("转成大写:"+str.toUpperCase());
//将大写转成小写
System.out.println("转成小写:"+str.toLowerCase());
//判断字符串的结尾
System.out.println("str结尾是否是空格:"+str.endsWith(" "));
//判断字符串的开头
System.out.println("str开头是否是空格:"+str.startsWith(" "));
//字符串替换
System.out.println("str中lo替换成s:"+str.replace("lo","s"));

//拆分字符串
String [] b = str.split("l");
System.out.println("==============使用l字符串进行拆分后==========");
for (String s : b) {
    System.out.println(s);
}

//不区分大小写比较字符串
String newStr = " hello world ";
boolean comStr = newStr.equalsIgnoreCase(str);
System.out.println(comStr);
```

##### 8、StringBuffer与StringBuilder

StringBuffer: 可变长字符串，JDK1.0提供，运行效率慢、线程安全；StringBuilder: 可变长字符串，JDK5.0提供，运行效率快、线程不安全。

```java
StringBuffer sb = new StringBuffer();
//在字符串末尾追加字符串
sb.append(str);
System.out.println(sb);

//在index位置添加str
sb.insert(0,"测试");
System.out.println(sb);

//替换索引之间的字符串
sb.replace(11,15,"替换了");
System.out.println(sb);

//删除索引之间的字符串
sb.delete(11,15);
System.out.println(sb);
```
