title: 安装hexo时提示错误，使用npm安装速度慢
date: 2017-11-09 21:43:27
tags: hexo
---
## 错误描述

英文：
```bash
npm WARN deprecated swig@1.4.2:This package is no longer maintained
```
翻译：npm WARN弃用swig@1.4.2：这个包不再维护
## 解决方案
使用国内安装源，这里使用淘宝的,命令如下：
```bash 
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
使用国内的安装源后命令变为cnpm,如下：
### 原来
```bash
$ npm install hexo-cli -g
$ npm install hexo-deployer-git 
$ hexo init
$ npm install
$ hexo g
$ hexo s
```
### 现在
```bash
$ cnpm install hexo-cli -g 
$ cnpm install hexo-deployer-git 
$ hexo init
$ cnpm install
$ hexo g
$ hexo s
```
### 问题描述
如果我不想使用国内的镜像怎么办呢，那也很简单，命令如下
```bash
$ cnpm onfig edit
```
找到```bash registry=https://registry.npm.taobao.org```将其注释即可