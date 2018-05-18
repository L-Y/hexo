title: hexo deploy failed
date: 2018-05-19 00:09:45
categories:
tags:
---
## question
---
### INFO  Deploying: git
#### INFO  Clearing .deploy folder...
#### FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
#### Error: EACCES: permission denied, unlink '/home/yang/hexo/.deploy_git/archives/index.html'
#### FATAL EACCES: permission denied, open '/home/yang/hexo/db.json'
#### Error: EACCES: permission denied, open '/home/yang/hexo/db.json'
### * answer:
#### 1. sudo ssh-keygen -t rsa -b 4096 -C "xxx@xxx.com"
#### 2. copy /var/root/.ssh/id_rsa/id_rsa_pub -> github/settings/SSH and GPG keys/new ssh key
#### 3. hexo deploy
#### * answer2:
#### 1. su root
#### 3. hexo deploy
