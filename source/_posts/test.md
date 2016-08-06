title: git命令 及遇到的问题
date: 2015-06-25 13:43:56
tags: git
---
CRLF -- Carriage-Return Line-Feed 回车换行

就是回车(CR, ASCII 13, \r) 换行(LF, ASCII 10, \n)。
这两个ACSII字符不会在屏幕有任何输出，但在Windows中广泛使用来标识一行的结束。而在Linux/UNIX系统中只有换行符。
也就是说在windows中的换行符为 CRLF， 而在linux下的换行符为：LF
使用git来生成一个rails工程后，文件中的换行符为LF， 当执行git add .时，系统提示：LF 将被转换成 CRLF
 
解决方法：
 
删除刚刚生成的.git文件
```bash 
rm -rf .git  
git config --gobal core.autocrlf false  
```
这样系统就不会去进行换行符的转换了
最后重新执行

1.    Git 更新代码
``` bash
git clone git://github.com/yourproject/projectname.git
```
2.    Git 更新分支 查看服务器上的所有分支
```bash
git branch –r
```

查看当前有效分支：
```bash
git branch
```
3.    git 错误处理

3.1.    fatal: Not a git repository (or any of the parent directories): .git 是由于 git 没有进入到目录中

3.2.    The file will have its original line endings in your workingdirectory.由于文件夹远程不存在，但是不影响提交

4.    打标签 (tag)
4.1.    查看当前标签状态

git tag

4.2.    创建标签
```bash
git tag -a ice-1.0 0d0fc38441  -m "ice hhvm 1.0"
```
解释：
```bash
-a ice  ： -a 是创建  ice 是名称

0d0fc38441  这个是你 commit 的 ID
```
可以通过 git log 获取：

也可以通过网页中的 commit  获取

如果不加 commit 的标示，那么就是最后的，如果加了，可以在中间打上版本的标签，这个很好
```bash
-m “ ice hhvm 1.0 ”: 是描述
```
4.3.    分享提交标签到远程服务器上
```bash
git push origin master

git push origin --tags
```
注：

–tags  前面是 2 个横杠 (-)

如果这里错误了会如下错误：
```bash
error: src refspec –tags does not match any.

error: failed to push some refs to'https://github.com/huzhiguang/hiphop_extension.git'
```
正确后会提示：
```bash
Username for 'https://github.com': XXXXXXXX

Password for 'https://XXXXXXXXXX@github.com':

Counting objects: 1, done.

Writing objects: 100% (1/1), 166 bytes| 0 bytes/s, done.

Total 1 (delta 0), reused 0 (delta 0)

To https://github.com/huzhiguang/hiphop_extension.git

 * [new tag]          ice-1.0  ->  ice-1.0

然后你去你的 git 上查看就会有新的了的 tag 版本了

注：

--tags   参数表示提交所有   tag   至服务器端，普通的   git push origin master   操作不会推送标签到服务器端。
```
4.4.   删除本地标签命令
```bash
git tag -d  ice-1.0
```
执行后运行 git tag 可以查看是否已经删除了

4.5.   删除远程标签命令
```bash
git push origin :refs/tags/  ice-1.0
```

 

5.    git 操作

5.1.    创建 git 仓库

创建新文件夹，打开，然后执行
```bash
git init
```
以创建新的  git  仓库。

5.2.    检出仓库

执行如下命令以创建一个本地仓库的克隆版本：
```bash
git clone /path/to/repository
```
如果是远端服务器上的仓库，你的命令会是这个样子：
```bash
git clone username@host:/path/to/repository
```
例子：
```bash
git clone  https://github.com/huzhiguang/hiphop_extension.git
```
这里需要用 https 的，否则你提交等操作无法进行

5.3.    工作原理

你的本地仓库由  git  维护的三棵“树”组成。第一个是你的 工作目录，它持有实际文件；第二个是 缓存区（ Index ），它像个缓存区域，临时保存你的改动；最后是  HEAD ，指向你最近一次提交后的结果。

5.4.    添加、提交与推送

5.4.1.    添加到缓存区

你可以计划改动（把它们添加到缓存区），使用如下命令：
```bash
git add <filename>

git add *
```
这是  git  基本工作流程的第一步；使用如下命令以实际提交改动：
```bash
git commit -m " 代码提交信息 "
```
现在，你的改动已经提交到了  HEAD ，但是还没到你的远端仓库。

例子：
```bash
git add a.php

git commit –m “test a.php”
```
这时只是把代码提交到了本地缓存

5.4.2.    推送改动

你的改动现在已经在本地仓库的  HEAD  中了。执行如下命令以将这些改动提交到远端仓库：
```bash
git push origin master
```
可以把  master  换成你想要推送的任何分支。

如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：
```bash
git remote add origin <server>
```
如此你就能够将你的改动推送到所添加的服务器上去了。

在你当前的分支下：

git branch  查看，可以通过 gitcheckout ice 切换（如果你有多个的话，默认是 master ）

例子：
```bash
git push origin master
```
推送到 master 上
```bash
git push origin ice 推送到 ice  分支上
```
这时你去你的 git 上查看，就可以看到已经上传到远程服务器的内容了

5.5.    分支

