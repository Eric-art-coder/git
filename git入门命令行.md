# Git远程操作详细
> 这个主要是参考[blog](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)

详细如下：
**本文详细介绍了5个基本的git命令**  
- git clone  
- git remote  
- git fetch  
- git pull  
- git push  

**本文只是针对git初级用户，这不包括完全小白，如果是完全明白，基本上是可以覆盖所有的普通的操作！**

![image](https://github.com/guimeisang/git/blob/master/img/git%E5%85%A5%E9%97%A8%E5%9F%BA%E6%9C%AC%E5%9B%BE.jpg)

## 一、==git clone==
远程操作的第一步，通常是从远程主机克隆一个版本库，这时就要用到git clone命令。

```
$ git clone <版本库的网址>
```
比如：克隆jQuery的版本库。

```
$ git clone https://github.com/jquery/jquery.git
```
git clone支持多种协议，除了HTTP(s)以外，还支持SSH、Git、本地文件协议等，下面是一些例子。

```
$ git clone http[s]://example.com/path/to/repo.git/
$ git clone ssh://example.com/path/to/repo.git/
$ git clone git://example.com/path/to/repo.git/
$ git clone /opt/git/project.git 
$ git clone file:///opt/git/project.git
$ git clone ftp[s]://example.com/path/to/repo.git/
$ git clone rsync://example.com/path/to/repo.git/
```
**通常来说，Git协议下载速度最快，SSH协议用于需要用户认证的场合。各种协议优劣的详细讨论请参考[官方文档](http://git-scm.com/book/en/Git-on-the-Server-The-Protocols)。**  

## 二、==git remote== 
为了便于管理，Git要求每个远程主机都必须指定一个主机名。**git remote命令就用于管理主机名**。  
不带选项的时候，git remote命令列出所有远程主机。

```
$ git remote
origin
```
使用-v选项，可以参看远程主机的网址。

```
$ git remote -v
origin  git@github.com:jquery/jquery.git (fetch)
origin  git@github.com:jquery/jquery.git (push)
```
上面命令表示，当前只有一台远程主机，叫做origin，以及它的网址。  
克隆版本库的时候，所使用的远程主机自动被Git命名为origin。如果想用其他的主机名，需要用git clone命令的-o选项指定。

```
$ git clone -o jQuery https://github.com/jquery/jquery.git
$ git remote
jQuery
```
==git remote show==命令加上主机名，可以查看该主机的详细信息。  

```
$ git remote show <主机名>
```
==git remote add==命令用于添加远程主机。

```
$ git remote add <主机名> <网址>
```
==git remote rm==命令用于删除远程主机。

```
$ git remote rm <主机名>
```
==git remote rename==命令用于远程主机的改名。

```
$ git remote rename <原主机名> <新主机名>
```
## 三、git fetch
一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到==git fetch==命令。

```
$ git fetch <远程主机名>
```
上面命令将某个远程主机的更新，全部取回本地。  
git fetch命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。  
默认情况下，git fetch取回所有分支（branch）的更新。如果只想取回特定分支的更新，可以指定分支名。  

```
$ git fetch <远程主机名> <分支名>
```
比如，取回origin主机的master分支。  

```
$ git fetch origin master
```
所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如origin主机的master，就要用origin/master读取。  
git branch命令的-r选项，可以用来查看远程分支，-a选项查看所有分支。  

```
$ git branch -r
origin/master

$ git branch -a
* master
  remotes/origin/master
```
上面命令表示，本地主机的当前分支是master，远程分支是origin/master。  
取回远程主机的更新以后，可以在它的基础上，使用git checkout命令创建一个新的分支。  

```
$ git checkout -b newBrach origin/master
```
上面命令表示，在origin/master的基础上，创建一个新分支。  
此外，也可以使用git merge命令或者git rebase命令，在本地分支上合并远程分支。  

```
$ git merge origin/master
# 或者
$ git rebase origin/master
```
上面命令表示在当前分支上，合并origin/master。
## 四、git pull  
git pull命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。它的完整格式稍稍有点复杂。  

```
$ git pull <远程主机名> <远程分支名>:<本地分支名>
```
