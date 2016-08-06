title: Sublime Text3 之 emmet
date: 2016-07-07 20:41:04
tags: sublime
---
HTML 的生成 按tab键或 ctrl+e

## 生成HTML文档结构



```	bash
<!-- 1.命令：html:5 or ! -->
<!-- 效果如下： 开始 -->

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	
</body>
</html>

<!-- 结束 -->
```
```	bash
<!-- 2.命令：html:xt  -->
<!-- 生成html4过渡型 -->

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
<head>
	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
	<title>Document</title>
</head>
<body>
	
</body>
</html>

<!-- 结束 -->
```
```	bash
<!-- 3.命令：html:4s  or heml:xs -->
<!-- 生成html4严格型 -->

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
<head>
	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8" />
	<title>Document</title>
</head>
<body>
	
</body>
</html>

<!-- 结束 -->
```
```	bash
<!-- 4.命令 div p h1 span em input  直接tab 即可 -->
<!-- 生成html 常规标签 -->

<div></div>
<p></p>
<h1></h1>
<input type="text" />

<!-- 结束 -->
```
```bash
<!-- 5.生成带有 id 、class 的 HTML 标签: #为 id，. 为 class -->
<!-- 开始  如：div#header.header -->

<div id="header" class="header"></div>

<!-- 结束 -->
```
```bash
<!-- 6.生成后代：> -->
<!-- 开始  如：div#header>span.header -->

<div id="header"><span class="header"></span></div>

<!-- 结束 -->
```
```bash
<!-- 7.生成兄弟元素: + -->
<!-- 开始  如：div#header+div.siblings -->

<div id="header"></div>
<div class="siblings"></div>

<!-- 结束 -->
```
```bash
<!-- 8.生成上级元素： ^ -->
<!-- 开始 如：div#prev^div#next div#prev>span>a^^div#next -->

<div id="prev"></div>
<div id="next"></div>

<div id="prev"><span><a href=""></a></span></div>
<div id="next"></div>

<!-- 结束 -->
```
```bash
<!-- 9.重复生成多个 * -->
<!-- 开始 如： ul>li.list*5 -->

<ul>
	<li class="list"></li>
	<li class="list"></li>
	<li class="list"></li>
	<li class="list"></li>
	<li class="list"></li>
</ul>

<!-- 结束 -->
```
```bash
<!-- 10.生成分组 () -->
<!-- 开始 如： ul>(li.list>a)*5 -->

<ul>
	<li class="list"><a href=""></a></li>
	<li class="list"><a href=""></a></li>
	<li class="list"><a href=""></a></li>
	<li class="list"><a href=""></a></li>
	<li class="list"><a href=""></a></li>
</ul>

<!-- 比较一下  ul>li.list>a*5 -->

<ul>
	<li class="list">
		<a href=""></a>
		<a href=""></a>
		<a href=""></a>
		<a href=""></a>
		<a href=""></a>
	</li>
</ul>

<!-- 结束 -->
```
```bash
<!-- 11.生成自定义属性 [] -->
<!-- 开始 如： img[http://www.baidu.com][alt=baidu] -->

<img src="http://www.baidu.com" alt="baidu" />

<!-- 结束 -->

<!-- 12.生成递增 $ -->

<!-- 开始 如： ul>li.list$*5 -->

<ul>
	<li class="list1"></li>
	<li class="list2"></li>
	<li class="list3"></li>
	<li class="list4"></li>
	<li class="list5"></li>
</ul>

<!-- 结束 -->
```
```bash
<!-- 13.特定顺序递增 @N -->
<!-- 开始 如： ul>li.list$@100*5 -->

<ul>
	<li class="list100"></li>
	<li class="list101"></li>
	<li class="list102"></li>
	<li class="list103"></li>
	<li class="list104"></li>
</ul>

<!-- 结束 -->
```
```bash
<!-- 14.多位递增 $$$$ -->
<!-- 开始 如： ul>li.list$$$$*5 -->

<ul>
	<li class="list0001"></li>
	<li class="list0002"></li>
	<li class="list0003"></li>
	<li class="list0004"></li>
	<li class="list0005"></li>
</ul>

<!-- 结束 -->
```
```bash
<!-- 15.递减 @- -->
<!-- 开始 如： ul>li.list$@-*5 -->

<ul>
	<li class="list5"></li>
	<li class="list4"></li>
	<li class="list3"></li>
	<li class="list2"></li>
	<li class="list1"></li>
</ul>

<!-- 结束 -->
```
```bash
<!-- 15.特定递减 @-N -->
<!-- 开始 如： ul>li.list$@-100*5 -->

<ul>
	<li class="list104"></li>
	<li class="list103"></li>
	<li class="list102"></li>
	<li class="list101"></li>
	<li class="list100"></li>
</ul>

<!-- 结束 -->
```
```bash
<!-- 16.申城文本 {} -->
<!-- 开始 如： p{这里生成文本} -->

<p>这里生成文本</p>

<!-- 结束 -->

<!-- 16.缺省元素  -->

<!-- 开始 如： .header+.content -->

<div class="header"></div>
<div class="content"></div>

<!-- 结束 -->
```





