---
title: redis报错RedisCommandExecutionException
date: 2022-12-15 09:42:26
tags: redis
categories: redis相关
---

##### 1、redis执行后报错：

```log
Error in execution; nested exception is io.lettuce.core.RedisCommandExecutionException: MISCONF Redis is configured to save RDB snapshots, but is currently not able to persist on disk. Commands that may modify the data set are disabled. Please check Redis logs for details about the error.
```

这是因为redis强制杀掉端口后，导致redis的快照不能持久化。

##### 2、解决方法：

  进入redis安装目录，找到redis.conf文件，修改如下配置为no,然后重启redis即可。

```xml
stop-writes-on-bgsave-error 
```

