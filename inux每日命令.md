[TOC]

# Linux每日命令

#### 一.归档压缩

#### 1. 1 tar

###### 1.1.1 简介

```
tar命令可以为linux的文件和目录创建档案。利用tar，可以为某一特定文件创建档案（备份文件），也可以在档案中改变文件，或者向档案中加入新的文件。利用tar命令，可以把一大堆的文件和目录全部打包成一个文件，这对于备份文件或将几个文件组合成为一个文件以便于网络传输是非常有用的。
```

######  1.1.2 参数

| 参数          | 简介                     |
| ------------- | ------------------------ |
| -c : --create | 创建新的备份文件         |
| -t : --list   | 列出备份文件内容         |
| -z ：--gzip   | 通过gzip指令处理备份文件 |
| -f ：--file   | 指定备份文件             |
| -x            | 还原备份文件             |
| -v            | 显示指令执行过程         |

###### 1.1.3 常用组合

```
# 打包压缩（cvf/zcvf/jcvf）
tar -cvf log.tar log2019.log		#仅打包，不压缩
tar -zcvf log.tar.gz log2019.log	#打包后，以gzip压缩
tar -jcvf log.tar.bz2 log2019.log	#打包后，以bzip压缩

# 文件查看(ztvf)
tar -ztvf log.tar.gz	#查询tar包内的文件

#tar包解压(全部-zxvf)
tar -zxvf /opt/soft/test/log.tar.gz

#tar包部分解压
#tar -ztvf /opt/soft/test/log.tar.gz
#tar -zxvf /opt/soft/test/log.tar.gz
```

#### 1.2. gzip

###### 1.2.1 简介

```
gzip命令用来压缩文件。 压缩后缀名.gz,通常和tar命令联合使用，压缩率通常为%60~70%。可指定多个文件同时压缩。
```

###### 1.2.2 参数

| 参数        | 简介                     |
| ----------- | ------------------------ |
| gzip        | 压缩                     |
| -f  --force | 强制压缩                 |
| -r          | 递归                     |
| -t          | 测试压缩文件是否正确无误 |
| -v          | 显示过程                 |
| -d          | 解压                     |

###### 1.2.3 常用组合

```
1.压缩文件
# gzip [filename...]
2.递归压缩目录
# gzip -rv [dirname]
3.压缩tar备份文件
# gzip -r filename.tar

4.解压文件，列出详细信息
# gzip -dv [filename...]
5.递归解压目录
gzip -dr [dirname]
```

#### 二、网络监控

#### 2.1 nethogs

###### 2.1.1 简介

```
常用Linux开源网络监视工具：
	iftop:检查网络带宽使用情况
	netstat:查看接口统计报告
	top：监控系统当前运行进程
nethogs:可以按照进程实时统计网络带宽利用率
```

###### 2.1.2 参数

| 参数             | 简介                                       |
| ---------------- | ------------------------------------------ |
| -d [seconds]     | 刷新间隔                                   |
| [device name...] | 检测给定某个或某些设备带宽（default:eth0） |
| -h               | 帮助                                       |

###### 2.1.3 实例

```
1.设置5s刷新频率
# nethogs -d 5
2.监视多接口
# nethogs eth0 eth1
```

###### 2.1.4 交互命令

```
m : 修改单位（B/K/M）
r : 按接收流量排序
s : 按发送流量排序
q : 退出命令提示符
```

#### 三、进程管理

#### 3.1.top

###### 3.1.1 简介

```
top: 实时动态查看系统整体运行情况，可用快捷键管理。
top - 20:17:10（当前系统时间） up 8 days（系统运行天数）, 11:27,  1 user（用户登陆数）,  load average: 0.00, 0.01, 0.05（系统平均负载）Tasks:  74 total（总进程数）,2 running（运行数）,72 sleeping（睡眠数）,  0 stopped,   0 zombie（冻结）%Cpu(s):  0.7 us（用户空间）,  0.7 sy（内核）,  0.0 ni（用户空间改变优先级）, 98.7 id（空闲）,  0.0 wa（等待输入输出的CPU时间百分比）,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1883724 total,   149464 free,   798028 used,   936232 buff/cache（内核缓冲）
KiB Swap:        0 total,        0 free,        0 used.   908124 avail Mem
```

