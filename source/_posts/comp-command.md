title: php 之 composer 的命令一
date: 2016-07-13 20:10:55
categorise: php
tags: php
---

##前言 帮助无疑是最好的教程

```bash
<!-- 此命令将列出所有可操作的命令 -->

composer list

<!-- 如想查看list用法 可使用 -->

composer list -h  or composer list --help

<!-- 又如 -->
composer init -h

comoser search -h

```

#全局参数 可与任意命令使用
```bash
--verbose (-v): 增加反馈信息的详细度。
	<!-- 1.)-v 表示正常输出。 -->
	composer -v
	<!-- 2.)-vv 表示更详细的输出。 -->
	comoser -vv
	<!-- 3.)-vvv 则是为了 debug。 -->
	composer -vvv
<!-- --help (-h): 显示帮助信息。 -->

composer -h

--quiet (-q): 禁止输出任何信息。

--no-interaction (-n): 不要询问任何交互问题。

--working-dir (-d): 

如果指定的话，使用给定的目录作为工作目录。

<!-- --profile: 显示时间和内存使用信息。 -->

composer --profile

--ansi: 强制 ANSI 输出。

--no-ansi: 关闭 ANSI 输出。

--version (-V): 显示当前应用程序的版本信息。
```
##初始化 init

```bash
composer init
package: 
{
    "name": "test/testname",
    "description": "This is a test!",
    "type": "test/test",
    "license": "test",
    "minimum-stability": "beta",
    "require": {}
}
<!-- 参数 -->
--name: 包的名称。
--description: 包的描述。
--author: 包的作者。
--homepage: 包的主页。
--require: 需要依赖的其它包，必须要有一个版本约束。并且应该遵循 foo/bar:1.0.0 这样的格式。
--require-dev: 开发版的依赖包，内容格式与 --require 相同。
--stability (-s): minimum-stability 字段的值。
<!-- 完成的命令如下 -->

composer init --name test/testname --description "This is a test" --type "test/test" --license "test" --stability "dev" --require "" --author ly 
```

##安装 install

说明：
install 命令从当前目录读取 composer.json 文件，处理了依赖关系，并把其安装到 vendor 目录下
```bash

composer install
<!-- 参数 -->
--prefer-source: 下载包的方式有两种： source 和 dist。对于稳定版本 composer 将默认使用 dist 方式。而 source 表示版本控制源 。如果 --prefer-source 是被启用的，composer 将从 source 安装（如果有的话）。如果想要使用一个 bugfix 到你的项目，这是非常有用的。并且可以直接从本地的版本库直接获取依赖关系。
--prefer-dist: 与 --prefer-source 相反，composer 将尽可能的从 dist 获取，这将大幅度的加快在 build servers 上的安装。这也是一个回避 git 问题的途径，如果你不清楚如何正确的设置。
--dry-run: 如果你只是想演示而并非实际安装一个包，你可以运行 --dry-run 命令，它将模拟安装并显示将会发生什么。
--dev: 安装 require-dev 字段中列出的包（这是一个默认值）。
--no-dev:跳过 require-dev 字段中列出的包。
--no-scripts:跳过 composer.json 文件中定义的脚本。
--no-plugins: 关闭 plugins。
--no-progress: 移除进度信息，这可以避免一些不处理换行的终端或脚本出现混乱的显示。
--optimize-autoloader (-o): 转换 PSR-0/4 autoloading 到 classmap 可以获得更快的加载支持。特别是在生产环境下建议这么做，但由于运行需要一些时间，因此并没有作为默认值。

Tips：
如果当前目录下存在 composer.lock 文件，它会从此文件读取依赖版本，而不是根据 composer.json 文件去获取依赖。这确保了该库的每个使用者都能得到相同的依赖版本。

如果没有 composer.lock 文件，composer 将在处理完依赖关系后创建它。
```

## update
```bash
为了获取依赖的最新版本，并且升级 composer.lock 文件，你应该使用 update 命令。

composer update
<!-- 这将解决项目的所有依赖，并将确切的版本号写入 composer.lock。 -->

<!-- 如果你只是想更新几个包，你可以像这样分别列出它们： -->

php composer.phar update vendor/package vendor/package2
<!-- 你还可以使用通配符进行批量更新： -->

php composer.phar update vendor/*
<!--  -->

update-参数

与安装的 参数相同 另外如下：
```bash
 
--with-dependencies 

<!-- 同时更新白名单内包的依赖关系，这将进行递归更新。 -->
```bash

##申明依赖 require

说明：
require 命令增加新的依赖包到当前目录的 composer.json 文件中
```bash
composer require
<!-- 如果你不希望通过交互来指定依赖包，你可以在这条令中直接指明依赖包。 -->

composer require vendor/package:2.* vendor/package2:dev-master

参数 ：
--prefer-source: 当有可用的包时，从 source 安装。
--prefer-dist: 当有可用的包时，从 dist 安装。
--dev: 安装 require-dev 字段中列出的包。
--no-update: 禁用依赖关系的自动更新。
--no-progress: 移除进度信息，这可以避免一些不处理换行的终端或脚本出现混乱的显示。
--update-with-dependencies 一并更新新装包的依赖。

```

##全局执行 global
```bash
composer global require fabpot/php-cs-fixer:dev-master
<!-- 如要更新 使用-->

composer global update
```

##搜索命令 search
说明：
search 命令允许你为当前项目搜索依赖包，通常它只搜索 packagist.org 上的包，你可以简单的输入你的搜索条件。
```bash
composer search lavarel

参数：
--only-name (-N): 仅针对指定的名称搜索（完全匹配）。

```

##展示命令 show

说明：
列出所有可用的软件包
```bash
composer show 
composer show monolog/monolog
php composer.phar show monolog/monolog 1.2.1
参数：
--installed (-i): 列出已安装的依赖包。

--platform (-p): 仅列出平台软件包（PHP 与它的扩展）。

--self (-s): 仅列出当前项目信息。
```