title: php 之 composer 的命令三
date: 2016-07-13 20:10:55
categorise: php
tags: php
---

##环境变量
说明：
你可以设置一些环境变量来覆盖默认的配置。建议尽可能的在 composer.json 的 config 字段中设置这些值，而不是通过命令行设置环境变量。值得注意的是环境变量中的值，将始终优先于 composer.json 中所指定的值。
```bash
1.COMPOSER

<!-- 环境变量 COMPOSER 可以为 composer.json 文件指定其它的文件名。如 -->

COMPOSER=composer-other.json composer install

2.COMPOSER_ROOT_VERSION

<!-- 通过设置这个环境变量，你可以指定 root 包的版本，如果程序不能从 VCS 上猜测出版本号，并且未在 composer.json 文件中申明。 -->


3.COMPOSER_VENDOR_DIR

<!-- 通过设置这个环境变量，你可以指定 composer 将依赖安装在 vendor 以外的其它目录中。 -->

4.COMPOSER_BIN_DIR

<!-- 通过设置这个环境变量，你可以指定 bin（Vendor Binaries）目录到 vendor/bin 以外的其它目录。 -->

5.http_proxy or HTTP_PROXY

<!-- 如果你是通过 HTTP 代理来使用 Composer，你可以使用 http_proxy 或 HTTP_PROXY 环境变量。只要简单的将它设置为代理服务器的 URL。许多操作系统已经为你的服务设置了此变量。

建议使用 http_proxy（小写）或者两者都进行定义。因为某些工具，像 git 或 curl 将使用 http_proxy 小写的版本。另外，你还可以使用 git config --global http.proxy <proxy url> 来单独设置 git 的代理。 -->


6.no_proxy

<!-- 如果你是使用代理服务器，并且想要对某些域名禁用代理，就可以使用 no_proxy 环境变量。只需要输入一个逗号相隔的域名 排除 列表。

此环境变量接受域名、IP 以及 CIDR地址块。你可以将它限制到一个端口（例如：:80）。你还可以把它设置为 * 来忽略所有的 HTTP 代理请求。 -->


7.HTTP_PROXY_REQUEST_FULLURI

<!-- 如果你使用了 HTTP 代理，但它不支持 request_fulluri 标签，那么你应该设置这个环境变量为 false 或 0 ，来防止 composer 从 request_fulluri 读取配置。 -->


8.HTTPS_PROXY_REQUEST_FULLURI

<!-- 如果你使用了 HTTPS 代理，但它不支持 request_fulluri 标签，那么你应该设置这个环境变量为 false 或 0 ，来防止 composer 从 request_fulluri 读取配置。 -->


9.COMPOSER_HOME

<!-- COMPOSER_HOME 环境变量允许你改变 Composer 的主目录。这是一个隐藏的、所有项目共享的全局目录（对本机的所有用户都可用）。

它在各个系统上的默认值分别为：

*nix /home/<user>/.composer。
OSX /Users/<user>/.composer。
Windows C:\Users\<user>\AppData\Roaming\Composer。 -->

10.COMPOSER_HOME/config.json

<!-- 你可以在 COMPOSER_HOME 目录中放置一个 config.json 文件。在你执行 install 和 update 命令时，Composer 会将它与你项目中的 composer.json 文件进行合并。

该文件允许你为用户的项目设置 配置信息 和 资源库。

若 全局 和 项目 存在相同配置项，那么项目中的 composer.json 文件拥有更高的优先级。
 -->

11.COMPOSER_CACHE_DIR

<!-- COMPOSER_CACHE_DIR 环境变量允许你设置 Composer 的缓存目录，这也可以通过 cache-dir 进行配置。

它在各个系统上的默认值分别为：

*nix and OSX $COMPOSER_HOME/cache。
Windows C:\Users\<user>\AppData\Local\Composer 或 %LOCALAPPDATA%/Composer。

COMPOSER_PROCESS_TIMEOUT

这个环境变量控制着 Composer 执行命令的等待时间（例如：git 命令）。默认值为300秒（5分钟）。 -->


12.COMPOSER_DISCARD_CHANGES

<!-- 这个环境变量控制着 discard-changes config option。 -->


13.COMPOSER_NO_INTERACTION

<!-- 如果设置为1，这个环境变量将使 Composer 在执行每一个命令时都放弃交互，相当于对所有命令都使用了 --no-interaction。可以在搭建 虚拟机/持续集成服务器 时这样设置。 -->
```

