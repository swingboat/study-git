# stud-git（note）

> https://www.liaoxuefeng.com/wiki/896043488029600

##创建版本库

```shell
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

> 在当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的。.git目录默认是隐藏的，用ls -ah命令就可以看见

### 把文件添加到版本库

```shell
$ git add readme.txt
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

> git commit命令成功后，1 file changed：1个文件被改动；2 insertion：插入了两行内容。

### 查看文件到状态

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

> 上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交修改。

### 查看修改的内容

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

提交后，查看文件的状态

```shell
$ git add readme.txt
$ git commit -m "add content"
$ git status
On branch master
nothing to commit, working tree clean
```

> Git告诉我们当前没有需要提交的修改，而且工作目录是干净的。

### 版本回退

当你觉得文件修改到一定程度的时候，就可以保存一个“快照”，这个快照在Git中被称为commit。一旦文件被改乱了，或者误删除了。可以从最近的一个commit恢复。

使用git log命令查看历史

```shell
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

> 如果嫌输出的信息太多，可以加上--pretty=oneline
>
> Git的commit id不是1，2，3...递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示

Git必须知道当前的版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当前往上100个版本写成HEAD-100

回归到上一个版本：

```shell
$ git reset --hard HEAD
HEAD is now at e475afc add distributed
```

回到指定的版本：

```shell
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

> 版本号没必要写全，前几位就可以了，Git会自动去找。

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──> ○ add distributed
        │
        ○ wrote a readme file
```

> Git在内部有个指向当前版本的HEAD指针，当回退版本的时候，Git仅仅是移动HEAD指针。

但查不到某个commit id的时候，可以使用git reflog命令。Git提供`git reflog`用来记录每一次命令。

---

* `HEAD`指向的版本就是当前版本，Git允许我们在版本的历史之间穿梭：`git reset --hard commit_id`
* 穿梭前，用`git log`查看提交历史，以便确定回退到哪个版本
* 要重返未来，用`git reflog`查看历史命令，以便确定回到未来的哪个版本

### 工作区和暂存区

![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

* 第一步用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区
* 第二步用`git commit`提交更改，实际上就是把暂存区所有内容提交到当前分支

### 管理修改

Git跟踪并管理的是修改，而不是文件。

查看工作区和版本库里面最新版本的区别：

```shell
$ git diff HEAD -- readme.txt 
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
```

每次修改，如果不用`git add`到暂存区，那就不会加入到commit中。

### 撤销修改

命令`git checkout -- readme.txt`意思就是，把readme.txt文件在工作区的修改全部撤销，让文件回到最近一次`git commit`或`git add`时的状态。

用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区

### 删除文件

```shell
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

### 远程仓库

关联一个远程库，使用命令：`git remote add origin git@server-name:path/repo-name.git`

关联后，使用命令：`git push -u origin master`推送master分支所有内容

##分支管理

###创建与合并

HEAD严格来说不是指向提交，而是指向master（当前的分支），master才是指向提交的。

![git-br-initial](https://www.liaoxuefeng.com/files/attachments/919022325462368/0)

Examples：

![git-br-on-master](https://www.liaoxuefeng.com/files/attachments/919022533080576/0)

把dev分支的工作合并到master上：

```shell
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定的分支到当前分支。`Fast-forward`：说明这次合并是“快进模式”，也就是直接把master指向dev的当前分支。注意：并不少每次合并都能Fast-forward。

> Branch相关的命令：
>
> 1. 创建并切换到新的分支dev
>
> `git checkout -b dev` or `git switch -c dev`
>
> 2. 切换已经存在的分支dev
> `git checkout dev` or `git switch dev`
>
> 3. 查看分支
> `git branch`
>
> 4. 创建分支div
> `git branch dev ` `
>
> 5. 合并分支dev到当前分支
>
> `git merge dev`
>
> 6. 删除分支dev
> `git branch -d dev`
>
> 7. 强行删除分支dev
>
>   `git branch -D dev` 强行删除，需要使用大写的`-D`参数
>
> 8. 终止merge
>   `git merge --abort`

### 分支管理策略

合并分支时，git会用Fast-forward模式，但这种模式下，删除分支后，会丢掉分支信息。

禁用Fast-forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```shell
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

> 因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

![git-no-ff-mode](https://www.liaoxuefeng.com/files/attachments/919023225142304/0)

合并后，用`got log`查看分支历史：

```shell
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```

### Bug分支

每个bug通过一个临时分支来修复，修复后，合并分支，最后将临时分支删除。但当前在dev分支进行开发，还有很多的工作没有提交。但又不能马上提交，怎么处理？

Git提供了`stash`功能，可以把当前工作现场‘储藏’起来，等以后恢复现场后继续工作：

```shell
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

现在，用`git status`查看工作区，就是干净的

用`git stash list`命令查看当前保存的stash

```shell
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

恢复工作现场，两种办法：

1. 用`git stash apply`恢复，但是恢复后，stash内容并不删除，用`git stash drop`来删除
2. 用`git stash pop`，恢复的同时把stash内容也删除了。

可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash：

```she
$ git stash apply stash@{0}
```

同样的bug，也需要在dev上修复。我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。

Git专门提供了`cherry-pick`命令，能复制一个特定的提交到当前分支：

```shell
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

### Rebase

显示git的commit记录：

```shell
$ git log --graph --pretty=oneline --abbrev-commit
*   e0ea545 (HEAD -> master) Merge branch 'master' of github.com:michaelliao/learngit
|\  
| * f005ed4 (origin/master) set exit=1
* | 582d922 add author
* | 8875536 add comment
|/  
* d1be385 init hello
...
```

问题：提交的历史交叉了，但可以push到远处。

使用`git rebase`命令，可以把原本分叉的提交变成一条直线。rebase操作前后，最终的提交内容是一致的，但本地commit修改内容已经变化了（主要是基于哪个修改发生变化了）。	

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 标签管理

发布一个版本时，在版本库中打一个标签（Tag），确定了打标签时刻的版本。标签是版本库的一个快照，但它是指向某个commit的指针（跟分支很像，但分支可以移动，标签不能移动），所以创建和删除标签都是瞬间的。

```shell
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
$ git tag v1.0
$ git tag
v1.0
```

首先需要切换到需要打标签的分支上，使用命令`git tag <name>`打一个新的标签。使用`git tag`查看所有标签。默认标签是打在最新提交的commit上的。

使用下面的命令打在指定的commit上

```shell
$ git tag v0.9 f52c633
```

创建带有说明的标签：

```shell
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

> `-a`指定标签名，`-m`指定说明文字

使用下面的命令查看标签信息：

```shell
$ git show <tagName>
```

删除一个标签

```shell
$ git tag -d v0.1
Deleted tag 'v0.1' (was f15b0dd)
```

推送某个标签到远程

```shell
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v1.0 -> v1.0
```

一次性推送全部尚未推送到本地标签

```shell
$ git push origin --tags
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v0.9 -> v0.9
```

删除标签(先删除本地标签，然后再推送到远程)

```shell
$ git tag -d v0.9
Deleted tag 'v0.9' (was f52c633)
$ git push origin :refs/tags/v0.9
To github.com:michaelliao/learngit.git
 - [deleted]         v0.9
```