创建一个叫做“ feature_x ”的分支，并切换过去：
```bash
git checkout -b feature_x
```
切换回主分支：
```bash
git checkout master
```
再把新建的分支删掉：
```bash
git branch -d feature_x
```
除非你将分支推送到远端仓库，不然该分支就是 不为他人所见的：
```bash
git push origin <branch>
```
例：
```bash
git checkout –b ice ( 没有的时候可以用  ­–b 创建 )
```
如果存在 branch ，那么直接切换即可，如：
```bash
git checkout master
```
推送到远程仓库
```bash
git push origin master
```
5.6.    更新与合并

要更新你的本地仓库至最新改动，执行：
```bash
git pull
```
以在你的工作目录中 获取（ fetch ） 并 合并（ merge ） 远端的改动。

要合并其他分支到你的当前分支（例如  master ），执行：
```bash
git merge <branch>
```
两种情况下， git  都会尝试去自动合并改动。不幸的是，自动合并并非次次都能成功，并可能导致 冲突（ conflicts ）。 这时候就需要你修改这些文件来人肉合并这些 冲突（ conflicts ） 了。改完之后，你需要执行如下命令以将它们标记为合并成功：
```bash
git add <filename>
```
在合并改动之前，也可以使用如下命令查看：
```bash
git diff <source_branch><target_branch>
```
例：
```bash
git pull  从远程获取最新改动

git merge ice
```
比如当前的 branch  是 master ，那么就是将 ice  这个分支的内容合并到 master 中，但是可能会发生冲突

5.7.    标签

在软件发布时创建标签，是被推荐的。这是个旧有概念，在  SVN  中也有。可以执行如下命令以创建一个叫做  1.0.0  的标签：
```bash
git tag 1.0.0 1b2e1d63ff
```
1b2e1d63ff  是你想要标记的提交  ID  的前  10  位字符。使用如下命令获取提交  ID ：
```bash
git log
```
你也可以用该提交  ID  的少一些的前几位，只要它是唯一的。

5.8.    替换本地改动

假如你做错事（自然，这是不可能的），你可以使用如下命令替换掉本地改动：
```bash
git checkout -- <filename>
```
此命令会使用  HEAD  中的最新内容替换掉你的工作目录中的文件。已添加到缓存区的改动，以及新文件，都不受影响。

假如你想要丢弃你所有的本地改动与提交，可以到服务器上获取最新的版本并将你本地主分支指向到它：
```bash
git fetch origin
```
5.9.    删除一个 repository

点击 setting


然后点击：

Click Delete this repository in the Danger Zone™area


5.10.      分配合作用户

首先点击 setting

然后点击 collaborators

然后输入合作者的账号，搜索出后，点击 Add 即可


5.11.      创建一个新的 repositories


选择 repositories

然后点击 New

然后点击 create reposltory


5.12.      删除文件
```bash
git rm php/ice/Ice.php.bak
```
注： rm 后面需要跟上具体的路径
```bash
git commit –m “delte Ice.php.bak”
```
更新远程分支：
```bash
git push origin ice
```
然后上传后，更新同步本地：
```bash
git pull
```
5.13.      公司访问 git 访问不了

配置 host

184.50.87.50 a248.e.akamai.net 

5.14.      Git 操作 pullrequest

首先需要 fork 你想要进行合作的项目：

比如我 fork 的是 hiphop 那么就点击这里：

然后再你自己的 git 中就会 fork 出一个项目出来

然后你可以修改你的项目，然后点击 new pull request

然后点击 click to create a pull requeset for this comparsion

然后点击标题，添加内容

点击 send pull request


5.15.      git fork  同步原服务

（1）        首先 fork 服务

去已经有的项目中进行 fork 一个项目，然后也可以进行 pullrequeset, 这个操作参见 5.14

（2）        遇到问题

当原来的版本服务修改时，我们 fork 的服务就不好进行贡献代码，那么我们需要进行与远程的服务进行同步，那么下面就是同步的过程

（3）        克隆自己已经 fork 的服务到本地
```bash
git clone https://github.com/huzhiguang/hiphop-php.git
```
（4）        添加一个远程服务
```bash
git remote add upstream  https://github.com/facebook/hiphop-php.git
```
格式：

git remote add  远程仓库名称 远程仓库地址

（5）        抓取远程服务信息
```bash
git fetch upstream
```
格式：

git fetch  远程仓库名称

（6）        合并远程仓库信息到本地仓库中

git merge upstream/master

合并的过程中会遇到错误：

```bash
<<<<<<<HEAD

// 自己的代码

   /*

      author:huzhiguang

        date:2013/7/31

        function:string_md5function don't free and have memery leak

   */

  //md5 = MD5(string_md5(s, sz, false,out_len));

   char * string_md5_char=string_md5(s, sz,false, out_len);

   md5 = MD5(string_md5_char);

   free(string_md5_char);

=======

// 远程服务代码

  char * md5str = string_md5(s, sz, false,out_len);

  md5 = MD5(md5str);

  free(md5str);

>>>>>>> upstream/master
```
这里提示了冲突内容，我们将冲突的内容删除后，然后保存文件；

（7）        合并代码成功后，我们进行添加和提交代码

git add *

git commit –m “sync code”

git push origin master

（8）同步完成后，查看远程服务中的提交会有最新的同步后的结果


（9）在 pull request 中如果有由于冲突的文件造成的行等小差异解决后然后提交即可
