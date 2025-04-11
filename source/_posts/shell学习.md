---
title: shell学习
date: 2025-04-11 15:07:44
tags: shell学习
categories: shell相关
---

### Shell编程

#### shell脚本编写格式

shell脚本文件就是一个文本文件,  后缀名建议使用 `.sh` 结尾

格式:

(1)首行需要设置shell解释器的类型

```shell
#!/bin/bash
```

(2)单行注释

```shell
# 注释内容
```

(3)多行注释

```shell
:<<!
# 注释内容1
# 注释内容2
!
```

#### shell脚本执行方式

##### sh解释器执行

```shell
sh 脚本文件
```

##### bash解释器执行

```shell
bash 脚本文件
```

##### 仅路径执行

脚本文件自己执行需要具有可执行权限, 否则无法执行。

```shell
./ 脚本文件
```



#### Shell变量

##### 系统环境变量

是系统提供的共享变量，共享给所有的Shell程序使用。

shell配置文件分类:

(1)全局配置文件

```
/etc/profile
/etc/profile.d/*.sh
/etc/bashrc
```

(2)用户配置文件

```
当前用户/.bash_profile
当前用户/.bashrc
```

查看当前系统环境变量

```shell
env
```

查看系统环境变量和自定义变量

```
set
```

###### 常用系统环境变量

| 变量名称 | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| PATH     | 与windows环境变量PATH功能一样，设置命令的搜索路径，以冒号为分割 |
| HOME     | 显示当前用户主目录：/root                                    |
| SHELL    | 显示当前shell解析器类型：/bin/bash                           |
| HISTFILE | 显示当前用户执行命令的历史列表文件：/root/.bash_history      |
| PWD      | 显示当前所在路径：/root                                      |
| OLDPWD   | 显示之前的路径                                               |
| HOSTNAME | 显示当前主机名：itheima                                      |
| HOSTTYPE | 显示主机的架构，是i386、i686、还是x86、x64等：x86_64         |
| LANG     | 显示当前系统语言环境：zh_CN.UTF-8                            |

![image-20250406123802051](D:\github\code-ran.github.io\source\_posts\shell学习.assets\image-20250406123802051.png)

##### 自定义变量

###### 自定义局部变量

定义在一个脚本文件中的变量, 只能在这个脚本文件中使用的变量, 就是局部变量。

(1)语法格式

```shell
var_name=value
```

命名规则

```
1. 变量名称可以有字母,数字和下划线组成, 但是不能以数字开头
2. 等号两侧不能有空格
3. 在bash环境中, 变量的默认类型都是字符串类型, 无法直接进行数值运算
4. 变量的值如果有空格, 必须使用双引号括起来
5. 不能使用Shell的关键字作为变量名称
```

(2)查询变量语法

直接使用变量名查询

```shell
$var_name
```

使用花括号,花括号方式适合拼接字符串

```shell
${var_name}
```

(3)变量删除

```shell
unset var_name
```

###### 自定义常量

变量设置值以后不可以修改的变量叫常量。

(1)语法格式

```shell
readonly var_name
```

![image-20250406124855833](D:\github\code-ran.github.io\source\_posts\shell学习.assets\image-20250406124855833.png)

###### 自定义全局变量

在当前脚本文件中定义全局变量, 这个全局变量可以在当前Shell环境与子Shell环境中使用。

(1)父子shell环境

如果a.sh脚本文件中，执行了b.sh脚本文件，那么a.sh就是父shell环境.

(2)语法格式:

```shell
export var1 var2
```

例子

```shell
1. 创建2个脚本文件 demo2.sh 和 demo3.sh

2. 编辑demo2.sh 

   命令1:定义全局变量VAR4

   命令2: 执行demo3.sh脚本文件

3. 编辑demo3.sh

   输出全局变量VAR4

4. 执行demo2.sh脚本文件
```

![image-20250406131443261](D:\github\code-ran.github.io\source\_posts\shell学习.assets\image-20250406131443261.png)



##### 特殊变量

(1)$n

```shell
$n
```

用于接收脚本文件执行时传入的参数，**$0 用于获取当前脚本文件名称的**，**$1~$9, 代表获取第一输入参数到第9个输入参数**，第10个以上的输入参数获取参数的格式: **${数字}**, 否则无法获取

(2)执行脚本文件传入参数语法

```shell
sh 脚本文件 输入参数1 输入参数2 ...
```

(3)例子

