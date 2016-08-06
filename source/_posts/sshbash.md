title: 多个github账号发布hexo
date: 2015-06-25 23:45:41
tags: hexo
---
##用多个Github账号发布Hexo博客
Posted on 2015-01-10   |  
如果只有一个 Github 账号，用 Hexo 发布博客很简单，在_config.yml里进行如下配置就好：

我的 _config.yml
```bash
deploy:
  type: github
  repo: https://github.com/RockerFlower/rockerflower.github.com.git
```
但如果是同一台设备承载了两个或以上用户的 Git 需求，比如我女友也要用 Hexo 写博客，就需要配置一下 SSH 了。操作系统为 OS X，Windows 同理。

新建 User 2 的 SSH Key

# 新建 SSH key：
```bash
$ cd ~/.ssh
$ ssh-keygen -t rsa -C "example2@email.com"
```
# 设置 SSH Key 的名字为 id_rsa_seeyuen
```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/RockerFlower/.ssh/id_rsa): 
$ id_rsa_seeyuen
```
##把新密钥添加到 SSH Agent 中
为了让 SSH 识别新的私钥，需将其添加到 SSH Agent 中：

```bash
$ ssh-add ~/.ssh/id_rsa_seeyuen
```
如果出现Could not open a connection to your authentication agent的错误，就尝试用以下命令：

```bash
$ ssh-agent bash
$ ssh-add ~/.ssh/id_rsa_seeyuen
```
##把新生成的 id_rsa_seeyuen.pub 内容添加到 GitHub 后台
修改 config 文件
在 ~/.ssh 下找到 config 文件，如果没有就创建：

```bash
$ touch config        # 创建config
```
然后写入：

```bash
# Default Github User (example1@mail.com)
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa
# Second Github User (example2@mail.com)
# 建一个 Github 别名，新建的帐号使用这个别名做克隆和更新
Host git2
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_seeyuen
```
其中的规则是：从上至下读取 config 的内容，在每个 Host 下寻找对应的私钥。这里将 GitHub SSH 仓库地址中的 git@github.com 替换成新建的 Host 别名如：git2。
这个例子中，需要 git 的是各自的 Hexo 仓库，那么 See Yuen 博客仓库的原地址是：git@github.com:SeeYuen/seeyuen.github.com.git，替换后就是：git2:SeeYuen/seeyuen.github.com.git。

修改 Hexo 的 _config.yml
我的 _config.yml
```bash
deploy:
  type: github
  repo: https://github.com/RockerFlower/rockerflower.github.com.git
See Yuen 的 _config.yml
```
```bash
deploy:
  type: github
  repo: git2:SeeYuen/seeyuen.github.com.git
  ```
完成。这样在各自的目录下可以互不影响地 deploy 到各自的 Github 仓库了。
