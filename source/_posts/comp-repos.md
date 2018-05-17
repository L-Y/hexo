title: php 之 composer 库/资源包
date: 2016-07-13 19:42:01
categorise: php
tags: php
---

##创建一个包

```bash
{
	<!-- 声明 供应商名/及项目名称   -->
	"name": "google/testName", 
	<!-- 添加一个 require 到项目中 相当与创建一个依赖于其他库的包 -->
	"require": {
		"mogolog/mogolog": "2.0.*",
		"symfony/http-kernel": 2.8.*|3.0.*,
		"phpunit/phpunit": ~4.1
		"php": >=5.5.9
		"ext-mbstring": *
		"ext-openssl": *
	},
	<!-- 声明这个包的地址 也就是google/testName所发布的地址 -->
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/google/testname"
        }
    ],
	<!-- 声明版本号 -->
	"version": "1.0.0"
}
```

##标签的解释

对于每一个看起来像版本号的标签，都会相应的创建一个包的版本。它应该符合 'X.Y.Z' 或者 'vX.Y.Z' 的形式，-patch、-alpha、-beta 或 -RC 这些后缀是可选的。在后缀之后也可以再跟上一个数字。

下面是有效的标签名称的几个例子：
```bash
1.0.0
v1.0.0
1.10.5-RC1
v4.4.4beta2
v2.0.0-alpha
v2.0.4-p1
```
注意： 即使你的标签带有前缀 v， 由于在需要 require 一个版本的约束时是不允许这种前缀的， 因此 v 将被省略（例如标签 V1.0.0 将创建 1.0.0 版本）

##分支

对于每一个分支，都会相应的创建一个包的开发版本。如果分支名看起来像一个版本号，那么将创建一个如同 {分支名}-dev 的包版本号。例如一个分支 2.0 将产生一个 2.0.x-dev 包版本（加入了 .x 是出于技术的原因，以确保它被识别为一个分支，而 2.0.x 的分支名称也是允许的，它同样会被转换为 2.0.x-dev）。如果分支名看起来不像一个版本号，它将会创建 dev-{分支名} 形式的版本号。例如 master 将产生一个 dev-master 的版本号。

下面是版本分支名称的一些示例：

1.x
1.0 (equals 1.0.x)
1.1.x

##锁文件

如果你愿意，可以在你的项目中提交 composer.lock 文件。他将帮助你的团队始终针对同一个依赖版本进行测试。任何时候，这个锁文件都只对于你的项目产生影响。

如果你不想提交锁文件，并且你正在使用 Git，那么请将它添加到 .gitignore 文件中。


##发布到 VCS（线上版本控制系统）

一旦你有一个包含 composer.json 文件的库存储在线上版本控制系统（例如：Git），你的库就可以被 Composer 所安装。在这个例子中，我们将 acme/hello-world 库发布在 GitHub 上的 github.com/username/hello-world 中。

现在测试这个 agoogle/testName 包，我们在本地创建一个新的项目。我们将它命名为 acme/blog。此博客将依赖 google/testName，而后者又依赖 monolog/monolog。我们可以在某处创建一个新的 blog 文件夹来完成它，并且需要包含 composer.json 文件：
```bash
{
    "name": "myapp/blog",
    "require": {
        "google/testName": "dev-master"
    }
}
```
在这个例子中 name 不是必须的，因为我们并不想将它发布为一个库。在这里为 composer.json 文件添加描述。

现在我们需要告诉我们的应用，在哪里可以找到 hello-world 的依赖。为此我们需要在 composer.json 中添加 repositories 来源申明：
```bash
{
    "name": "acme/blog",
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/google/testName"
        }
    ],
    "require": {
        "google/testName": "dev-master"
    }
}
```
更多关于包的来源是如何工作的，以及还有什么其他的类型可供选择，请查看资源库。

这就是全部了。你现在可以使用 Composer 的 install 命令来安装你的依赖包了！

小结： 任何含有 composer.json 的 GIT、SVN、HG 存储库，都可以通过 require 字段指定“包来源”和“声明依赖”来添加到你的项目中。


##发布到 packagist

好的，你现在可以发布你的包了，但你不会希望你的用户每次都这样繁琐的指定包的来源。

你可能注意到了另一件事，我们并没有指定 monolog/monolog 的来源。它是怎么工作的？答案是 packagist。

Packagist 是 Composer 主要的一个包信息存储库，它默认是启用的。任何在 packagist 上发布的包都可以直接被 Composer 使用。就像 monolog 它被 发布在 packagist 上，我们可以直接使用它，而不必指定任何额外的来源信息。

如果我们想与世界分享我们的 testName，我们最好将它发布到 packagist 上。这样做是很容易的。

你只需要点击那个大大的 "Submit Package" 按钮并注册。接着提交你库的来源地址，此时 packagist 就开始了抓取。一旦完成，你的包将可以提供给任何人使用。

ok.完