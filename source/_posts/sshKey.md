title: hexo之SSH kEY 二
date: 2015-01-29 11:18:00
categories:
tags: [ssh]
photos:
- http://bruce.u.qiniudn.com/2013/11/27/reading/photos-1.jpg
---
## GitHub
首先注册一个『GitHub』帐号，已有的默认默认请忽略
建立与你用户名对应的仓库，仓库名必须为『your_user_name.github.com』
添加SSH公钥到『Account settings -> SSH Keys -> Add SSH Key』
## 添加SSH-Key。

### 首先设置你的用户名密码：

``` bash
git config --global user.email "bu.ru@qq.com"
git config --global user.name "bruce-sha"
```

More info: [Writing](http://hexo.io/docs/writing.html)

### 生成密钥：

``` bash
ssh-keygen -t rsa -C "bu.ru@qq.com"
```

More info: [Server](http://hexo.io/docs/server.html)

### 输入文件路径：

``` bash
H:\hexo\blog>ssh-keygen -t rsa -C "bu.ru@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (//.ssh/id_rsa): H:\git\myssh\ssh
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in H:\git\myssh\ssh.
Your public key has been saved in H:\git\myssh\ssh.pub.
The key fingerprint is:
b0:0c:2e:67:33:ab:c1:50:10:40:0a:ba:c1:80:59:22 bu.ru@qq.com
```

More info: [Generating](http://hexo.io/docs/generating.html)

### 最后可以验证一下：

``` bash
ssh -T git@github.com
```

More info: [Deployment](http://hexo.io/docs/deployment.html)

