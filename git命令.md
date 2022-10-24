[toc] 

# git命令学习

- 一些常见的git命令的学习

## 创建版本库

`git init`

### git add 与 git commit -m

提交文件到仓库分两步

- 将文件**添加**到仓库
  - `git add a.txt`
- 将文件**提交**到仓库
  - `git commit -m '本次提交的描述'`
- 分两步的原因
  - 因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件

## 查看版本库状态

### git statues

`git status`

## 查看之前的提交

### git log

`git log [--pretty=oneline]`

## 查看历史的指令
### git reflog
`git reflog`

## 时光机版本回退与前进

### 去到任意版本的指令git reset --hard xx_id

`git reset --hard commit_id`

- commit_id
  - HEAD^代表上一版本
    - HEAD^^代表上上一版本以此类推
  - 也可以是利用`git log`查看的版本id号
    - 一般用于回退到以前的版本
  - 也可以是利用`git reflog`查看的历史提交命令的版本号
    - 一般用于回到未来

## 暂存区的概念
- 称为stage或者index

### git status能查看工作区的状态方便`git add xxx`

- 前缀为Untracked files的表示还从来没有被add过

- 前缀为changes not staged for commit的表示修改了文件还没有提交

## 查看工作区和版本库里面最新版本的区别

### git diff HEAD -- < file >

`git diff HEAD -- readme.txt`

## 撤销你的修改

### git checkout -- < file >

- 不能忽略--，忽略之后就成了切换分支的指令了
- 分两种情况
  - `readme.txt`自修改后还**没有被放到暂存区**，现在，撤销修改就回到和版本库一模一样的状态。**回到最新的版本库**
  - `readme.txt`已经**添加到暂存区**后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。**回到add前**

**例如：**

```git
## 1
$ vi readme.txt
$ git checkout -- readme.txt
## 2
$ vi readme.txt
$ git add readme.txt
$ git checkout -- readme.txt
```

### git reset HEAD < file >

可以把暂存区的修改撤销掉（unstage），重新放回工作区

- 意思就是撤销add，但是修改不会变，想要撤销修改的话需要再`git checkout -- <file>`



## 某客户端连接到自己的仓库的步骤

查看是否配置了git的username 和 email

`git config --lis`

然后创建`SSH`

`ssh-keygen -t rsa -C "youremail@example.com"` 一路回车

然后登陆GitHub，打开`“Account settings”`，`“SSH Keys”`页面：

然后点`“New SSH Key”`，填上任意`Title`，在`Key`文本框里粘贴`id_rsa.pub`文件的内容：



## 添加远程库

### 登录网站 创建仓库

- 登录`github.com`网站，点击右上角`＋`号，点击`Create a new repo`按钮，创建一个新的仓库
- 输入仓库的名字
- 默认设置

### 在本地终端输入指令连接到远程仓库

- 然后根据github的提示，在本地的learngit仓库下运行命令：
  - `git remote add origin git@github.com:luopanforever/learngit.git`
- 然后把本地库的所有内容推送到远程库上
  - `git push -u origin master`
  - `-u`参数只在第一次推送的时候添加  其意义是为了方便以后的推送或者拉取时可以简化命令
  - 第一次使用会发出警告  可以不用管
- 之后只要本地`commit`了就可以用下列命令将本地master分支的最新修改推送至GitHub仓库

## 删除远程库

### 撤销连接

首先查看远程库信息`git remote -v`

然后根据前缀来用指令`git remote rm <name>`撤销与远程库的连接

如果还想连接回来则`git remote add origin git@github.com:xxx.git `

## 克隆远程仓库

### git clone

`git clone git@gitbub.com:luopanforever/gitskills.git`



## 分支管理

### 创建与分支合并

#### 创建分支语法 checkout -b & branch

- 一 、创建的同时且转移
  - `git checkout -b dev` 创建分支并且令HEAD指向他
- 二、创建与转移分开
  - `git branch dev` 创建分支
  - `git checkout dev` 指向dev分支

#### 合并分支  merge

- HEAD指向master，此时想要合并dev使用下列语句

  - `git merge dev` 

  - 进行的操作是将master指向dev

#### 删除分支

##### git branch -d dev

`git branch -d dev`

### 解决冲突
#### 问题描述：

- 在main上创建一个新的分支且切换分支，然后修改README.md文件，并且提交到版本库

  - ```shell
    $ git checkout -b features
    $ vi README.md
    $ git add README.md
    $ git commit -m'change readme by feature'
    ```

- 切换回main分支，然后修改相同的README.md文件，并且提交到版本库

  - ```shell
    $ git branch main
    $ vi README.md
    $ git add README.md
    $ git commit -m'change readme by main'
    ```

#### 冲突出现

此时在main分支上`git merge feature`的话就会出现冲突，需要在main分支下手动去**修改**readme.md文件来**解决冲突**

之后feature不需要了直接删除`git branch -d feature`

### 分支管理策略

#### 利用`merge --no-ff -m'description' branch_name`来代替直接merge带来的Fast forward模式，来显式的在`git log --graph --pretty=oneline --abbrev-commit`中显示分支

### BUG分支

#### git stash的介绍

- 各种语法
  - `git stash` 隐藏add后的提交
  - `git stash list` 查看隐藏的空间
  - `git stash pop` 恢复隐藏空间并且删除隐藏空间的缓存
    - 可以分为以下两步
      - `git stash apply stash@{?} ` 恢复？号隐藏空间
      - `git stash drop stash@{?}` 删除？号隐藏空间缓存

- git stash之后可以隐藏当前的工作现场
  - 意思是在这之前git status有很多内容  git stash 之后git status 就变干净了
  - 之后去master主分支上就看不见dev的提交啦

#### git cherry-pick < commit_id >介绍

- 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动

### feature分支

- 开发一个新feature，最好新建一个分支；

- 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

### 多人协作

。。。
