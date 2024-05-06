---
title: java使用file.delete方法删除文件失败
date: 2023-03-01 16:02:56
tags: java(IO)
categories: java IO相关
---

java在使用file.delete()方法对路径下的文件进行删除的时候，会遇到无法删除的情况。需要检查一下所有的文件流打开，使用之后是否对文件流都进行了关闭的操作。
