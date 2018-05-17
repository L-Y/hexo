title: 使用git时出现的错误整理
date: 2017-11-09 22:29:03
categories: git
tags:
---
##问题描述
```bash
warning: LF will be replaced by CRLF in tags/sublime/index.html.
```
## 原因分析
回车(CR, ASCII 13, \r) 换行(LF, ASCII 10, \n)。
这两个ACSII字符不会在屏幕有任何输出，但在Windows中广泛使用【CRLF】来标识一行的结束。而在Linux/UNIX系统中只有换行符。
也就是说在windows中的换行符为 CRLF， 而在linux下的换行符为：LF
因为git是基于unix的，默认为LF，而现在使用的是windows，所以会提示 LF呗替换为CRLF
## 解决方案
删除刚刚生成的.git文件，代码
```bash
$ rm -rf .git  
$ git config --gobal core.autocrlf false  
```

