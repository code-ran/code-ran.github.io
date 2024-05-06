---
title: linux使用curl命令发送请求
date: 2023-12-05 13:56:19
tags: curl
categories: curl相关
---

### 1、curl概述

​        通过curl对api进行发送请求，是跨平台的(linux、win、mac都支持)，不需要浏览器也能作为Http客户端进行请求的发送。

测试接口：

```java
package com.ransibi.controller;
import com.alibaba.fastjson.JSONObject;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("rsb")
public class TestPostController {

    @PostMapping("post/test")
    public JSONObject testPostMethod(@RequestBody TestBean testBean) {
        System.out.println(testBean);
        String jsonInfo = testBean.getUname() + testBean.getPwd();
        JSONObject result = new JSONObject();
        result.put("code", 200);
        result.put("data", jsonInfo);
        return result;
    }

    @GetMapping("get/test")
    public JSONObject testGetMethod() {
        JSONObject result = new JSONObject();
        result.put("code", 200);
        result.put("data", "sucess");
        return result;
    }

    @GetMapping("get/test/param")
    public JSONObject testParamGetMethod(TestBean testBean) {
        JSONObject result = new JSONObject();
        result.put("code", 200);
        result.put("data", testBean.getUname() + testBean.getPwd());
        return result;
    }
}
```

```java
package com.ransibi.controller;
import lombok.Data;

@Data
public class TestBean {
    private String uname;
    private Integer pwd;
}
```



### 2、使用curl发送Get请求

(1)无参get请求

```
curl -v http://ip:port/rsb/get/test
```

(2)有参get请求

&符号在linux系统中为后台运行的操作符，所以需要使用反斜杠‘ \ ’进行转义,如下所示:

```
curl -v http://ip:port/rsb/get/test/param?uname=ransibi\&pwd=123456789
```



### 3、使用curl发送Post请求

(1)发送application/json请求

```
curl -H "Content-Type:application/json" -X POST -d '{"uname": "huang","pwd": "12345"}' "http://ip:port/rsb/post/test"

#使用数据文件
curl -X POST -d "@data.json" "http://www.example.com"
```

(2)发送multipart/form-data请求

```
curl -X POST -d 'username=cavan' http://www.example.com
```

(3)发送**application/x-www-form-urlencoded**请求

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'username=cavan' -d 'password=123456' http://www.example.com

tips: -H "Content-Type: application/x-www-form-urlencoded"可以省略
curl -X POST -d "param1=value1&param2=value2" http://www.example.com

```

### 

### 4、Linux安装curl命令

```
--------ubuntu/debian
sudo apt update
sudo apt install curl

--------centos/rhel
sudo yum install curl

--------fedora
sudo dnf install curl

--------macos
brew install curl
```