```
1. 创建脚本文件demo4.sh
2. 编辑demo4.sh的文件内容
# 命令1: 打印当前脚本文件名字
# 命令2: 打印第1个输入参数
# 命令3: 打印第2个输入参数
# 命令4: 打印第10个输入参数
```

![image-20250406132616345](D:\github\code-ran.github.io\source\_posts\shell学习.assets\image-20250406132616345.png)

(2)$#

获取所有输入参数的个数

(3)$*和$@

都是获取所有输入参数, 用于以后输出所有参数。如果使用了双引号`$@`获取的就是参数列表对象, 每个参数都是一个独立字符串。

(4)$?

用于获取上一个Shell命令的退出状态码, 或者是函数的返回值。每个Shell命令的执行都有一个返回值, 这个返回值用于说明命令执行是否成功。一般来说, 返回0代表命令执行成功, 非0代表执行失败。

(5)$$

用于获取当前Shell环境的进程ID号



#### 自定义系统环境变量

当前用户进入Shell环境初始化的时候，会加载全局配置文件**/etc/profile**里面的环境变量, 供给所有Shell程序使用，以后只要是所有Shell程序或命令使用的变量, 就可以定义在这个文件中。

(1)重新加载/etc/profile文件数据更新系统环境变量

```shell
source /etc/profile
```



#### Shell字符串变量

**(1)使用单引号**

任何字符都会原样输出，不会去解析变量。

**(2)使用双引号**

包含了变量，那么该变量会被解析得到值，而不是原样输出。

**(3)不使用引号**

不被引号包围的字符串中出现变量时也会被解析，这一点和双引号`" "`包围的字符串一样。

但是字符串中不能出现空格，否则空格后边的字符串会作为其他命令解析。

**获取字符串长度：**

```shell
${#字符串变量名}
```



##### 拼接

(1)无符号拼接

```shell
echo ${var1}${var2}
```

(2)双引号拼接

```shell
echo "${var1}${var2}"
```

(3)混合拼接

```shell
echo ${var1}"&"${var2}
echo ${var1}'&'${var2}
```



##### 截取

| 格式                     | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| ${变量名:start:length}   | 从 string 字符串的左边第 start 个字符开始，向右截取 length 个字符。start从0开始 |
| ${变量名:start}          | 从 string 字符串的左边第 start 个字符开始截取，直到最后。    |
| ${变量名:0-start:length} | 从 string 字符串的右边第 start 个字符开始，向右截取 length 个字符。start从1开始, 代表右侧第一个字符 |
| ${变量名:0-start}        | 从 string 字符串的右边第 start 个字符开始截取，直到最后。    |
| ${变量名#*chars}         | 从 string 字符串左边第一次出现 *chars 的位置开始，截取 *chars 右边的所有字符。 |
| ${变量名##*chars}        | 从 string 字符串左边最后一次出现 *chars 的位置开始，截取 *chars 右边的所有字符。 |
| ${变量名%chars*}         | 从 string 字符串右边第一次出现 chars* 的位置开始，截取 chars* 左边的所有字符。 |
| ${变量名%%chars*}        | 从 string 字符串右边最后一次出现 chars* 的位置开始，截取 chars* 左边的所有字符 |



#### Shell数组

**定义**

```shell
array_name=(item1  item2 ...)
```

```shell
array_name=([索引下标1]=item1  [索引下标2]=item2  ...) 
```

注意：等号两边不能有空格

Shell 是弱类型的，它并不要求所有数组元素的类型必须相同

```shell
arr=(20 56 "http://www.itcast.cn/")
```

**获取**

1.通过下标获取元素值,index从0开始

```shell
${arr[index]}
```

> 注意使用`{ }`

2.获取值同时复制给其他变量

```shell
item=${arr[index]}
```

3.使用 `@` 或 `*` 可以获取数组中的所有元素

```shell
${arr[@]}
${arr[*]}
```

4.获取数组的长度或个数

```shell
${#arr[@]}
${#arr[*]}
```

5.获取数组指定元素的字符长度

```shell
${#arr[索引]}
```

**拼接**

```shell
array_new=(${array1[@]} ${array2[@]} ...)
array_new=(${array1[*]} ${array2[*]} ...)
```

**删除**

删除数组指定元素数据

```shell
unset array_name[index]
```

删除整个数组

```shell
unset array_name
```



#### Shell内置命令

使用type 来确定一个命令是否是内置命令：

```shell
type 命令
```

