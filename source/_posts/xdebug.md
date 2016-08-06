title: php 之 xdebug
date: 2015-03-21 14:45:44
categories: code
tags: [php]
---
## PHP配置调试工具XDebug
### 1. 去xdebug的官方网站那http://www.xdebug.org 上下载与php版本对应的xdebug，我的是window系统，下载的.dll文件。
### 2. 把php_xdebug.dll放入php文件夹的ext里面（php的扩展文件都放在此处，统一一下而已）
### 3. 打开php配置文件php.ini,在最后添加以下代码把xdebug.dll加载到php环境中
``` bash
[Xdebug]
zend_extension_ts="D:/php/ext/php_xdebug.dll"   //这里是绝对路径
xdebug.auto_trace=On
xdebug.collect_params=On
xdebug.collect_return=On
xdebug.trace_output_dir="c:/php5/debuginfo"
xdebug.profiler_enable=On
xdebug.profiler_output_dir="c:/php5/debuginfo"
```
备注：如果PHPINFO中还是没有XDebug的信息，把上面配置文件中的
``` bash
zend_extension_ts="D:/php/ext/php_xdebug.dll"
```
改为
``` bash
zend_extension="D:/php/ext/php_xdebug.dll"
```
这里点很重要，这个问题让我郁闷了很久，结果就是这样解决的，具体也不知道是为什么，网上说PHP5.3以后才是通过zend_extension这种方式加载，不管他了，搞定才是王道


以下是参数解释：
``` bash
zend_extension_ts="c:/webserver/php5/ext/php_xdebug.dll"
```
加载xdebug模块。这里不能用extension=php_xdebug.dll的方式加载，必须要以zend的方式加载，否则安装上后，phpinfo是显示不出xdebug这个项的。
``` bash
xdebug.auto_trace=on;
```
自动打开“监测函数调用过程”的功模。该功能可以在你指定的目录中将函数调用的监测信息以文件的形式输出。此配置项的默认值为off。
``` bash
xdebug.collect_params=on;
```
打开收集“函数参数”的功能。将函数调用的参数值列入函数过程调用的监测信息中。此配置项的默认值为off。
``` bash
xdebug.collect_return=on
```
打开收集“函数返回值”的功能。将函数的返回值列入函数过程调用的监测信息中。此配置项的默认值为off。
``` bash
xdebug.trace_output_dir=”c:Tempxdebug”
```
设定函数调用监测信息的输出文件的路径。
``` bash
xdebug.profiler_enable=on
```
打开效能监测器。
``` bash
xdebug.profiler_output_dir=”c:Tempxdebug”;
```
设定效能监测信息输出文件的路径


注意：php5.3安装xdebug的时候，有些地方和php5.2安装有所区别，php5.3使用zend_extension

以下是PHP5.3的配置信息：
``` bash
[Xdebug]
zend_extension=D:/Program Files/wamp/bin/php/php5.3.3/ext/php_xdebug-2.1.2-5.3-vc6.dll
;是否开启自动跟踪
xdebug.auto_trace = On
;是否开启异常跟踪
xdebug.show_exception_trace = On
;是否开启远程调试自动启动
xdebug.remote_autostart = On
;是否开启远程调试
xdebug.remote_enable = On
;允许调试的客户端IP
xdebug.remote_host=192.168.1.227
;远程调试的端口（默认9000）
xdebug.remote_port=9000
;调试插件dbgp
xdebug.remote_handler=dbgp
;是否收集变量
xdebug.collect_vars = On
;是否收集返回值
xdebug.collect_return = On
;是否收集参数
xdebug.collect_params = On
;跟踪输出路径
xdebug.trace_output_dir="D:/Program Files/wamp/bin/php/debuginfo"
;是否开启调试内容
xdebug.profiler_enable=On
;调试输出路径
xdebug.profiler_output_dir="D:/Program Files/wamp/bin/php/debuginfo"

```
### 4. 重启apache，安装完成

可以通过查看phpinfo信息确定xdebug是否集成到了php环境中，出现如下内容，代表xdebug可以在php中使用了