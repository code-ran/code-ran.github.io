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

```xml
<if test="startTime != null and startTime != ''">
     and  ayz_time <![CDATA[>=]]> CONCAT(#{startTime},' 00:00:00')
</if>
<if test="endTime != null and endTime != ''">
    and  ayz_time <![CDATA[<=]]> CONCAT(#{endTime},' 23:59:59')
 </if>
```

