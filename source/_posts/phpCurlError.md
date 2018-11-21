title: php使用curl时报错解决方法
date: 2018-11-21 12:24:17
categories: php
tags:
---
# 问题总结
### 问：file_get_contents 开启https支持
答：配置php.ini 开启php_openssl.dll 扩展，开启allow_url_fopen=On
代码部分：
```php
	$w = stream_get_wrappers();
	echo 'openssl: ',  extension_loaded  ('openssl') ? 'yes':'no', "\n";
	echo 'http wrapper: ', in_array('http', $w) ? 'yes':'no', "\n";
	echo 'https wrapper: ', in_array('https', $w) ? 'yes':'no', "\n";
	echo 'wrappers: ', var_export($w);
```
作用：用于检测配置项是否存在https;

### 问：curl 抓取网站信息时，出现此错误：This site requires JavaScript and Cookies to be enabled.
答：修改curl 参数CURLOPT_USERAGENT为Google Bot，伪装成google bot去抓取
代码部分：
```php
	curl_setopt($ch, CURLOPT_USERAGENT, "Google Bot"); 
```
作用：伪装成谷歌机器去抓取页面信息