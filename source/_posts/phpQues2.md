title: 什么是CGI、FastCGI、PHP-CGI、PHP-FPM、Spawn-FCGI？
date: 2017-11-09 22:52:24
categories: php
tags:
---
## 名词解释
### CGI
CGI全称是“公共网关接口”(Common Gateway Interface)，HTTP服务器与你的或其它机器上的程序进行“交谈”的一种工具，
其程序须运行在网络服务器上。CGI可以用任何一种语言编写，只要这种语言具有标准输入、输出和环境变量。如php,
perl,tcl等
PS:是一种协议
### FastCGI
FastCGI像是一个常驻(long-live)型的CGI，它可以一直执行着，只要激活后，不会每次都要花费时间去fork一次(这是CGI
最为人诟病的fork-and-execute 模式)。它还支持分布式的运算, 即 FastCGI 程序可以在网站服务器以外的主机上执行并
且接受来自其它网站服务器来的请求。FastCGI是语言无关的、可伸缩架构的CGI开放扩展，其主要行为是将CGI解释器进程
保持在内存中并因此获得较高的性能。众所周知，CGI解释器的反复加载是CGI性能低下的主要原因，如果CGI解释器保持在
内存中并接受FastCGI进程管理器调度，则可以提供良好的性能、伸缩性、Fail- Over特性等等。
PS:同样是一种协议
### PHP-CGi
PHP-CGI是PHP自带的FastCGI管理器。
PS：PHP解释器
### PHP-FPM
PHP-FPM是一个PHP FastCGI管理器，是一个补丁，用于将fastCGI整合进php包中，php5.3.3后已经集成到php中，之前的版
本属于第三方包
PS：php-fpm是fastcgi进程的管理器，用来管理fastcgi进程的，它实现了Fastcgi的程序，被PHP官方收了
### Spawn-FCGI
Spawn-FCGI是一个通用的FastCGI管理服务器，它是lighttpd中的一部份，很多人都用Lighttpd的Spawn-FCGI进行FastCGI模
式下的管理工作
PS：一个通用的FastCGI管理服务器
## 工作原理及联系
CGI：每当客户请求CGI的时候，WEB服务器就请求操作系统生成一个新的CGI解释器进程(如php-cgi.exe，CGI的一个进程则处
理完一个请求后退出，下一个请求来时再创建新进程。当然，这样在访问量很少没有并发的情况也行。可是当访问量增大并
发存在，这种方式就不 适合了。于是就有了fastcgi。
fastCGI：FastCGI像是一个常驻(long-live)型的CGI，它可以一直执行着，只要激活后，不会每次都要花费时间去fork一次
（这是CGI最为人诟病的fork-and-execute 模式）。
#### FastCGI的工作原理
1、Web Server启动时载入FastCGI进程管理器（IIS ISAPI或Apache Module)
2、FastCGI进程管理器自身初始化，启动多个CGI解释器进程(可见多个php-cgi)并等待来自Web Server的连接。
3、当客户端请求到达Web Server时，FastCGI进程管理器选择并连接到一个CGI解释器。Web server将CGI环境变量和标
准输入发送到FastCGI子进程php-cgi。
4、FastCGI子进程完成处理后将标准输出和错误信息从同一连接返回WebServer。当FastCGI子进程关闭连接时，请求便
告处理完成。FastCGI子进程接着等待并处理来自FastCGI进程管理器(运行在Web Server中)的下一个连接。 在CGI模式中，
php-cgi在此便退出了。
在上述情况中，你可以想象CGI通常有多慢。每一个Web请求PHP都必须重新解析php.ini、重新载入全部扩展并重初始化
全部数据结构。使用FastCGI，所有这些都只在进程启动时发生一次。一个额外的好处是，持续数据库连接可以工作。
#### FastCGI的不足
因为是多进程，所以比CGI多线程消耗更多的服务器内存，PHP-CGI解释器每进程消耗7至25兆内存，将这个数字乘以50或
100就是很大的内存数。Nginx 0.8.46+PHP 5.2.14(FastCGI)服务器在3万并发连接下，开启的10个Nginx进程消耗150M内存
（15M10=150M），开启的64个php-cgi进程消耗1280M内存（20M64=1280M），加上系统自身消耗的内存，总共消耗不到2GB
内存。如果服务器内存较小，完全可以只开启25个php-cgi进程，这样php-cgi消耗的总内存数才500M。

#### PHP-CGI的不足
1、php-cgi变更php.ini配置后需重启php-cgi才能让新的php-ini生效，不可以平滑重启
2、直接杀死php-cgi进程,php就不能运行了。(PHP-FPM和Spawn-FCGI就没有这个问题,守护进程会平滑从新生成新的子进程。）

## 总结
首先，CGI是干嘛的？CGI是为了保证web server传递过来的数据是标准格式的，方便CGI程序的编写者。
web server（比如说nginx）只是内容的分发者。比如，如果请求index.html，那么webserver会去文件系统中找到这个文件
，发送给浏览器，这里分发的是静态数据。好了，如果现在请求的是/index.php，根据配置文件，nginx知道这个不是静态
文件，需要去找PHP解析器来处理，那么他会把这个请求简单处理后交给PHP解析器。Nginx会传哪些数据给PHP解析器呢？
url要有吧，查询字符串也得有吧，POST数据也要有，HTTP header不能少吧，好的，CGI就是规定要传哪些数据、以什么
样的格式传递给后方处理这个请求的协议。仔细想想，你在PHP代码中使用的用户从哪里来的。
　　当web server收到/index.php这个请求后，会启动对应的CGI程序，这里就是PHP的解析器。接下来PHP解析器会解
析php.ini文件，初始化执行环境，然后处理请求，再以规定CGI规定的格式返回处理后的结果，退出进程。web server
再把结果返回给浏览器。好了，CGI是个协议，跟进程什么的没关系。那fastcgi又是什么呢？Fastcgi是用来提高CGI程序
性能的。提高性能，那么CGI程序的性能问题在哪呢？"PHP解析器会解析php.ini文件，初始化执行环境"，就是这里了。
标准的CGI对每个请求都会执行这些步骤（不闲累啊！启动进程很累的说！），所以处理每个时间的时间会比较长。这明
显不合理嘛！那么Fastcgi是怎么做的呢？首先，Fastcgi会先启一个master，解析配置文件，初始化执行环境，然后再启
动多个worker。当请求过来时，master会传递给一个worker，然后立即可以接受下一个请求。这样就避免了重复的劳动，
效率自然是高。而且当worker不够用时，master可以根据配置预先启动几个worker等着；当然空闲worker太多时，也会
停掉一些，这样就提高了性能，也节约了资源。这就是fastcgi的对进程的管理。
那PHP-FPM又是什么呢？是一个实现了Fastcgi的程序，被PHP官方收了。
大家都知道，PHP的解释器是php-cgi。php-cgi只是个CGI程序，他自己本身只能解析请求，返回结果，不会进程管理
所以就出现了一些能够调度php-cgi进程的程序，比如说由lighthttpd分离出来的spawn-fcgi。好了PHP-FPM也是这么
个东东，在长时间的发展后，逐渐得到了大家的认可（要知道，前几年大家可是抱怨PHP-FPM稳定性太差的），也越来越流行。
参考地址：
https://www.cnblogs.com/wanghetao/p/3934350.html
https://www.lvtao.net/tool/885.html
