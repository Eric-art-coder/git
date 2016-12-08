# Git分支管理
> 在正式开发中，以下会就几个比较典型的情况进行详细指导

详细如下：
版本的分支(branch)和合并（merge）是否方便。有代码的物理拷贝。但是git只生成一个指向当前版本的指针(header)；这个就是得git很方便
但是这个好处会带来一些不便之处：就是你不加注意的话，很有可能会留下一个枝节曼生/四处开放的版本库。到处都是分支的脉络。
下面有一种策略，它可以使得版本库演进保持简洁，主干清晰，各分支清晰各司其职，井井有条。

##　一、主分支Master
首先，代码库应该有一个，且有一个主分支。所有提供用户使用的正式版本，都在这个主分支上面发布。
![image](https://github.com/guimeisang/git/blob/master/img/bg1.png)

Git主分支的名字，默认叫做Master，它是自动建立的。版本库初始化以后，默认就是在主干上面开发。

## 二、开发分支dev
主分支主要是用来分布重大版本，日常开发应该在另一条分支上面完成的。我们把开发叫做dev
![image](https://github.com/guimeisang/git/blob/master/img/bg2.png)

这个分支可以用来生成代码的最新隔夜版本(nightly)。如果是想正式对外发布，就在Master分支上，把master分之合并到dev（此时dev已经提前几个commit了）
- Git创建dev分支
```
git checkout -b dev master
```
然后你就在dev上面，写你的代码，写完之后，git add <你的文件> → git commit -m "记录" → git push。开发完之后
将dev分支发布到Master分支的命令：
```
# 切换到Master分支
git checkout master
# 对Develop分支进行合并（--no-ff参数代表着:Git执行"快进式合并"（fast-farward merge），会直接将Master分支指向Develop分支）
git merge --no-ff dev -m "将master分支merge到dev上去"
```
在master分支上面merge完之后，还需要，对merge的文件进行 git commit和git push 命令！

 ![image](https://github.com/guimeisang/git/blob/master/img/bg3.png)

 使用--no-ff参数后，会执行正常合并，在Master分支上生成一个新节点。为了保证版本演进的清晰，我们希望采用这种做法。关于合并的更多解释，请参考Benjamin Sandofsky的
 [《Understanding the Git Workflow》](http://sandofsky.com/blog/git-workflow.html)

  ![image](https://github.com/guimeisang/git/blob/master/img/bg4.png)

## 三、临时性分支

前面讲到版本库的两条主要分支：Master和Develop。前者用于正式发布，后者用于日常开发。其实，常设分支只需要这两条就够了，不需要其他了。

但是，除了常设分支以外，还有一些临时性分支，用于应对一些特定目的的版本开发。临时性分支主要有三种：
- 功能（feature）分支
- 预发布（release）分支
- 修补bug（fixbug）分支
**这三种分支都属于临时性需要，使用完以后，应该删除，使得代码库的常设分支始终只有Master和Develop。**

## 四、 功能分支

接下来，一个个来看这三种"临时性分支"。
第一种是功能分支，它是为了开发某种特定功能，从Develop分支上面分出来的。开发完成后，要再并入Develop。
 ![image](https://github.com/guimeisang/git/blob/master/img/bg5.png)

功能分支的名字，可以采用feature-*的形式命名。
创建一个功能分支：
```
git checkout -b feature-x develop
```
在feature-x分支上开发完成后()，将功能分支合并到develop分支：
```
git checkout develop
git merge --no-ff feature-x
```
merge之后再dev分支上，git commit 和git push 将最新的文件push上去。
删除feature分支：
```
git branch -d feature-x
```

## 预发布分支
第二种是预发布分支，它是指发布正式版本之前（即合并到Master分支之前），我们可能需要有一个预发布的版本进行测试。
预发布分支是从Develop分支上面分出来的，预发布结束以后，必须合并进Develop和Master分支。它的命名，可以采用release-*的形式。

创建一个预发布分支：
```
git checkout -b release-1.2 develop
```

确认没有问题后，合并到master分支：
```
git checkout master
git merge --no-ff release-1.2
# 对合并生成的新节点，做一个标签
git tag -a 1.2
```


再合并到develop分支：
```
　git checkout develop
　git merge --no-ff release-1.2
```
记得在master和dev上面，将更新的文件commmit和push上去
最后，删除预发布分支：
```
git branch -d release-1.2
```
##　六、修补bug分支
最后一种是修补bug分支。软件正式发布以后，难免会出现bug。这时就需要创建一个分支，进行bug修补。
修补bug分支是从Master分支上面分出来的。修补结束以后，再合并进Master和Develop分支。它的命名，可以采用fixbug-*的形式。

 ![image](https://github.com/guimeisang/git/blob/master/img/bg6.png)

 创建一个修补bug分支：
 ```
 git checkout -b fixbug-0.1 master
 ```
 修补结束后，合并到master分支：
 ```
 git checkout master
 git merge --no-ff fixbug-0.1
 git tag -a 0.1.1
 ```
 再合并到develop分支：
 ```
 git checkout develop
 git merge --no-ff fixbug-0.1
 ```
 最后，删除"修补bug分支"：
 ```
 git branch -d fixbug-0.1
 ```

 (完)

 ## 七、重命名远程分支
 
 在开发中，我们需要管理好自己的分支。  
 有一种场景就是本来在dev分支进行开发的，但是为了区别版本号，需要将dev改成v1.2.0版本  
 解决的思路就是：先删除远程的分支，然后重命名本地分支，再重新提交一个远程分支。  

 1. 先查一下所以的分支
 ```
$ git branch -av
* dev                           c38c6b2 异步加载，缓存 commit
  master                        b70dd0f 有问题的文件上传做个提示...
  remotes/origin/HEAD           -> origin/master
  remotes/origin/dev            c38c6b2 异步加载，缓存 commit
  remotes/origin/master         b70dd0f 有问题的文件上传做个提示...
  remotes/origin/release-v1.1.0 b70dd0f 有问题的文件上传做个提示...
 ```

2. 删除远程分支
```
$ git push --delete origin dev
To https://xxx.com/xxx/xxx.git
 - [deleted]         dev
```

3. 重命名本地分支：
```
git branch -m devel dev-v1.2.0
```

4. 推送本地分支：
```
$ git push origin dev-v1.2.0
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (13/13), done.
Writing objects: 100% (20/20), 3.79 KiB | 0 bytes/s, done.
Total 20 (delta 8), reused 19 (delta 7)
remote:
remote: Create merge request for dev-v1.2.0:
remote:   https://xxx/merge_reque            
sts/new?merge_request%5Bsource_branch%5D=dev-v1.2.0
remote:
To https://xxx.git
 * [new branch]      dev-v1.2.0 -> dev-v1.2.0
```

5. 再查看下所有的branch情况
```
* dev-v1.2.0                    c38c6b2 异步加载，缓存 commit
  master                        b70dd0f 有问题的文件上传做个提示...
  remotes/origin/HEAD           -> origin/master
  remotes/origin/dev-v1.2.0     c38c6b2 异步加载，缓存 commit
  remotes/origin/master         b70dd0f 有问题的文件上传做个提示...
  remotes/origin/release-v1.1.0 b70dd0f 有问题的文件上传做个提示...
```

6. 在删除远程分支的时候，容易碰到的问题就是如下：
```
$ git push --delete origin devel
remote: error: refusing to delete the current branch: refs/heads/devel
To git@github.com:zrong/quick-cocos2d-x.git
 ! [remote rejected] devel (deletion of the current branch prohibited)
error: failed to push some refs to 'git@github.com:zrong/quick-cocos2d-x.git'
```
这是由于在 github 中，devel 是项目的默认分支。要解决此问题，这样操作：  
- 进入 github 中该项目的 Settings 页面；
- 设置 Default Branch 为其他的分支（例如 master）；
- 重新执行删除远程分支命令。






> 该文主要参考[blog](http://www.ruanyifeng.com/blog/2012/07/git.html)
