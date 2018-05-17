title: php 之 composer 的JSON包的规范一
date: 2016-07-17 19:01:42
categorise: php
tags: php
---

## JSON shema

可以看这里 [composer.json](https://github.com/composer/composer/blob/master/res/composer-schema.json)

## ROOT 包

“root 包”是指由 `composer.json` 定义的在你项目根目录的包。这是 `composer.json`定义你项目所需的主要条件。（简单的说，你自己的项目就是一个 root 包）

- [x]  注意： 一个资源包是不是“root 包”，取决于它的上下文。 例：如果你的项目依赖 monolog 库，那么你的项目就是“root 包”。 但是，如果你从 GitHub 上克隆了 monolog 为它修复 bug， 那么此时 monolog 就是“root 包”。

## 属性
------
### 包名 name
包的名称，它包括供应商名称和项目名称，使用 `/`分隔。
例：
- monolog/monolog
- prs/log

### 描述 description
对于包的描述信息

### 版本 version

非必需，建议忽略 规范 符合 `'x.y.z'` or `vx.y.z` 后缀可加 `-dev` `-alpha` `-patch` `-beta` `-RC` 并且可附带数字
例:
- 1.0.1
- 3.0.1-beta1
- 3.0.2-dev
- 2.0.1-alpha3
- [x]  注意： Packagist 使用 VCS 仓库， 因此 version 定义的版本号必须是真实准确的。 自己手动指定的 version，最终有可能在某个时候因为人为错误造成问题。

### 类型 type

composer 支持的类型如下：

- library: 这是默认类型，它会简单的将文件复制到 vendor 目录。
- project: 这表示当前包是一个项目，而不是一个库。例：框架应用程序 Symfony standard edition，内容管理系统 SilverStripe installer 或者完全成熟的分布式应用程序。使用 IDE 创建一个新的工作区时，这可以为其提供项目列表的初始化。
- metapackage: 当一个空的包，包含依赖并且需要触发依赖的安装，这将不会对系统写入额外的文件。因此这种安装类型并不需要一个 dist 或 source。
- composer-plugin: 一个安装类型为 composer-plugin 的包，它有一个自定义安装类型，可以为其它包提供一个 installler。

[自定义案例](https://github.com/5-say/composer-doc-cn/blob/master/cn-introduction/articles/custom-installers.md)

### 关键词 keywords

包含相关的关键词的数组。这些可用于搜索和过滤。
例:
- logging
- events
- database
- templating

### 项目主页 homepage
该项目的地址： 如 http://127.0.0.1/
### 版本发布时间 time
必须符合 `YYYY-MM-DD` 或 `YYYY-MM-DD HH:MM:SS` 格式
### 许可协议 license
包的许可协议，它可以是一个字符串或者字符串数组。
例:
- LGPL-2.1
- JSON
- Apache-2.0
- zlib
- WXwindos
- PHP-3.0.1
- MIT
参见 [许可协议](http://spdx.org/licenses/)
CODE：
```bash
{
    "license": "MIT"
}
{
    "license": [
       "PHP-3.0.1",
       "PHP-3.0.3"
    ]
}
{
    "license": "(PHP-3.0.1 or PHP-3.0.3)"
}
```
### 作者 author
包的作者，是一个数组
属性如下：
```bash
{
    "authors": [
        {
            "name": "yang l",
            "email": "youremail@gmail.com",
            "homepage": "http://www.yourdomain.com",
            "role": "Developer"
        },
        {
        	// 此处可增加额外的作者 格式同上
        }
    ]
}
```
### 支持  support
包的作者，是一个数组
属性如下：
- email: 项目支持 email 地址。
- issues: 跟踪问题的 URL 地址。
- forum: 论坛地址。
- wiki: Wiki 地址。
- irc: IRC 聊天频道地址，类似于 irc://server/channel。
- source: 网址浏览或下载源
```bash
{
    "support": {
        "email": "support@example.org",
        "irc": "irc://irc.freenode.org/composer"
    }
}
```
### Package links 包名到版本的映射

```bash
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```
例：如果你想允许依赖一个不稳定的包，你可以在一个包的版本约束后使用它，或者是一个空的版本约束内使用它。
```bash
{
    "require": {
        "monolog/monolog": "1.0.*@beta",
        "acme/foo": "@dev"
    }
}
```
如果你的依赖之一，有对另一个不稳定包的依赖，你最好在 require 中显示的定义它，并带上足够详细的稳定性标识。
```bash
{
    "require": {
        "doctrine/doctrine-fixtures-bundle": "dev-master",
        "doctrine/data-fixtures": "@dev"
    }
}
```
require 和 require-dev 还支持对 dev（开发）对dev 版本的明确引用 以确保它们被锁定到一个给定的状态，即使你运行了更新命令。你只需要明确一个开发版本号，并带上诸如 #<ref> 的标识
```bash
{
    "require": {
        "monolog/monolog": "dev-master#2eb0c0978d290a1c45346a1955188929cb4e5db7",
        "acme/foo": "1.0.x-dev#abc123"
    }
}
```