###### 3.1.2 交互命令

```
M: 按内存大小排序
P: 按CPU使用百分比排序
T: 按时间/累计时间排序
c: 切换显示命令名称和完整命令行
m: 切换显示内存信息
l: 切换显示平均负载和启动时间信息
```

#### 四、数据同步

#### 4.1 rsync

###### 4.1.1 简介

```
rsync命令时远程数据同步工具， 可通过LAN/WAN快速同步多台主机间的文件，使用“rsync算法”。此算法只传送两个文件的不同部分，而不是每次都整份传送，速度相当快。
```

###### 4.1.2 语法

```
rsync [OPTION]... SRC DEST  	#本地文件拷贝
rsync [OPTION]... SRC [USER@]host:DEST		#拷贝本地文件至远程服务器目录（ssh/rsh）
rsync [OPTION]... [USER@]host:SRC DEST		#拷贝远程主机文件至本地(ssh/rsh)
rsync [OPTION]... [USER@]host::SRC DEST		#从远程rsync服务器中拷贝文件到本地机
rsync [OPTION]... SRC [USER@]host::DEST		#从本地机器拷贝文件到远程rsync服务器
rsync [OPTION]... rsync://[USER@]host[:port]/SRC [DEST]		#未知
```

###### 4.1.3 参数

| 参数            | 简介                                                |
| --------------- | :-------------------------------------------------- |
| -a: --archive   | 以递归方式传输文件，并保持所有文件属性              |
| -v: --verbose   | 详细模式输出                                        |
| -z：--compress  | 对备份的文件在传输时进行压缩处理                    |
| --progres       | 在传输时现实传输过程                                |
| -b: --backup    | 创建备份，目的存在同样文件，老文件重命名为~filename |
| --backup-dir    | 将备份文件存放在设定目录下                          |
| --suffix=SUFFIX | 定义备份文件前缀                                    |
| -q: --quiet     | 精简输出模式                                        |
| -r: --recursive | 对子目录以递归模式处理                              |
| --delete        | 删除DST中SRC没有的文件                              |

###### 4.1.4 常用组合

```
# rsync -avz --progres --delete src dest
```

#### 五、Linux系统管理

#### 5.1  uptime

###### 5.1.1 简介

```
uptime: 打印系统总共运行了多长时间和系统的平均负载
[root@delicloud000 ~]# uptime
 19:59:04 up 90 days, 20:31,  2 users,  load average: 0.49, 0.51, 0.51
 显示内容为：
 系统当前时间/主机运行时间/用户链接/用户连接数/系统平均负载（1min/5min/15min）
```

###### 5.2.2 系统平均负载

```
解释：系统平均负载是指在特定时间间隔内运行队列中的平均【进程数】。
1. 每个CPU内核当前活动进程数<3,性能良好；>5 性能存在严重问题。
2. 双核CPU,当Load Average = 6 时，机器已被充分利用。
```

#### 六、文件管理

#### 6.1 diff

###### 6.1.1 简介

```

```



#### 七、shell脚本

#### 7.1 exec

###### 7.1.1 简介

```
exec: 调用并执行指令命令。通常用在shell脚本中，可以调用其他命令。
```

###### 7.1.2 实例

1.在当前目录下查找以.txt结尾的文件并找出含有 字符长“bin”的行。

```
find ./  -type f -name "*.txt" -exrc grep "bin" {} \；
解释：
{} ： 代表由find找到的内容
[ -exec ... \；]: 找到数据额外处理动作
```

2.在目录中查找更改时间在n日以前的文件并删除

```
find . -type f -mtime +14 -exec rm {} \;
```

###### 7.3 export

###### 7.3.1 简介

```

```



#### 八、硬盘监控

#### 8.1 sar

###### 8.1.1 简介

```

```

#### 8.2 iostat

