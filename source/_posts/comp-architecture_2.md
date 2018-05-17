title: php 之 composer 的JSON包的规范二
date: 2016-07-17 19:01:42
categorise: php
tags: php
---
## composer.json 包详解二
composer 自动加载器 及 资源库 配置等
### require

必须的软件包列表，除非这些依赖被满足，否则不会完成安装。


### require-dev (root-only)

这个列表是为开发或测试等目的，额外列出的依赖。“root 包”的 require-dev 默认是会被安装的。然而 install 或 update 支持使用 --no-dev 参数来跳过 require-dev 字段中列出的包。

### conflict

此列表中的包与当前包的这个版本冲突。它们将不允许同时被安装。

### replace

这个列表中的包将被当前包取代。这使你可以 fork 一个包，以不同的名称和版本号发布，同时要求依赖于原包的其它包，在这之后依赖于你 fork 的这个包，因为它取代了原来的包。

### provide
由该包提供的其他包的列表。这主要是有用的常见的接口。一个包可以依靠一些虚拟日志包

### suggest
建议安装的包 给你的用户一个建议，他们可以添加更多的包。
```bash
{
    "suggest": {
        "monolog/monolog": "Allows more advanced logging of the application flow"
    }
}
```
### 自动加载 autoload
PHP autoloader 的自动加载映射。

### PSR-0
```bash
{
    "autoload": {
        "psr-0": {
            "Monolog\\": "src/",
            "Vendor\\Namespace\\": "src/",
            "Vendor_Namespace_": "src/"
        }
    }
}
```
### PSR-4
```bash
{
    "autoload": {
        "psr-4": {
            "Monolog\\": "src/",
            "Vendor\\Namespace\\": ""
        }
    }
}
```
### Classmap
```bash
{
    "autoload": {
        "classmap": ["src/", "lib/", "Something.php"]
    }
}
```
### 文件Files

如果你想要明确的指定，在每次请求时都要载入某些文件，那么你可以使用 'files' autoloading。通常作为函数库的载入方式（而非类库）。

实例：
```bash
{
    "autoload": {
        "files": ["src/MyLibrary/functions.php"]
    }
}
```

### 加载开发版本 只在root包下 autoload-dev (root-only)
```bash
{
    "autoload": {
        "psr-4": { "MyLibrary\\": "src/" }
    },
    "autoload-dev": {
        "psr-4": { "MyLibrary\\Tests\\": "tests/" }
    }
}
```

### include-path
```bash
{
    "include-path": ["lib/"]
}
```

###  精确自动加载组件的目标目录 target-dir

```bash
{
    "autoload": {
        "psr-0": { "Symfony\\Component\\Yaml\\": "" }
    },
    "target-dir": "Symfony/Component/Yaml"
}
```
### minimum-stability (root-only)  最低稳定版本

这定义了通过稳定性过滤包的默认行为。默认为 stable（稳定）。因此如果你依赖于一个 dev（开发）包，你应该明确的进行定义。

对每个包的所有版本都会进行稳定性检查，而低于 minimum-stability 所设定的最低稳定性的版本，将在解决依赖关系时被忽略。对于个别包的特殊稳定性要求，可以在 require 或 require-dev 中设定（请参考 Package links）。

可用的稳定性标识（按字母排序）：dev、alpha、beta、RC、stable。


### 使用更加稳定的版本prefer-stable (root-only)

当此选项被激活时，Composer 将优先使用更稳定的包版本。

使用 "prefer-stable": true 来激活它。


### repositories (root-only) 
使用自定义的包资源库 如果package list 没有的话 可以使用此项
只能在“Root包”的 composer.json 中定义。附属包中的 composer.json 将被忽略。
支持类型：
- composer: 一个 composer 类型的资源库，是一个简单的网络服务器（HTTP、FTP、SSH）上的 packages.json 文件，它包含一个 composer.json 对象的列表，有额外的 dist 和/或 source 信息。这个 packages.json 文件是用一个 PHP 流加载的。你可以使用 options 参数来设定额外的流信息。
- vcs: 从 git、svn 和 hg 取得资源。
- pear: 从 pear 获取资源。
- package: 如果你依赖于一个项目，它不提供任何对 composer 的支持，你就可以使用这种类型。你基本上就只需要内联一个 composer.json 对象。
例：
```bash
{
    "repositories": [
        {
            "type": "composer",
            "url": "http://packages.example.com"
        },
        {
            "type": "composer",
            "url": "https://packages.example.com",
            "options": {
                "ssl": {
                    "verify_peer": "true"
                }
            }
        },
        {
            "type": "vcs",
            "url": "https://github.com/Seldaek/monolog"
        },
        {
            "type": "pear",
            "url": "http://pear2.php.net"
        },
        {
            "type": "package",
            "package": {
                "name": "smarty/smarty",
                "version": "3.1.7",
                "dist": {
                    "url": "http://www.smarty.net/files/Smarty-3.1.7.zip",
                    "type": "zip"
                },
                "source": {
                    "url": "http://smarty-php.googlecode.com/svn/",
                    "type": "svn",
                    "reference": "tags/Smarty_3_1_7/distribution/"
                }
            }
        }
    ]
}
```
- [x] 注：顺序 自上而下 即下面的资源会覆盖上面的 如果有的话

