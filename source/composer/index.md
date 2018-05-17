title: php 之 composer 的安装
date: 2016-07-12 20:11:29
tags: [php]
---
Composer 是 PHP 的一个依赖管理工具。它允许你申明项目所依赖的代码库，它会在你的项目中为你安装他们。

Composer 不是一个包管理器。是的，它涉及 "packages" 和 "libraries"，但它在每个项目的基础上进行管理，在你项目的某个目录中（例如 vendor）进行安装。默认情况下它不会在全局安装任何东西。因此，这仅仅是一个依赖管理。

Composer 将这样为你解决问题：

a) 你有一个项目依赖于若干个库。

b) 其中一些库依赖于其他库。

c) 你声明你所依赖的东西。

d) Composer 会找出哪个版本的包需要安装，并安装它们（将它们下载到你的项目中）。

##安装 - *nix

##局部安装  安装成功后使用命令  php composer.phar

```bash

curl -sS https://getcomposer.org/installer | php

curl -sS https://getcomposer.org/installer | php --install-dir=/home/composer  此参数可制订安装的位置

```

如果安装失败 请尝试
```bash
php -r "readfile('https://getcomposer.org/installer');" | php
```
##全局安装  安装成功后使用命令 composer  本人一般使用

```bash
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

##全局安装 (on OSX via homebrew)

```bash
brew update

brew tap josegonzalez/homebrew-php

brew tap homebrew/versions

brew install php55-intl

brew install josegonzalez/php/composer
```

##全局安装 windows

下载并且运行 Composer-Setup.exe，它将安装最新版本的 Composer ，并设置好系统的环境变量，因此你可以在任何目录下直接使用 composer 命令。

##手动安装 windows
```bash

C:\Users\Administrator>cd C:\bin

C:\bin>php -r "readfile('https://getcomposer.org/installer');" | php

C:\bin>echo @php "%~dp0composer.phar" %*>composer.bat

注意： 如果收到 readfile 错误提示，请使用 http 链接或者在 php.ini 中开启 php_openssl.dll 。
```
##测试 

重新打开CMD窗口

```bash
composer>composer -v
Composer version 1.2-dev (5a3d60c0cf0302a54b812f26fd23a0db67488e3e)

看到这就说明安装成功了
```
##简单使用 

```bash

e:\>mkdir TestComp

e:\>cd TestComp

e:\TestComp>touch composer.json

e:\TestComp>ls
composer.json

e:\TestComp>composer install
Loading composer repositories with package information
Updating dependencies (including require-dev)
  - Installing monolog/monolog (1.2.1)
    Downloading: Connecting...   Downloading: 100% 
monolog/monolog suggests installing ext-amqp (Allow sending log messages to an AMQP server (1.0+ required))
monolog/monolog suggests installing ext-mongo (Allow sending log messages to a MongoDB server)
monolog/monolog suggests installing mlehner/gelf-php (Allow sending log messages to a GrayLog2 server)
Writing lock file
Generating autoload files

e:\TestComp>ls
composer.json
composer.lock
vendor

e:\TestComp>

```

ok.结束





