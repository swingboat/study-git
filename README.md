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

