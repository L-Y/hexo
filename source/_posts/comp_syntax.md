title: php 之 composer 的用法
date: 2016-07-12 20:11:29
categories: php
tags: [php]
---
##composer.json：项目安装

要开始在你的项目中使用 Composer，你只需要一个 composer.json 文件。该文件包含了项目的依赖和其它的一些元数据。

##关于 require Key

组成部分：

1.包名称： monolog/monolog 
2.包版本：1.0.*
```bash
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

##包名称

组成部分：

1.供应商名称: monolog
2.项目名称： monolog

如：meizu/mei 2  google/android

##包版本

确切的版本号：1.0.2 ---你可以指定包的确切版本。

范围：>=1.0 >=1.0,<2.0 >=1.0,<1.1|>=1.2 ----有效的运算符：>、>=、<、<=、!=。 
你可以定义多个范围，用逗号隔开，这将被视为一个逻辑AND处理。一个管道符号|将作为逻辑OR处理。 
AND 的优先级高于 OR。	

通配符：1.0.*---你可以使用通配符*来指定一种模式。1.0.*与>=1.0,<1.1是等效的。

赋值运算符：~1.2--- ~1.2相当于>=1.2,<2.0

## ~波浪符运算符 

~ 最好用例子来解释： ~1.2 相当于 >=1.2,<2.0，而 ~1.2.3 相当于 >=1.2.3,<1.3。正如你所看到的这对于遵循 语义化版本号 的项目最有用。一个常见的用法是标记你所依赖的最低版本，像 ~1.2 （允许1.2以上的任何版本，但不包括2.0）。由于理论上直到2.0应该都没有向后兼容性问题，所以效果很好。你还会看到它的另一种用法，使用 ~ 指定最低版本，但允许版本号的最后一位数字上升。

注意： 虽然 2.0-beta.1 严格地说是早于 2.0，但是，根据版本约束条件， 例如 ~1.2 却不会安装这个版本。就像前面所讲的 ~1.2 只意味着 .2 部分可以改变，但是 1. 部分是固定的。


##  composer.lock - 锁文件 使用install 会自动生成

在安装依赖后，Composer 将把安装时确切的版本号列表写入 composer.lock 文件。这将锁定改项目的特定版本。

请提交你应用程序的 composer.lock （包括 composer.json）到你的版本库中

这是非常重要的，因为 install 命令将会检查锁文件是否存在，如果存在，它将下载指定的版本（忽略 composer.json 文件中的定义）。

这意味着，任何人建立项目都将下载与指定版本完全相同的依赖。你的持续集成服务器、生产环境、你团队中的其他开发人员、每件事、每个人都使用相同的依赖，从而减轻潜在的错误对部署的影响。即使你独自开发项目，在六个月内重新安装项目时，你也可以放心的继续工作，即使从那时起你的依赖已经发布了许多新的版本。

如果不存在 composer.lock 文件，Composer 将读取 composer.json 并创建锁文件。

这意味着如果你的依赖更新了新的版本，你将不会获得任何更新。此时要更新你的依赖版本请使用 update 命令。这将获取最新匹配的版本（根据你的 composer.json 文件）并将新版本更新进锁文件。

如果只想安装或更新一个依赖，你可以白名单它们
```bash
composer update monolog/monolog [...]
```

##Packagist

packagist 是 Composer 的主要资源库。 一个 Composer 的库基本上是一个包的源：记录了可以得到包的地方。Packagist 的目标是成为大家使用库资源的中央存储平台。这意味着你可以 require 那里的任何包。

当你访问 packagist 网站 (packagist.org)，你可以浏览和搜索资源包。

任何支持 Composer 的开源项目应该发布自己的包在 packagist 上。虽然并不一定要发布在 packagist 上来使用 Composer，但它使我们的编程生活更加轻松。


##自动加载

对于库的自动加载信息，Composer 生成了一个 vendor/autoload.php 文件。你可以简单的引入这个文件，你会得到一个免费的自动加载支持。

require 'vendor/autoload.php';
这使得你可以很容易的使用第三方代码。例如：如果你的项目依赖 monolog，你就可以像这样开始使用这个类库，并且他们将被自动加载。
```bash
$log = new Monolog\Logger('name');
$log->pushHandler(new Monolog\Handler\StreamHandler('app.log', Monolog\Logger::WARNING));

$log->addWarning('Foo');
```
你可以在 composer.json 的 autoload 字段中增加自己的 autoloader。
```bash
{
    "autoload": {
        "psr-4": {"Acme\\": "src/"}
    }
}
```
Composer 将注册一个 PSR-4 autoloader 到 Acme 命名空间。

你可以定义一个从命名空间到目录的映射。此时 src 会在你项目的根目录，与 vendor 文件夹同级。例如 src/Foo.php 文件应该包含 Acme\Foo 类。

添加 autoload 字段后，你应该再次运行 install 命令来生成 vendor/autoload.php 文件。

引用这个文件也将返回 autoloader 的实例，你可以将包含调用的返回值存储在变量中，并添加更多的命名空间。这对于在一个测试套件中自动加载类文件是非常有用的，例如。
```bash
$loader = require 'vendor/autoload.php';
$loader->add('Acme\\Test\\', __DIR__);
```
除了 PSR-4 自动加载，classmap 也是支持的。这允许类被自动加载，即使不符合 PSR-0 规范。详细请查看 自动加载-参考。

注意： Composer 提供了自己的 autoloader。如果你不想使用它，你可以仅仅引入 vendor/composer/autoload_*.php 文件，它返回一个关联数组，你可以通过这个关联数组配置自己的 autoloader。