###### 8.2.1 简介

```

```

#### 8.3 iotop

###### 8.3.1 简介

```
iotop: 用来监视磁盘I/O使用状况的top工具。
优点：可以查看每个进程IO使用情况
```

###### 8.3.2 选项

```
-o:只显示有io操作的进程
-d SEC:间隔SEC秒显示一次
-p PID:监控进程的pid
```

######  8.3.2 常用快捷键

```
1.o:只显示有IO输出的进程
2.p:进程/线程显示方式切换
3.<>:改变排序方式
4.a:显示累加使用量
```

#### 九.操作系统

##### 9.1 uname

```
用于打印当前系统相关信息（内核版本号、硬件架构、主机名称、操作系统类型等）
常用选项：
-a / --all: 显示全部信息
--help: 获取帮助
-n / --nodename: 显示在网络上的主机名称
```



###### MySQL运维

###### 2.1 MySQL查看死锁和解除锁

```
step1:查看在锁的事务

# mysql -u[user] -p[passwd] -h[host] -e 'SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX\G；' 

step2:杀死进程id(fields:trx_id)
# mysql -u[user] -p[passwd] -h[host] -e 'SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX\G；' | grep -C 2 SELECT | grep  trx_mysql_thread_id|awk '{print $NF}'   

```

编程脚本：

```
vim kill_select.sh
	#!/bin/bash
	id=$（mysql -u[user] -p[passwd] -h[host] -e 'SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX\G' | grep -C 2 SELECT | grep trx_mysql_thread_id|awk '{print $NF}'）
	for n in $id
	do 
		echo $n
		mysql -u[user] -p[passwd] -h[host] -e "kill $n"
	done
```

##### 2.2 查看MySQL运行线程(mysql show processlist)

###### 2.2.1 简介

```
1.show full processlist返回的结果是实时变化的，是对mysql链接执行的快照，用来处理突发事件非常有用。
2.一般用show processlist或show full processlist都是为了查看当前mysql是否有压力，都在跑什么语句，有没有慢SQL执行之类的。可以看到有多少链接属，那些线程有问题（time时间越长，越需要关注）等。
```

###### 2.2.2 命令详解

```
1.通过SHOW FULL PROCESSLIST查看
# mysql -u[user] -p[password] -h[host] -e "SHOW FULL PROCESSLIST\G;"
2.通过查询链接线程相关的表来查看快照
mysql -u[user] -p[password] -h[host] -e "select id, db, user, host, command, time, state, info from information_schema.processlistorder by time desc;"
3.通过navicat中的【工具】=> 【服务器监控】进行查看结果
```

###### 2.2.3 查询结果详解

```
结果示例：
mysql> SHOW FULL PROCESSLIST\G
*************************** 1. row ***************************
Id: 1
User: system user
Host:
db: NULL
Command: Connect
Time: 1030455
State: Waiting for master to send event
Info: NULL
```

```
各列解释：
Id：链接mysql 服务器线程的唯一标识，可以通过kill来终止此线程的链接。
User：当前线程链接数据库的用户。
Host：显示这个语句是从哪个ip 的哪个端口上发出的。可用来追踪出问题语句的用户。
db: 线程链接的数据库，如果没有则为null。
Command: 显示当前连接的执行的命令，一般就是休眠或空闲（sleep），查询（query），连接（connect）。
Time: 线程处在当前状态的时间，单位是秒
State：显示使用当前连接的sql语句的状态，很重要的列，后续会有所有的状态的描述，请注意，state只是语句执行中的某一个状态，一个 sql语句，已查询为例，可能需要经过copying to tmp table，Sorting result，Sending data等状态才可以完成
Info: 线程执行的sql语句，如果没有语句执行则为null。这个语句可以使客户端发来的执行语句也可以是内部执行的语句

```