### 配置项config (root-only)

支持以下选项：

- process-timeout: 默认为 300。处理进程结束时间，例如：git 克隆的时间。Composer 将放弃超时的任务。如果你的网络缓慢或者正在使用一个巨大的包，你可能要将这个值设置的更高一些。
- use-include-path: 默认为 false。如果为 true，Composer autoloader 还将在 PHP include path 中继续查找类文件。
- preferred-install: 默认为 auto。它的值可以是 source、dist 或 auto。这个选项允许你设置 Composer 的默认安装方法。
- github-protocols: 默认为 ["git", "https", "ssh"]。从 github.com 克隆时使用的协议优先级清单，因此默认情况下将优先使用 git 协议进行克隆。你可以重新排列它们的次序，例如，如果你的网络有代理服务器或 git 协议的效率很低，你就可以提升 https 协议的优先级。
- github-oauth: 一个域名和 oauth keys 的列表。 例如：使用 {"github.com": "oauthtoken"} 作为此选项的值， 将使用 oauthtoken 来访问 github 上的私人仓库，并绕过 low IP-based rate 的 API 限制。 关联知识 关于如何获取 GitHub 的 OAuth token。
- vendor-dir: 默认为 vendor。通过设置你可以安装依赖到不同的目录。
- bin-dir: 默认为 vendor/bin。如果一个项目包含二进制文件，它们将被连接到这个目录。
- cache-dir: unix 下默认为 $home/cache，Windows 下默认为 C:\Users\<user>\AppData\Local\Composer。用于存储 composer 所有的缓存文件。相关信息请查看 COMPOSER_HOME。
- cache-files-dir: 默认为 $cache-dir/files。存储包 zip 存档的目录。
- cache-repo-dir: 默认为 $cache-dir/repo。存储 composer 类型的 VCS（svn、github、bitbucket） repos 目录。
- cache-vcs-dir: 默认为 $cache-dir/vcs。此目录用于存储 VCS 克隆的 git/hg 类型的元数据，并加快安装速度。
- cache-files-ttl: 默认为 15552000（6个月）。默认情况下 Composer 缓存的所有数据都将在闲置6个月后被删除，这个选项允许你来调整这个时间，你可以将其设置为0以禁用缓存。
- cache-files-maxsize: 默认为 300MiB。Composer 缓存的最大容量，超出后将优先清除旧的缓存数据，直到缓存量低于这个数值。
- prepend-autoloader: 默认为 true。如果设置为 false，composer autoloader 将不会附加到现有的自动加载机制中。这有时候用来解决与其它自动加载机制产生的冲突。
- autoloader-suffix: 默认为 null。Composer autoloader 的后缀，当设置为空时将会产生一个随机的字符串。
optimize-autoloader Defaults to false. Always optimize when dumping the autoloader.
- github-domains: 默认为 ["github.com"]。一个 github mode 下的域名列表。这是用于GitHub的企业设置。
- notify-on-install: 默认为 true。Composer 允许资源仓库定义一个用于通知的 URL，以便有人从其上安装资源包时能够得到一个反馈通知。此选项允许你禁用该行为。
- discard-changes: 默认为 false，它的值可以是 true、false 或 stash。这个选项允许你设置在非交互模式下，当处理失败的更新时采用的处理方式。true 表示永远放弃更改。"stash" 表示继续尝试。Use this for CI servers or deploy scripts if you tend to have modified vendors.
例
```bash
{
    "config": {
        "bin-dir": "bin"
    }
}
```
### scripts (root-only)

Composer 允许你在安装过程中的各个阶段挂接脚本。

更多细节和案例请查看 [脚本](https://github.com/5-say/composer-doc-cn/blob/master/cn-introduction/articles/scripts.md)。


### extra

任意的，供 scripts 使用的额外数据。.

这可以是几乎任何东西。若要从脚本事件访问处理程序，你可以这样做：

$extra = $event->getComposer()->getPackage()->getExtra();
可选。


### bin

该属性用于标注一组应被视为二进制脚本的文件，他们会被软链接到（config 对象中的）bin-dir 属性所标注的目录，以供其他依赖包调用。

详细请查看 [Vendor Binaries](https://github.com/5-say/composer-doc-cn/blob/master/cn-introduction/articles/vendor-binaries.md)
###  rchive

这些选项在创建包存档时使用。

支持以下选项：

exclude: 允许设置一个需要被排除的路径的列表。使用与 .gitignore 文件相同的语法。一个前导的（!）将会使其变成白名单而无视之前相同目录的排除设定。前导斜杠只会在项目的相对路径的开头匹配。星号为通配符。
实例：

{
    "archive": {
        "exclude": ["/foo/bar", "baz", "/*.test", "!/foo/bar/baz"]
    }
}