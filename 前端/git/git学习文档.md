# git学习文档

## .gitignore文件的作用

一般在git管理的一个项目中都会创建一个`.gitignore`文件

该文件的作用：**是提交时忽略的文件或文件夹**

例如：在文件中写入

```
node_modules
```

提交时就不会提交node_modules文件夹（一般node项目我们都不会选择提交这个文件，到时候直接npm install以下就可以下载回来）

## ReadMe文件的作用

ReadMe一般都是用来记录这个项目的一些操作呀，介绍项目呀等待的功能



## git的一些基础操作

git init：初始化项目（创建仓库时先初始化一下）

git status：检查状态(让我们时刻掌握仓库当前的状态）可以看到那些文件变动了

git diff 文件名：可以看到文件修改了那些内容

git clone xxx ：克隆

git checkout -b xxx：创建xxx分支

git branch：查看当前项目所有分支

git add . ：表示添加到暂存区

git checkout xxx: 切换分支

git push origin -delete xxx: 删除服务器远端的分支

git branch -d xxx：删除本地已经合并了的分支

git branch --merged：查看有哪些合并到当前分支

```
git checkout master
```

git commit ：提交到本地仓库中

```git
git commit -m "完成了xxx功能"
```

## 提交到码云上的完整途径

第一步：将代码保存到本地仓库中

```
分支保存代码是：
git commit -m "完成了xxx功能"
主支是合并分支，保持本地仓库中的主支最新：
git merge 分支名
```

第二部：再将分支（主支）提交到码云云端上



## 在码云上创建一个新分支

```
第一步切换到该分支下：
git checkout xxx
第二步：推送
git push -u origin xxx
```



## 合并分支并将本地master上的代码更新：

第一步：切到主支上 git checkout 主支名

第二部：合并分支 git merge 分支名

```
git checkout master
git merge 分支名
```

合并之后可以将最新的master提交到云端

git push：将本地仓库中最新的代码推送到云端码云上



## 推送一个分支到云端码云上的步骤

第一步：先创建分支（敲完代码创建，和先创建）都一样

```
git checkout -b 分支名
```

第二部：然后将分支添加到暂存区：

```
git add .
```

第三步：如果第一次将分支提交到码云

```
// 第一次
git push -u origin 分支名
// 如果不是第一次就
切换到分支然后：git push
```

第四步：更新master再将最新的master提交到码云

```
//切换到master 然后合并
git checkout master
//合并
git merge 分支名
//推送
git push
```

##  将写好的代码提交到本地分支上(保持分支代码最新)

```
第一步：将分支代码添加到暂存区
git add .
第二步：将写好的代码提交到自己的本地分支上
git commit -m "完成了xxx功能..."
```



### 将文件添加到git仓库

```
第一步：
git add 文件名

第二步：
git commit -m '提交的描述'
```



### 查看修改的文件

```
git status:可以查询到那些文件变动

git diff 文件名:可以看到文件变动的内容
```



### 版本回退



```
git log :可以看到历史变动记录
会返回详细的变动（一大串内容）--查询后输入字母q退出

git log --pretty=oneline:简化版
```



在git中`HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`表示上上个版本。

上100个版本用`HEAD~100`表示

`HEAD 提交的commit id`可以回到指定版本，这个commit id 可以通过`git log` 看到这个id不用写全，写前5~6位就行。

```
git reset --hard HEAD^:回退到上个版本
```



```
git reflog:记录了每一次命令 第一位数字就是commit id，可以根据id 回退

git reset --hard xxxxx:叉叉为commit id
```



### 重要步骤

一定先要 git add 添加到暂存区， 然后 再git commit 提交上去



### 撤销修改

所有的撤销和修改都是在工作区和暂存区。如果将本地版本库推送到远端之后，那就无能为力了。

这个 -- 很重要，如果没有就和查询分支一样了

```
git checkout -- 文件名: 这个操作会让该文件回到最近一个git commit或者git add时的状态。

gitreset HEAD <file>: 可以把暂存区的修改撤销掉。
```



### 删除文件

如果你手动删除了文件管理器的文件，git 会察觉到。通过git status 可以看到。

一、你真的想要删除版本库中的文件

```
1.
git rm <file>: 从版本库中删除文件

2.
git commit -m 'remove file': 提交
```

这样就真的从版本库中删除文件了。



二、删除错了，因为版本库中还保存着你之前存放的文件，可以回退

```
git checkout -- <file>: 用版本库中的文件替换工作区里的文件
```

**总结**：命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。



## 远程仓库

关联远程仓库：

```
git remote add origin xxxx: 关联远程仓库

这个origin就是远程库的名字。
xxx 是公司给你的或者GitHub的

git remote -v: 查看远程库信息
```



把本地仓库推送到远程：

```
git push: 可以将本地仓库推送到远端

如果是一个新的远端仓库：
git push -u origin master: 加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

一单本地仓库做了提交 git commit -m 'xxx'，我们就可以将本体master推送到远端
git push origin master
```



删除远程库：

如果添加远程库时地址写错了，或者想要删除远端仓库的关联。

```
先查看远程库信息
git remote -v: 查看远程库信息

然后根据名字删除 比如删除origin
git remote rm origin: 就删除了本地库和远程库的关联
```



## 远端仓库拉取代码

分很多中情况

第一种：本地代码未修改，只有master分支，直接更新

```
git pull
```

但前提必须是本地的代码没更改过。比如，你提交了代码到 github 后，随后别人也提交代码到 github，然后你需要更新别人提交的代码到你本地，

就可以直接使用该命令。**假如你提交代码后再修改过你本地的代码，就会产生冲突，直接使用该命令会失败的**

第二种：本地代码有修改，多分支

```
// 切换到分支
git switch master

// 更新master分支
git pull

// 切换到自己的分支issue
git checkout issue

// 把master分支合并到自己的分支上
git merge master
```



第三种：本地代码有修改，只有master分支，直接覆盖本地代码

```
// 重置索引和工作目录
git reset --hard

// 更新代码
git pull
```



### 分支系统

可以在master上创建一个分支，在分支上提交工作，当分支的工作完毕之后，再切回master将分支合并，就将分支上完成的工作合并到了master上



一、创建分支，并切换分支：

```
git checkout -b 分支名: 加上参数-b标识创建并切换到该分支
git branch: 查看当前分支

相当于
git branch 分支名: 创建分支
git checkout 分支名: 切换到分支
```

创建并切换分支还有一种操作：

```
git switch -c 分支名: 创建并切切换分支

git switch master: 切换到已有的master分支
```







二、当分支的工作完成，切回master并没有被修改，然后将分支内容合并：

```
git merge 分支名: 将分支内容合并到当前分支
```



三、分支合并完后可以将删除刚刚被合并的分支了。

```
git branch -d 分支名: 删除分支
```





### 解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

​	

### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

一、`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

二、那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

三、你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。



### Bug 分支

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。



当突然遇到一个bug需要修理，但是自己的工作还没有完成只完成了一半，无法提交。

Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：



一、首先将自己暂时的工作stash一下，分支为dev

```
git stash: 把当前工作环境存储一下
```

二、查询那个分支有BUG需要修复，就切到该分支下创建一个新分支。

例如：master分支下下有BUG，切换到master并创建issue01分支

```
git switch master: 切换到master分支下

git switch -c issue01: 创建issue01分支
```



三、在分支issue01解决bug，合并分支issue01

```
git switch master: 切回master

git merge --no--ff -m "修复bug" issue01: 合并issue01


```



四、修改完毕BUG切回自己工作分支dev，并恢复

```
git stash list: 查看stash内容，也就之前stash过的

```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了



你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```
git stash apply stash@{0}: 恢复指定stash
```



五、在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在。

可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

`<commit>`是之前提交issue01时的id，找到即可



### 删除分支（丢弃一个没有被合并的分支）

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。



### 多人协作模式



1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

