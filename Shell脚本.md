# Shell 脚本知识

#### 一、编写规范

###### 1.1 Shell常用规范

```  
1.脚本开头书写规范如下：
	#!/bin/bash or #!/bin/sh
	#Date	18.55 2019-02-24
	#Author	Create by dangyang
	#Blog:http://oldboy.blog.51cto.com
	#Description:This scripts function is ...
	#Version: 1.1
	备注：可修改 ~/.vimrc 配置文件配置vim编辑文件时自动加上以上信息功能
2.注释必须为英文，禁用中文
3.使用sh/bash启动脚本：脚本没有可执行权限或脚本开头未指定解释器，常用的脚本启动方式有一下几种
	(1)sh/bash script-name
	(2) path/script-name  or ./script-name (需要执行权限，否则使用绝对路径和相对路径方法无法执行)
	(3) source script-name or . script-name : 读入或加载指定的shell脚本，在当前父进程中运行，不受权限约束。其他几种执行方式将常见创建子进程。
	(4)sh<script-name / cat script-name|sh
4.Shell脚本应存放在固定的路径下
5.Shell脚本的命名以.sh为扩展名
```

###### 1.2 shell书写规范

```
1.成对的符号应尽量一次性写出来，然后退格增加内容。{}、[]、''、``、""
2.中括号[]两端至少有一个空格。
3.对于流程语句，应一次性将格式写完，再添加内容。
    if 条件内容
        then
            内容
    fi
    for
    do
        内容
    done
4.通过缩进，让代码具有可读性
5.定义常规变量字符串变量值应加双引号，并且等号前后不能有空格，需要强引用的加单引号（所见及所得），命令引用使用反引号。
6.脚本中的单，双，反引号必须为英文状态下的符号。
```

###### 1.3 shell变量定义规范

```
可参考学习系统自带的/etc/init.d/functions函数库脚本的定义思路
1. OldboyAge=1	#每个单词首字母大写
2. oldboy_age=1	#单词之间用“_”
3. oldboyAgeSex=1	#小驼峰法，一般用于变量的定义/ 大驼峰法，一般用于类名定义
4. OLDBOYAGE=1	#单词全大写语法
```

[TOC]



#### 二、面试题集锦

1.CentOS Linux系统默认的Shell是什么？

```
bash
# echo $SHELL
# grep root /etc/passwd
```

2.echo $user返回的结果是（）？

```
# cat test.sh
  user=`whoami`
# sh test.sh
# echo $user
答案：返回空值
解释：使用sh执行脚本，此时会创建子进程来执行此shell,脚本执行完毕以后回退到父shell,因此，变量（包括函数）值等无法保留。
```

结论：

```
1.子shell会直接继承父shell脚本变量，反之不然。
2.若希望反过来继承，就要用source或.，在父shell脚本中事先加载子shell.
```



#### 三、常用操作

##### 3.1.查看Linux版本及bash版本

```
# cat /etc/redhat-release
# bash --version
```

##### 3.2.检测bash软件漏洞

```
参考链接：https://blog.csdn.net/jingxia2008/article/details/39637085
1.检测bash是否有漏洞
# env x='() {:;}; echo becareful' bash -c "echo this is the test"
执行脚本若打出"becareful"，则证明bash软件有安全漏洞，执行以下命令进行升级
2.升级
# yum -y update bash
# rmp -qa bash
```

##### 3.3shell变量

###### 3.3.1 查看shell变量

```
1. set:输出所有的变量，包括全局变量和局部变量
	-O:显示bash shell的所有参数配置信息
2. env:只显示全局变量
3. declare:输出所有的变量、函数、和已经导出的变量
```

###### 3.3.2 设置shell环境变量

```
1.export 变量名=value
2.变量名=value；export 变量名
3.declare -x 变量名=value
```

###### 3.3 .3取消本地变量和环境变量

```
# echo $USER
# unset USER	
```

###### 3.3.4 用户环境变量配置

```
1.首选编辑 ~/.bashrc
2.编辑/root/.bash_profile
```

###### 3.3.5 全局环境变量配置

```
1. /etc/profile
2. /etc/bashrc
3. /etc/profile.d/ (设置登录提示):若要在登录之后初始化或显示加载内容，则把脚本放在/etc/profile.d/即可。
4. cat /etc/motd (设置登录提示方式2)
```

示例：定义Java环境

```
vim /etc/profile
export JAVA_HOME=/usr/local/src/java/
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH

```

###### 3.3.6 系统级别默认全局变量

```
$HOME: 用户登录时进入的目录
$UID: 当前用户的UID(用户标识)，相当于[id -u]
$PWD: 当前工作目录的绝对路径
$SHELL:当前shell
$USER:当前user
```

##### 3.4.普通变量

###### 3.4.1 输出变量

```
1. $变量名 - 输出变量，有两种方式：$c和${c}
```

###### 3.4.2 命令结果赋值变量

```
1.变量名=`command`
2.变量名=$(command)
```

示例：按天打包网站的站点目录程序，生成不同的文件名（此为企业实战案例）

```
# CMD=$(date +%F)   将当前日期赋值给CMD
# echo $CMD
```

##### 3.5  shell变量总结

- [x] 若变量内容为连续的字符串或数字，赋值 时，变量内容两边可以不加引号，例如：a=1234  。
- [x] 变量内容很多时，如果有空格且希望解析内容中的变量， 需加双引号，例如a="/etc/rc.local/$USER"
- [x] 原样输出变量内容，加单引号，例如：a='$USER'
- [x] 变量内容是命令的解析结果的定义及赋值时，使用反引号(``)或$()；例如：a=$(ls)  ;
- [x] 输出变量内容： "$变量名"，常用"echo $变量名"的方式，也可以用printf;
- [x] $dbname_tname，当变量后面连续接有其他字符时，必须给变量加大括号{}，例如$dbname_tname 需改成${dbname}_tname.

#### 5.逻辑运算符、逻辑表达式

##### 5.1 逻辑运算符

###### 5.1.1 文件与目录

```
-f 常用！: 检测[文件]是否存在 if [ -f filename ] 
-d 常用！：检测[目录]是否存在
-e : 检测[搜索]是否存在，不做其他判断
-L ：检测是否为链接文件（symbolic link）
-S : 检测是否为socket文件
```

#### 四、shell技巧

##### 4.1 while read line

```

```

#### 五、企业案例

按天打包网站站点的目录程序，生成不同的文件名

```
#CMD=$(date +%F)
#tar zcf etc_$CMD.tar.gz  or tar zcf etc_$(date +%F).tar.gz /etc
```