```
Action:
由于Command的状态大部分都是sleep对我们分析问题没什么作用，所以我们可以通过如下语句来排除sleep状态的线程.
Fun1:
# mysql -u[user] -p[password] -h[host] -e "SHOW FULL PROCESSLIST\G;" | grep -v Sleep
Fun2:
# select id, db, user, host, command, time, state, info from information_schema.processlist where command != 'Sleep' order by time desc ;
这样就过滤出来哪些是正在干活的，然后按照消耗时间倒叙展示，排在最前面的，极大可能就是有问题的链接了，然后查看 info 一列，就能看到具体执行的什么 SQL 语句了，针对分析 
```

###### 2.2.4 kill使用

通过前面的查询，我们查到了问题sql，通常会kill掉这个链接的线程,具体看官网文档：https://dev.mysql.com/doc/refman/5.7/en/kill.html

```
-- 查询执行时间超过2分钟的线程，然后拼接成 kill 语句
select concat('kill ', id, ';')
from information_schema.processlist
where command != 'Sleep'
and time > 2*60
order by time desc 

```

###### 2.2.5 常见问题

```
些问题会导致连锁反应，而且不太好定位，有时候以为是慢查询，很可能是大多时间是在等在CPU、内存资源的释放，所以有时候同一个查询消耗的时间有时候差异很大
总结了一些常见问题：
CPU报警：很可能是 SQL 里面有较多的计算导致的
连接数超高：很可能是有慢查询，然后导致很多的查询在排队，排查问题的时候可以看到”事发现场“类似的 SQL 语句一大片，那么有可能是没有索引或者索引不好使，可以用：explain 分析一下 SQL 语句
```

# Redis运维

##### 3.1 常用数据类型

##### 3.2 常用命令集合

1.登陆redis数据库并查询keys

```
redis-cli -h "ip" -p "port" -a "password" -n "dbnumber" KEYS *[key]*
```

2.登陆redis数据库并删除相关keys

```
redis-cli -h "ip" -p "port" -a "password" -n "dbnumber" KEYS *[key]* | xargs redis-cli -h "ip" -p "port" -a "password" -n "dbnumber" del
```

#### 第6章 复制

```
在分布式系统中为解决单点问题，通常会把数据复制多个副本部署到其他机器，满足故障恢复和负载均衡等需求。Redis提供了复制功能，实现了相同数据的多个Redis副本。复制功能是高可用Redis的基础，哨兵和集群都是在复制基础上实现高可用的。复制也是Redis日常运维的常见维护点。知识点如下：
1.复制使用方式：如何建立或断开复制、安全性、只读等
2.复制可支持的拓扑结构及使用场景
3.复制原理：建立复制、全量复制、部分复制、心跳等
4.开发运维常见问题：读写分离、数据不一致、规避全量复制
```

#### 6.1 建立复制

```
参与复制的Redis实例划分为主（master）和从（slave）两个节点。默认Redis都是主节点，一个主节点可对应多个从节点，一个从节点只对应一个主节点。数据流单项复制，只能由主节点复制到从节点。
```

1. 配置复制方式

   ```
   1. 编辑配置文件，加入下列命令
   # slaveof {masterHost} {masterPort}
   2. 服务端启动加启动参数
   # redis-server --slaveof {masterHost} {masterPort}
   3. 运行redis客户端，执行命令
   # redis-cli
   127.0.0.1:6390> slaveof 127.0.0.1 6397
   ```

   

# nginx之php(thinkphp)配置

```
server {
        listen 8088;
#        server_name dladmins.feioou.com dlapis.feioou.com labeler.delicloud.com;
        index index.html index.htm index.php;
        root  /www/label/public;
	access_log  /var/log/nginx/label_access.log;
        location / {
        if (!-e $request_filename) {
            rewrite ^(.*)$ /index.php?s=/$1 last;
            break;
        }
	}

    location ~ [^/]\.php(/|$) {
      fastcgi_pass 127.0.0.1:9000;
      fastcgi_index index.php;
      include fastcgi.conf;
    }
	location /download {
	    alias /home/www/deli/public/download/;
	}
        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires      30d;
        }

        location ~ .*\.(js|css)?$
        {
            expires      12h;
        }

        location ~ /\.
        {
            deny all;
        }
    }

```

