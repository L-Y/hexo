title: SVN 命令集
date: 2015-03-21 14:35:19
categories: svn
tags:
---
从http://subversion.tigris.org获取subversion for windows的版本，安装之后就有了svn.exe这个基于命令行的客户端工具。当然服务器端的程序也有了，这里就不关心如何配置SVN服务了。安装程序把svn.exe的路径加入了path环境变量,我们已经可以在命令行中直接输入svn就可以使用了。

如果你不知道命令怎么用svn命令,可通过如下方式查询：
``` bash
svn help
```
知道了子命令，但是不知道子命令的用法，还可以查询：
``` bash
svn help ci 
```
开发人员常用命令

导入项目
``` bash
svn import http://svn.chinasvn.com:82/pthread --message "Start project"
```
导出项目
``` bash
svn checkout http://svn.chinasvn.com:82/pthread
```
采用 export 的方式来导出一份“干净”的项目
``` bash
svn export http://svn.chinasvn.com:82/pthread pthread
```
为失败的事务清场
``` bash
svn cleanup
```
在本地进行代码修改，检查修改状态
``` bash
svn status -v
svn diff
```
更新(update)服务器数据到本地
``` basg
svn update directory
svn update file
```
增加(add)本地数据到服务器
``` bash
svn add file.c
svn add dir
```
对文件进行改名和删除
``` bash
svn mv b.c bb.c
svn rm d.c
```
提交(commit)本地文档到服务器
``` bash
svn commit
svn ci
svn ci -m "commit"
```
查看日志
``` bash
svn log directory
svn log file
```
相关的一些东西：
1、在本地文件中，每个目录下都有一个.svn文件夹(属性为隐藏)，保存了相关的信息。
2、注册环境变量SVN_EDITOR为"E:\Program Files\Vim\vim71\gvim.exe"，结果在svn ci的时候，出现错误:

'E:\Program' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
svn: 提交失败(细节如下):
svn: system('E:\Program Files\Vim\vim71\gvim.exe svn-commit.tmp') 返回 1

把SVN_EDITOR改为"gvim.exe"，并且在path中添加路径"E:\Program Files\Vim\vim71\",这样就可以在提交的时候用vim编写注释了。

附:
提供免费SVN服务的网站:
``` bash
http://www.svnhost.cn/(推荐)
http://www.chinasvn.com
http://www.javaforge.com
http://unfuddle.com
http://svn.coollittlethings.com/index.php(针对开源免费，针对私人项目收费)
```

一、在linux下
删除这些目录是很简单的，命令如下
find . -type d -name ".svn"|xargs rm -rf
或者
find . -type d -iname ".svn" -exec rm -rf {} \;  
全部搞定。
二、在windows下用以下法子：
1、在项目平级的目录，执行dos命令：
``` bash
xcopy project_dir project_dir_1 /s /i
```
2、或者在项目根目录执行以下dos命令
for /r . %%a in (.) do @if exist "%%a\.svn" rd /s /q "%%a\.svn"
其实第二种方法可以用来干很多事的，比如把代码中的.svn替换为任意其他文件名并在硬盘根目录下执行，就可以从硬盘上删除所有的这个文件啦。
3、加注册表
Jon Galloway提供了一段注册表代码，可以将”Delete SVN Folders”命名增加到资源管理器的右键上，这样，鼠标点两下就能把选中目录下的所有.svn目录干掉了。Works just great!
代码为：
``` bash
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Folder\shell\DeleteSVN] 
@="Delete SVN Folders" 
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Folder\shell\DeleteSVN\command] 
@="cmd.exe /c \"TITLE Removing SVN Folders in %1 && COLOR 9A && FOR /r \"%1\" %%f IN (.svn) DO RD /s /q \"%%f\" \""
```
将这段代码保存为一个.reg文件，双击确认导入注册表即可。
4、bat方式
    在SVN目录发生不可预知的错误需要重建或其他情况下需要把所有SVN信息清除时，可以用本站制作的维护小工具，把下面内容复制保存成扩展名 为.bat文件的批处理文件，使用时直接将要清除SVN信息的文件夹拖动到新建的bat文件上面，即可出现操作界面，按Y执行清除所有子目录下SVN信息 操作:
``` bash 
@echo off
if "%1"=="" (
 goto error
) else (
 goto action %1
)
:error
echo.
echo 必须输入要操作的文件夹路径参数，或拖动文件夹到此命令文件上。
echo.
pause
goto end
:action %1
echo --------------------------------------------------------------------------
echo 本次操作将删除 [%1] 文件夹下所有的svn标记，请慎重操作！
echo     Y 清理文件夹
echo     N 退出
echo ---------------------------------------------------------------------------
choice /c YN /m 请选择菜单(按ctrl+c或N退出)：
if %errorlevel% equ 2 goto end
echo 正在清理文件夹：%1
echo 请稍候...
for /r %1 %%a in (.) do @if exist "%%a\.svn" rd /s /q "%%a\.svn"
echo 清理完毕!
echo 按任意键退出...
pause>echo.
:end
exit
```