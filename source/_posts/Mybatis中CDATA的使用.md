---
title: Mybatis中 <![CDATA[ ]]> 的使用
date: 2022-11-23 15:28:14
tags: MyBatis
categories: MyBatis相关

---

#### Mybatis中 <![CDATA[ ]]> 的使用

```
Mybatis的sql是写在xml映射文件中的，如果sql中有一些特殊的字符，在解析xml文件的时候会被转义，
使用<![CDATA[ ]]>就可以让这些特殊字符不被转义。<![CDATA[ ]]>是xml的语法，
放在CDATA[]内部的特殊字符都会被解析器忽略，所以在我们使用<if test="">、</if>、
<where>、</where>、<choose>、</choose>、<trim>、</trim>等标签实现动态sql时，
我们需要把sql语句中出现的<、<=、&等特殊符号都放在CDATA[]的内部
```

```xml
    <update id="delData" parameterType="com.hx.pojo.BrowsingHistory">
        update td_browsing_history set status = '9'
        <where>
            browsing_user_id = #{browsingUserId}
            <if test="startTime != null ">
                and browsing_time <![CDATA[ >= ]]> #{startTime}
            </if>
            <if test="endTime != null ">
                and browsing_time <![CDATA[ <= ]]> #{endTime}
            </if>
            <if test="id != null and id != ''">
                and ID = #{id}
            </if>
        </where>
    </update>
```

如何实现精确查询？可以使用CONCAT拼接字符串的方式。

这种方式使用String去接收参数

```xml
<if test="startTime != null and startTime != ''">
     and  ayz_time <![CDATA[>=]]> CONCAT(#{startTime},' 00:00:00')
</if>
<if test="endTime != null and endTime != ''">
    and  ayz_time <![CDATA[<=]]> CONCAT(#{endTime},' 23:59:59')
 </if>
```

另一种方式是使用Date接收参数，再结束时间的get构造方法里进行结束时间的时、分、秒、毫秒的组装。

```xml
<if test="startTime != null">
     and  ayz_time <![CDATA[>=]]> #{startTime,jdbcType=TIMESTAMP}
</if>
<if test="endTime != null">
    and  ayz_time <![CDATA[<=]]> #{endTime,jdbcType=TIMESTAMP}
 </if>
```

```java
    private Date beginTime;

    private Date endTime;
    
    public Date getBeginTime() {
        return beginTime;
    }

    public void setBeginTime(Date beginTime) {
        this.beginTime = beginTime;
    }

    public Date getEndTime() {
        if (endTime == null) {
            return null;
        }
        try {
            Calendar c = Calendar.getInstance();
            c.setTime(endTime);
            c.set(Calendar.HOUR_OF_DAY, 23);

            c.set(Calendar.MINUTE, 59);

            c.set(Calendar.SECOND, 59);

            c.set(Calendar.MILLISECOND, 999);
            return c.getTime();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return endTime;
    }

    public void setEndTime(Date endTime) {
        this.endTime = endTime;
    }
```

使用java8 LocalDateTime优化

```java
    private static Date getEndTime(Date dateParam) {
        if (dateParam == null) {
            return null;
        }
        Date endTime = null;
        try {
            LocalDateTime localDateTime = LocalDateTime.ofInstant(dateParam.toInstant(), ZoneId.systemDefault());
            //LocalDateTime 可以用于更改日期和时间的年、月、日、时、分、秒和纳秒
            localDateTime = localDateTime.withHour(23).withMinute(59).withSecond(59).withNano(999999999);
            endTime = Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());
        } catch (Exception e) {
            e.printStackTrace();
        }
        return endTime;
    }
```

或者

```java
            LocalDateTime localDateTime = LocalDateTime.ofInstant(dateParam.toInstant(), ZoneId.systemDefault());
            //LocalDateTime 可以用于更改日期和时间的年、月、日、时、分、秒和纳秒
            localDateTime = localDateTime.withHour(23).withMinute(59).withSecond(59);
            //如果需要更改毫秒，需要转成Instant
            Instant instant = localDateTime.atZone(ZoneId.systemDefault()).toInstant();
            //然后使用plusMillis()追加毫秒值
            instant = instant.plusMillis(999);
            LocalDateTime localDateTime1 = LocalDateTime.ofInstant(instant, ZoneId.systemDefault());
            endTime = Date.from(localDateTime1.atZone(ZoneId.systemDefault()).toInstant());
```

使用

```java
    public static void main(String[] args) {
        String str = "2024-08-21";
        Date date = DateUtil.parse(str);
        Date time = getEndTime(date);
        //hutool工具
        String formatDateTime = DateUtil.format(time,"yyyy-MM-dd HH:mm:ss:SSS");
        System.out.println(formatDateTime);
    }
```

```
Wed Aug 21 23:59:59 CST 2024
2024-08-21 23:59:59:999
```



