title: html的meta标签
date: 2015-03-21 14:28:41
categories: code
tags: [html]
---
```bash
meta
	|
	|
	name
	|
	|----------------
					|----keywords
					|----description
					|----application/name
					|----author
					|----generator
	|---------------|
	http-equiv
	|
	|----------------
					|----content-type
					|----refresh
					|----default-style
					|----content-language
					|----set-cookie
	content
	|
	|----------------
					|
					|
					|
	charset
```	
/**
 * IE兼容模式
 * @http-equiv 属性
 */
``` bash
<meta http-equiv="X-UA-Compatible" content="IE=8">
```
用法 分三种

## 1.网站描述 优化== name与content组合
	example:
``` bash
		<meta name="keywords" content="....">
		<meta name="description" content="....">
		<meta name="author" content="....">
```
## 2.http指令
	example:
``` bash
		<meta http-equiv="Content-Type" content="text/json">
		<meta http-equiv="refresh" content="3">
```
## 3.字符集
	example:
``` bash
		<meta charset="utf8">
```
## html5写法
	example:
``` bash
		<meta http-equiv="Content-Type" content="text/html;charset=utf8">
		header('Content-Type:text/html;charset=utf-8')
```		