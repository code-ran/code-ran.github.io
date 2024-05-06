---
title: vue初始化报错
date: 2021-10-26 14:12:21
tags: vue
categories: vue相关
---

1、当我们在构建vue 项目时：会报禁止在该系统上运行脚本的错误与。

```
vue init webpack vue_test
```

<!--more -->

![image-20211025174128857](./vue初始化报错/image-20211025174128857.png)

解决方法：

   在小娜搜索框搜索：powerShell，然后以管理员身份打开，输入如下命令进行执行：

```
set-ExecutionPolicy RemoteSigned
```

之后输入yes回车即可。然后去vscode里执行vue初始化程序，就能成功了。
