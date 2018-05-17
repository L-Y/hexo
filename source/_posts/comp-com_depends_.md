title: php 之 composer 的命令二
date: 2016-07-13 20:10:55
categorise: php
tags: php
---

##依赖性检测 depends

说明：
depends 命令可以查出已安装在你项目中的某个包，是否正在被其它的包所依赖，并列出他们
```bash
composer depends  psr/log

参数 如下

depends [-r|--recursive] [-t|--tree] [--] <package> [<constraint>]
```

##有效性检测 validate

说明:
在提交 composer.json 文件，和创建 tag 前，你应该始终运行 validate 命令。它将检测你的 composer.json 文件是否是有效的
```bash
php composer.phar validate

参数：

--no-check-all: Composer 是否进行完整的校验。

```
##依赖包状态检测 status
说明：
如果你经常修改依赖包里的代码，并且它们是从 source（自定义源）进行安装的，那么 status 命令允许你进行检查，如果你有任何本地的更改它将会给予提示。
```bash
composer status
composer status -v
composer status -vv
composer status -vvv
```
##自我更新composer  self-update or selfupdate
说明：
将 Composer 自身升级到最新版本，只需要运行 self-update 命令。它将替换你的 composer.phar 文件到最新版本
```bash
composer self-update
<!-- 更新到特定的版本 -->

comoser self-update 1.0.0-alpha7

参数：
--rollback (-r): 回滚到你已经安装的最后一个版本。
--clean-backups: 在更新过程中删除旧的备份，这使得更新过后的当前版本是唯一可用的备份。
```
##更改配置 config
说明：
config 命令允许你编辑 Composer 的一些基本设置，无论是本地的 composer.json 或者全局的 config.json 文件。
```bash
composer config --list

composer config secure-http false

composer config github-domains [github.com,github.cn]

<!-- 修改包来源 -->
comoser config repositories.foo vcs http://github.com/foo/bar

参数：
--global (-g): 
<!-- 操作位于 $COMPOSER_HOME/config.json 的全局配置文件。如果不指定该参数，此命令将影响当前项目的 composer.json 文件，或 --file 参数所指向的文件。 -->
--editor (-e): 
<!-- 使用文本编辑器打开 composer.json 文件。默认情况下始终是打开当前项目的文件。当存在 --global 参数时，将会打开全局 composer.json 文件。 -->
--unset: 
<!-- 移除由 setting-key 指定名称的配置选项。 -->
--list (-l): 
<!-- 显示当前配置选项的列表。当存在 --global 参数时，将会显示全局配置选项的列表。 -->
--file="..." (-f): 
<!-- 在一个指定的文件上操作，而不是 composer.json。注意：不能与 --global 参数一起使用。 -->
```
##创建项目 create-project

说明：
你可以使用 Composer 从现有的包中创建一个新的项目。这相当于执行了一个 git clone 或 svn checkout 命令后将这个包的依赖安装到它自己的 vendor 目录。

此命令有几个常见的用途：

你可以快速的部署你的应用。
你可以检出任何资源包，并开发它的补丁。
多人开发项目，可以用它来加快应用的初始化。
要创建基于 Composer 的新项目，你可以使用 "create-project" 命令。传递一个包名，它会为你创建项目的目录。你也可以在第三个参数中指定版本号，否则将获取最新的版本。

如果该目录目前不存在，则会在安装过程中自动创建。

```bash
composer create-project lavarel/laravel path 2.2.*
composer create-project --prefer-dist laravel/laravel blog

参数：
--repository-url: 提供一个自定义的储存库来搜索包，这将被用来代替 packagist.org。可以是一个指向 composer 资源库的 HTTP URL，或者是指向某个 packages.json 文件的本地路径。
--stability (-s): 资源包的最低稳定版本，默认为 stable。
--prefer-source: 当有可用的包时，从 source 安装。
--prefer-dist: 当有可用的包时，从 dist 安装。
--dev: 安装 require-dev 字段中列出的包。
--no-install: 禁止安装包的依赖。
--no-plugins: 禁用 plugins。
--no-scripts: 禁止在根资源包中定义的脚本执行。
--no-progress: 移除进度信息，这可以避免一些不处理换行的终端或脚本出现混乱的显示。
--keep-vcs: 创建时跳过缺失的 VCS 。如果你在非交互模式下运行创建命令，这将是非常有用的。
```

##打印自动加载 dump-autoload
说明：
某些情况下你需要更新 autoloader，例如在你的包中加入了一个新的类。你可以使用 dump-autoload 来完成，而不必执行 install 或 update 命令。
此外，它可以打印一个优化过的，符合 PSR-0/4 规范的类的索引，这也是出于对性能的可考虑。在大型的应用中会有许多类文件，而 autoloader 会占用每个请求的很大一部分时间，使用 classmaps 或许在开发时不太方便，但它在保证性能的前提下，仍然可以获得 PSR-0/4 规范带来的便利。

```bash
composer dump-autoload
参数：
--optimize (-o): 转换 PSR-0/4 autoloading 到 classmap 获得更快的载入速度。这特别适用于生产环境，但可能需要一些时间来运行，因此它目前不是默认设置。
--no-dev: 禁用 autoload-dev 规则。
```
#查看许可协议 licenses
```bash
composer licenses
composer licenses --format=json
composer license --format=json --no-dev
```
#执行脚本 run-script
```bash
composer run-script  testscript
<!-- 说明 关于脚本请看后面的文章 -->

```
#诊断 diagnose
说明：
如果你觉得发现了一个 bug 或是程序行为变得怪异，你可能需要运行 diagnose 命令，来帮助你检测一些常见的问题
```bash
composer diagnose
```

#归档 archive
说明：
此命令用来对指定包的指定版本进行 zip/tar 归档。它也可以用来归档你的整个项目，不包括 excluded/ignored（排除/忽略）的文件。
```bash
composer archive vendor/package 2.0.21 --format=zip
参数：
--format (-f): 指定归档格式：tar 或 zip（默认为 tar）。
--dir: 指定归档存放的目录（默认为当前目录）。

```






