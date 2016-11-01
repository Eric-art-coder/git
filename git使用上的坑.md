# 如何提交日记到github上
> 比如你更新了日记之后，运行下面几个命令行就可以将最新的日记，push上去
- 在Diary文件夹里，打开git bash Here
- git add . 或者是 git add [文件名.文件后缀]
- git commit -m "这里写你的描述"
- git remote add origin [这里写你的https路径]
- git push -u origin master 

> 不过我现在打算使用sourceTree


# 如何理解打分支和合并

详细还是请看[廖雪峰的介绍](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)
- $ git checkout -b dev 创建dev分支并且切换到dev分支 $ git branch dev 和 $ git checkout dev
- $ git checkout dev 切换到dev分支
- $ git branch 查看当前本地分支，当前分支前会有一个*

然后就可以在dev分支上正常提交了，比如对readme.txt做一个修改，加上一行：`Creating a new branch is quick.`

然后提交：
```
$ git add readme.txt 
$ git commit -m "branch test"
[dev fec145a] branch test
 1 file changed, 1 insertion(+)
```
现在，dev分支的工作完成，我们就可以切换回master分支：

```
$ git checkout master
Switched to branch 'master'
```

切换回master分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：

初始化状态

![image](https://github.com/guimeisang/Diary/blob/master/img/01.png)

创建dev分支并且header指针指向当前dev分支

![image](https://github.com/guimeisang/Diary/blob/master/img/02.png)

当前dev分支继续commit和push

![image](https://github.com/guimeisang/Diary/blob/master/img/04.png)

- $ git checkout master 先将head指针指向master上面  然后 $ git merge dev 现在，我们把dev分支的工作成果合并到master分支（注意当前的分支一定master分支，需要合并的分支）

将master合并提前到dev上去

![image](https://github.com/guimeisang/Diary/blob/master/img/03.png)

- $ git merge [branch] 用于合并指定分支到当前分支，这个合并相当于快进模式，直接把`master`和`dev`的当前提交，所以合并非常快。
- $ git branch -d dev 合并之后就可以放心的删除dev分支了。
- $ git remote set-url origin [路径]：可以更改远程仓库的路径。