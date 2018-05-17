title: php线程安全与非线程安全问题
date: 2017-11-09 22:39:46
categories: php
tags:
---
## 问题描述
究竟该如何选择PHP版本，什么是线程安全，什么又是非线程安全
## 解决方案
线程安全：线程安全就是多线程访问时，采用了加锁机制，当一个线程访问该类的某个数据时，进行保护，其他线程不能进行访问直到该线程读取完，其他线程才可使用。不会出现数据不一致或者数据污染
非线程安全(即线程不安全)：线程不安全就是不提供数据访问保护，有可能出现多个线程先后更改数据造成所得到的数据是脏数据。

我选择哪个版本？

IIS
如果你使用的是PHP的FastCGI IIS，你应该使用非线程安全（NTS）版本的PHP。

Apache
请使用Apache Lounge提供的Apache构建。 他们提供了针对x86和x64的Apache的VC9，VC11和VC14版本。 我们使用他们的二进制文件构建Apache SAPI。

如果你使用PHP作为apache.org（不推荐）的Apache模块，你需要使用旧的Visual Studio 6编译的VC6版本的PHP。 不要使用apache.org二进制文件的VC9 +版本的PHP。

使用Apache，您必须使用Thread Safe（TS）版本的PHP。


解释如下：

从2000年10月20日发布的第一个Windows版的PHP3.0.17开始的都是线程安全的版本，这是由于与Linux/Unix系统是采用多进程的工作方式不同的是Windows系统是采用多线程的工作方式。如果在IIS下以CGI方式运行PHP会非常慢，这是由于CGI模式是建立在多进程的基础之上的，而非多线程。一般我们会把PHP配置成以ISAPI的方式来运行，ISAPI是多线程的方式，这样就快多了。但存在一个问题，很多常用的PHP扩展是以Linux/Unix的多进程思想来开发的，这些扩展在ISAPI的方式运行时就会出错搞垮IIS。因此在IIS下CGI模式才是PHP运行的最安全方式，但CGI模式对于每个HTTP请求都需要重新加载和卸载整个PHP环境，其消耗是巨大的。

为了兼顾IIS下PHP的效率和安全，微软给出了FastCGI的解决方案。FastCGI可以让PHP的进程重复利用而不是每一个新的请求就重开一个进程。同时FastCGI也可以允许几个进程同时执行。这样既解决了CGI进程模式消耗太大的问题，又利用上了CGI进程模式不存在线程安全问题的优势。

因此，如果是使用ISAPI的方式来运行PHP就必须用Thread Safe(线程安全)的版本；而用FastCGI模式运行PHP的话就没有必要用线程安全检查了，用None Thread Safe(NTS，非线程安全)的版本能够更好的提高效率。