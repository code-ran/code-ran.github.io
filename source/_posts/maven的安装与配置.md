---
title: maven的安装与配置
date: 2021-10-31 03:28:14
tags: maven安装配置
categories: maven相关
---

###                                               Maven的安装与配置——并集成到IDEA中：

#### 1、首先去Maven的官网去下载maven的压缩文件：

http://maven.apache.org/download.cgi#

![image-20211031033019173](./maven的安装与配置/image-20211031033019173.png)

<!--more --> 

#### 2、然后解压，新建一个本地仓库reporsitory

![image-20211031033105727](./maven的安装与配置/image-20211031033105727.png)

#### 3、配置maven的仓库路径：

![image-20211031033124114](./maven的安装与配置/image-20211031033124114.png)

![image-20211031033138567](./maven的安装与配置/image-20211031033138567.png)

#### 4、更改为阿里云镜像：

```xml
 <mirror><id>nexus-aliyun</id>
	<mirrorOf>central</mirrorOf>
	<name>Nexus aliyun</name>
	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
	</mirror>
```

#### 5、配置maven的环境变量：

就是将maven解压的bin路径添加至path环境变量中。

![image-20211031033203659](./maven的安装与配置/image-20211031033203659.png)

#### 5、将Maven集成到IDEA中：

![image-20211031033222800](./maven的安装与配置/image-20211031033222800.png)

![image-20211031033237969](./maven的安装与配置/image-20211031033237969.png)

然后apply一下；至此maven就配置成功了。
