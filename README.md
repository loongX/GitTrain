## git 操作规范

### 一、 创建与合并分支

**1、 从master分支创建dev分支并切换到dev分支**

```
git checkout master

git checkout -b dev
```

其中，git checkout -b dev 等价于:

```
git branch dev

git checkout dev
```

（1）

```
git branch
```

查看本地当前的分支，分支前面带“*”表示当前分支，剩下的分支表示本地有的分支。

（2）

```
git branch -a
```

查看远程全部的分支，白色的表示本地有的，红色的表示本地没有，仅在远程存在。

**2、修改代码、提交代码（当前的操作是在dev分支上进行）**

```
git add a.html

git commit -m "提交文件a.html"
```

**3、分支合并(将dev合并到master)**

```
git checkout master 

git merge dev
```

**4、合并完成后，删除dev分支.(删除dev分支时，注意我们当前所在的分支不能是dev分支)**

```
git branch -d dev
```

**5、删除后，查看分支(此时看不到dev分支了)**

```
git branch
```

**6、总结 ：工作中经常从master创建新的分支，具体操作如下**

```
master创建新分支：

git checkout master

git checkout -b  issues1234

git push origin issues1234

git add ..

git commit -m "***"

git push origin issues1234
```

> 注意：将本地分支branch1推到远端的branch2操作步骤：

```
    git push origin branch1:branch2
```

**7、删除分支：**

```
git branch -D   issues1234  //本地强制删除分支issues1234

git push origin  :issues1234  //推到远程
```

### 二、 解决冲突

**1、发生冲突的文件**

```
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

其中，git使用<<<<<<<，=======，>>>>>>>标记文件中自己和别人产生冲突的部分。

在<<<<<<<，=======之间为自己的代码；
=======，>>>>>>>之间为别人的代码。

如果保留自己的代码，将别人的代码删掉即可。

**2、冲突解决后提交**

```
git status

git add ***

git commit -m "fix conflict"

git push origin 分支名
```

### 三、Bug分支

**1、储藏更改:将当前更改的代码储藏起来，等以后恢复使用**

```
git stash
```

**2、恢复储藏的代码**

```
git stash pop //恢复的同时把stash内容删掉

或者

git stash apply  //恢复stash，但是stash内容并不删除

git stash drop //在上面操作的基础上，以此来删除stash


注： git stash list //查看全部的stash列表
```

**3、将stash空间清空**

```
git stash clear
```

**4、git stash pop 和 git stash apply 区别**

```
原来git stash pop stash@{id}命令会在执行后将对应的stash id 从stash list里删除，
而 git stash apply stash@{id} 命令则会继续保存stash id。
```

### 四、版本回退

**1、回退至上一个版本**

```
git reset --hard HEAD 
```

**2、回退至指定版本**

```
git reset --hard  版本号
```

**3、查看以往版本号(本地的commit)**

```
git reflog
```

**4、查看各版本号及信息(所有的commit：本地commit + 其他同事的commit)**

```
git log
```

### 五、撤销修改

**1、撤销修改**

```
git  checkout -- a.html
```

> 分两种情况分析：

```
①： 还没有执行 git add 操作，执行上面的操作后，会恢复到和版本库中一模一样的版本状态。

②： 执行了git add ，还没执行 git commit ,再执行上面的操作后，会恢复到git add 结束后的状态
```

注：一旦执行了git commit -m "***"，就不能再使用上面的命令回退。

**2、撤销新建文件**

比如新建一个aa.html页面，并未执行git add ,即没有被git追踪，此时如果你想撤销新建动作，可执行：

```
git clean -f ../aa.html
```

**3、撤销新建文件夹**

比如新建一个文件夹"demo"，并未执行git add ,即没有被git追踪，此时如果你想撤销新建动作，可执行：

```
git clean -df ./demo
  
```

### 六、对于已经push的版本，进行回退

**1、第一步：**

```
git reset --hard 版本号 //本地回退到指定的版本
```

**2、第二步：**

```
git push  -f origin dev    //将远程的也回退到指定版本
```

### 七、本地同步远程删除的分支

```
git fetch origin -p  //用来清除已经没有远程信息的分支，这样git branch -a 就不会拉取远程已经删除的分支了
```

### 八、删除掉没有与远程分支对应的本地分支

从gitlab上看不到的分支在本地可以通过git branch -a 查到，删掉没有与远程分支对应的本地分支：

```
git fetch -p
```

### 九、查看远程库的一些信息，及与本地分支的信息

```
 git remote show origin 
```

git remote show命令加上主机名，可以查看该主机的详细信息。

```
$ git remote show <主机名>
```
git remote add命令用于添加远程主机。

```
$ git remote add <主机名> <网址>
```
git remote rm命令用于删除远程主机。

```
$ git remote rm <主机名>
```
git remote rename命令用于远程主机的改名。

```
$ git remote rename <原主机名> <新主机名>
```
###  十、打标签

列出现有标签

```
$ git tag
$ git tag -l 'v1.4.2.*'
```

新建标签

创建一个含附注类型的标签

```
$ git tag -a v1.4 -m 'my version 1.4'
```

 `-m` 选项则指定了对应的标签说明，Git 会将此说明一同保存在标签对象中。如果没有给出该选项，Git 会启动文本编辑软件供你输入标签说明。

查看标签

```
git show v1.5
```

轻量级标签

 `-a`，`-s`或 `-m` 选项都不用，直接给出标签名字即可

```
$ git tag v1.4
```

后期对早先的某次提交加注标签

```
$ git log --pretty=oneline #展示的提交历史

$ git tag -a v1.2 9fceb02
```

分享标签

默认情况下，`git push` 并不会把标签传送到远端服务器上，只有通过显式命令才能分享标签到远端仓库。其命令格式如同推送分支，运行 `git push origin [tagname]` 即可

```
$ git push origin v1.5
```

一次推送所有本地新增的标签上去

```
$ git push origin --tags
```

### 十一、图形化查看

cd进目录里面然后打开gitk

```
$ gitk [git log options]
```

查看其它分支

```
$ gitk dev
```
gitk 是 git gui 中的一个小工具
```
$ git gui
```



### 12、 git pull

```
$ git pull <远程主机名> <远程分支名>:<本地分支名>
```
远程分支是与当前分支合并
```
$ git pull origin next
```
上面命令表示，取回origin/next分支，再与当前分支合并。实质上，这等同于先做git fetch，再做git merge。

```
$ git fetch origin
$ git merge origin/next
```
手动建立追踪关系
```
git branch --set-upstream master origin/next
```
### 13. git push

```
$ git push <远程主机名> <本地分支名>:<远程分支名>
```

不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机

```
$ git push --all origin
```

如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做`git pull`合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用`--force`选项。

```
$ git push --force origin 
```

使用`--force`选项，会使远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用`--force`选项。

`git push`不会推送标签（tag），需要用`--tags`选项来提交所有tag。

```
$ git push origin --tags
```
删除远程分支
```
#省略本地分支名，则表示删除指定的远程分支，等同于推送一个空的本地分支到远程分支
$ git push origin :master 
# 等同于
$ git push origin --delete master
```

