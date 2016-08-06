title: Hexo本地文件托管到GitHub
date: 2015-06-25 15:27:20
tags: hexo
---


像我就不知道什么时候把Hexo的本地文件弄丢了，所以重部署Hexo本地文件夹之后就决定将Hexo本地文件托管到GitHub，以防再次丢失。
托管步骤
1.在GitHub上建立一个新的repository，名字与本地Hexo文件相同
2.在本地Hexo文件Git Bash here，首先用git初始化管理这个目录
```bash
git init
```
3.把Hexo所有文件加入到git的管理中
```bash
git add .
```

此时使用git status可以看到一大片绿的文件名，表示这些文件的管理添加成功
4.使用命令git commit -m "first commit",将这些文件提交到本地仓库
5.使用命令git remote origin http://github.com/username/yourRepositoryName.git把本地文件托管到该远程仓库
6.使用命令git push origin master，将本地仓库的改动推送到远程仓库
同步命令
同步到本地
1.clone整个Hexo文件
1
```bash
git clone https://你的托管地址.git
```
这样即使本地Hexo文件丢失或者换了新的电脑也可以将Hexo的源文件复制到本地了
2.同步更新到本地
当远程仓库有更新时，使用git pull或git fetch即可同步代码到本地
同步到github
写博客后，使用如下命令提交更新到远程仓库
```bash
git add .
git commit -m 
git push
```
