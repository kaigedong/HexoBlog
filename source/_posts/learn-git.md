---
title: learn-git
date: 2018-04-13 17:51:19
tags:
---


# git 学习笔记(ubuntu)
from 廖雪峰git笔记

<!-- more -->

## 安装
`sudo apt install git`

## 首次配置
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
>因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址
>注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

## 创建版本库
>版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

```
#在某个合适的目录下
mkdir learngit
cd learngit
pwd
```
>/home/dong/Documents/learngit

```
#通过git init 命令把这个目录编程Git可以管理的仓库
git init
#Initialized empty Git repository in /home/dong/Documents/learngit/.git/
```

### 把文件添加到版本库

所有的版本控制系统，**只能跟踪文本文件**的改动，比如TXT文件，网页，所有程序代码等。图片，视频是不知道的。**强烈建议使用UTF-8编码**。

**创建一个`readme.txt`文件**:

```
Git is a version control system.
Git is free software.
```

**一定要放在`learngit`目录下（子目录也行）。

把一个文件放到Git仓库只需要两步：

### 1. 用`git add`告诉Git，把文件添加到仓库：

```shell
   git add readme.txt
```
>git add --all #添加所有文件
>git add -A #同上

### 2. 用`git commit`告诉Git，把文件提交到仓库：

```
   git commit -m "wrote a readme file"
   [master (root-commit) cb926e7] wrote a readme file
    1 file changed, 2 insertions(+)
    create mode 100644 readme.txt
```

> `git commit`说明：`-m`后面输入的是本次提交说明，可以输入任何内容，最好是有意义的。
>
> `git commit`命令执行成功后会告诉你，1个文件被改动（我们新添加的readme.txt文件），插入了两行内容（readme.txt有两行内容）。
>
> 为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：
>
> ```
> $ git add file1.txt
> $ git add file2.txt file3.txt
> $ git commit -m "add 3 files."
> ```

### 小结
初始化一个Git仓库，使用`git init`命令。
添加文件到Git仓库，分两步：

- 第一步，使用命令`git add `，注意，可反复多次使用，添加多个文件；
- 第二步，使用命令`git commit`，完成。


## 查看文件状态

### 1. git status

修改`readme.txt` 的两行内容，然后使用`git status`命令查看结果：

```shell
$git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#    modified:   readme.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```

`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。

### 2. git diff file

修改了文件，但没提交。已经记不清怎么修改的`readme.txt`了，所以需要`git diff`命令来查看

```shell
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

知道了对readme.txt作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是`git add`：

```shell
 git add readme.txt
```

**在执行第二步`git commit`之前，我们查看下状态`git status`

```shell
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   readme.txt
#
```

`git status`告诉我们，将要被提交的修改包括readme.txt，下一步，就可以放心地提交了：

然后再用`git status`查看仓库的当前状态：

```
$ git commit -m "add distributed"
[master ea34578] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working directory clean）的。

### 小结

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。


## 版本回退

### git log --pretty=oneline 查看提交日志

实际工作中，我们可以用`git log`查看每次修改了什么。**`git log `显示从最近到最远的提交日志。**可以加上`--pretty=oneline`参数，让`git log`输出的信息更简洁。

git log 的输出结果：一大串数字是`commit id`（版本号）。

git 中，用`HEAD`表示当前版本，上个版本就是`HEAD^`，上上个版本就是`HEAD^^`，上一百个简写为`HEAD~100`。

#### git reset --hard HEAD^回到上个版本

```shell
$ git reset --hard HEAD^
HEAD is now at ea34578 add distributed
```

>现在再用`git log`就看不到刚才的第一条记录了！
>Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向append GPL：改向只想`add distributed`.
>**所以让HEAD指向哪个版本号，就把当前版本定位在哪。**

#### git reflog 查看版本的记录！



## git 撤销工作区、暂存区更改

#### 1. 直接丢弃工作区修改

场景1：当你改乱了工作区某个文件的内容，想**直接丢弃工作区**的修改时，用命令`git checkout -- file`。

#### 2. 先丢弃暂存区，在丢弃工作区修改

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。

#### 3. 版本回退

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。



### git rm 删除一个文件 并且`git commit`

```shell
$ git rm test.txt
rm 'test.txt'
$ git commit -m "remove test.txt"
[master d17efd8] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

## 远程仓库

>请自行注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

+ 第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：


```shell
ssh-keygen -t rsa -C "youremail@example.com"
```

> 你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

+ 第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容。


### 添加远程仓库

1. 首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：

2. 在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：

3. 目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

4. 现在，我们根据GitHub的提示，在本地的`learngit`仓库下运行命令：

   ```shell
   git remote add origin git@github.com:kaigedong/learngit.git
   ```

> 添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

5. 下一步，就可以把本地库的所有内容推送到远程库上：

    ```
    $ git push -u origin masterCounting objects: 19, done.Delta compression using up to 4 threads.Compressing objects: 100% (19/19), done.Writing objects: 100% (19/19), 13.73 KiB, done.Total 23 (delta 6), reused 0 (delta 0)To git@github.com:michaelliao/learngit.git * [new branch]      master -&gt; masterBranch master set up to track remote branch master from origin.
       $ git push -u origin master
       Counting objects: 19, done.
       Delta compression using up to 4 threads.
       Compressing objects: 100% (19/19), done.
       Writing objects: 100% (19/19), 13.73 KiB, done.
       Total 23 (delta 6), reused 0 (delta 0)
       To git@github.com:michaelliao/learngit.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from origin.
    ```
    > 把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

6.  由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

7.  推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

8.  从现在起，只要本地作了提交，就可以通过命令：

   ```shell
   git push origin master
   ```

9. 把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

### 小结

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

