---
title: 使用git联合github或者gitee来管理文件
date: 2021-09-06 16:27:12
tags: git
categories: git相关
---

1、首先在gitee或者github上创建一个仓库：

2、然后在电脑上，将刚刚新建的一个仓库给克隆下来，将所需要上传的文件拷贝到仓库中。

3、然后输入：

```
git status
```

查看文件状态：如果是红色的，那么就还没有添加到待缓冲区中。

<!--more-->

需要输入：

```
git add .
```

4、然后再提交:

```
git commit -m "first commit"
```

5、最后部署到仓库中：

```
git push -u origin master
```





注意：如果说，部署后出现一下错误：

```
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

```

说明已经与仓库失去关联：

需要输入一下命令来重新关联仓库：

```
git remote -v
git remote add origin 仓库地址
```

之后再重新git push -u origin master即可。

