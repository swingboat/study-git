# stud-git

## 创建版本库

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

