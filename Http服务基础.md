### Http服务基础

#### 1. http协议版本

```
（1）HTTP/0.9: 原型版本，功能简陋
	HTTP 0.9是第一个版本的HTTP协议，已过时。只允许客户端发送GET请求，且不支持请求头。由于没有协议头，造成了HTTP 0.9协议只支持一种内容，即纯文本。不过网页仍然支持用HTML语言格式化，同时无法插入图片。HTTP/0.9具有典型的无状态性，每个事务独立进行处理，事务结束时就释放这个连接。由此可见，HTTP协议的无状态特点在其第一个版本0.9中已经成型。一次HTTP 0.9的传输首先要建立一个由客户端到Web服务器的TCP连接，由客户端发起一个请求，然后由Web服务器返回页面内容，然后连接会关闭。如果请求的页面不存在，也不会返回任何错误码。
（2）HTTP/1.0: 第一个广泛使用的版本，支持MIME
	HTTP协议的第二个版本，第一个在通讯中指定版本号的HTTP协议版本，至今仍被广泛采用。相对于HTTP 0.9 增加了如下主要特性：
	请求与响应支持头域
	响应对象以一个响应状态行开始
	响应对象不只限于超文本
	开始支持客户端通过POST方法向Web服务器提交数据，支持GET、HEAD、POST方法。（短连接）每一个请求建立一个TCP连接，请求完成后立马断开连接。这将会导致2个问题：连接无法复用，head of line blocking。连接无法复用会导致每次请求都经历三次握手和慢启动。三次握手在高延迟的场景下影响较明显，慢启动则对文件类请求影响较大。head of line blocking会导致带宽无法被充分利用，以及后续健康请求被阻塞。
 (3) HTTP/1.1: 增强了缓存功能
（4）HTTP/2.0 ：新一代http协议
```

#### 2.完整http请求流程

``` 
（1）建立或处理TCP连接，接收或拒绝请求；
（2）接收请求：接收来自于网络请求报文中对某资源的一次请求过程；
 并发响应模型（Web I/O）
 	单进程I/O结构：启动一个进程处理用户请求，而且一次只处理一个，多个请求被串行执行。
 	多进程I/O结构：并行启动多个进程，每个进程响应一个请求。
 	复用I/O结构：一个进程响应n个请求，有两种实现方式。
 		多线程模型：一个进程生成N个线程，每个线程响应一个用户请求。
 		事件驱动：event-driven
 	复用多进程I/O结构：启动多个进程，每个进程响应n个请求。
（3）处理请求：对请求报文进行解析，并获取请求的资源及请求的方法。
 	元数据：请求报文的首部 
 		<METHOD> <URL> <VERSION>
 		HOST:www.dangyang.com 请求主机名
 		Connection:
 （4）访问资源
 	web服务器-存放web资源的服务器，负责向请求者提供对方请求的静态资源，【或动态运行后生成的资源】。这些资源存放于本地文件系统某路径下，此路径通常为DocRoot。httpd服务默认DocRoot为/var/www/html。
 	web服务器资源路径映射方式:docroot/alias/虚拟主机docroot/用户家目录docroot
 （5）构建响应报文
 	响应资源的MIME类型处理：显示分类、魔法分类、协商分类。
 	URL重定向：web服务器构建的响应并非客户端请求的资源，而是资源另外一个访问路径。
 （6）发送响应报文
 （7）记录日志
```

#### 3.http服务器程序 & 应用程序服务器

```
http服务器程序：httpd(apache) / nginx / lighttpd
应用程序服务器：IIS / tomcat,jetty,jboss,resin/ webshpere,weblogic,oc4j
服务排名站点查询：www.netcraft.com
```

#### 4.httpd服务的安装配置使用

httpd是apache(a patchy server)下的一款应用服务程序; ASF(apache software foundation)

##### 4.1 httpd特性（httpd-2.2:event为测试使用，httpd-2.4:event为生产使用）

```
1.高度模块化：core + modules
2.DSO: Dynamic shared object
3.MPM: Mult Processing Modules
	prefork:多进程模型，每个进程响应一个请求。一个主进程，负责生成n个子进程（工作进程），每个工作进程处理一个用户请求，即使没有用户请求，也会预先生成多个空闲进程，随时等待请求到达，最大不会超过1024个。
	worker:多线程模型，每个线程响应一个请求。一个主进程，生成多个子进程，每个子进程负责生成多个线程，每个线程响应一个请求。
	event:事件驱动模型，每个线程响应n个请求。一个主进程，生成m个子进程，每个进程响应n个请求。
```

##### 4.2 httpd功能特性

```
1.虚拟主机：ip / port / FQDN
2.CGI:Common Gateway Interface - 通用网关接口
3.反向代理
4.负载均衡
5.路径别名
6.丰富的用户认证机制（basic、digest）
7.支持第三方模块
```

5. ##### httpd服务安装方法(rpm和编译安装

   ```
   Centos6:
   (1)程序环境【rpm -qc httpd*】
   	配置文件： /etc/httpd/conf/httpd.conf ; /etc/httpd/conf.d/*.conf
   	脚本文件：/etc/rc.d/init.d/httpd - 配置文件（/etc/sysconfig/httpd）
   	主程序文件：/usr/sbin/httpd ; /usr/sbin/httpd.event; /usr/sbin/httpd.worker
   	日志文件： /var/log/httpd/{access_log/error_log}
        站点文档目录：/var/www/html
   (2)配置文件组成
   	# grep "Section" /etc/httpd/conf/httpd.conf
   	### Section 1: Global Enviroment
   	### Section 2: 'Main' server configuration
   	### Section 3: Virtual Hosts
   (3)常用配置
   ---修改监听的IP和Port---
   	Listen [IP:]PORT - 省略IP表示监听本机的所有IP；Listen可重复出现多次。
   ---持久连接---
   	1.Persistent Connection:连接建立，每个资源获取完成后不会断开连接，而是继续等待其他的请求完成。
   	2.断开策略：请求资源量限制(100),时间限制(可动态调整)。
   	3.弊端：对于并发访问量较大的服务器，持久连接功能会使有些请求的得不到响应而丢失。
   	4.折中方案：使用较短的持久连接方案。httpd-2.4支持毫秒级持久时间。
   	KeepAlive On|Off
   	MaxKeepAliveRequests #
   	KeepAliveTimeout #
   ---日志设定---
   	错误日志：ErrorLog logs/error_log  #此处为相对路径
   		日志级别：debug,info,notice,wawn,error,crit,alert,emerg(级别由低到高)
   	访问日志：CustomLog logs/access_log combined
   	LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\""
   	%h:客户端IP地址
   	%l:Remote logname "-"表示为空
   	%u:Remote user(Remote user (from auth; may be bogus if return status (%s) is 401)) 认证用户名
   	%t:Time the request was received (standard english format)  服务器接收请求的时间
   	%r:	First line of request 请求报文的首行信息(method URL HTTP/1.1)
   	%>s:响应状态码
   	%b:响应报文的大小，单位为字节，不包括响应报文首部
   	%{Referer}i:请求报文当中“referer”首部的值，当前资源的访问入口，即从那个页面中的超链接跳转而来。
   	%{User-Agent}i:请求报文当中“User-Agent'首部的值，即发出请求用到的应用程序。
   	参考文档：httpd.apache.org/docs/2.2/mod/mod_log_config.html
   ---路径别名---
   
    Centos7:
    
   ```

   

   -