[TOC]



## Git 分支 - 分支简介

​	Git 保存的不是文件的变化或者差异，而是一系列不同时刻的 **快照** 。

​	首次提交产生的提交对象没有父对象，普通提交操作产生的提交对象有一个父对象， 而由多个分支合并产生的提交对象有多个父对象。

​	Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 `master`。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 `master` 分支。 `master` 分支**会在每次提交时自动向前移动**。

![image-20210106103411523](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20210106103411523.png)

## 分支创建

* 创建 testing 分支

  `$ git branch testing `	# 在当前所在的提交对象上创建一个新分支

* 两个指向相同提交历史的分支

  ![image-20210106104046873](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20210106104046873.png)

  **git 通过 `HEAD` 这个特殊指针，区分指向当前所在的本地分支。而不会因为 `git branch` 创建的新分支，自动切换到新分支。**

------

**Note | HEAD 指针会不断跟随分支更新移动而移动**

------

* 通过 `git` log 查看 `HEAD` 指向当前所在的分支 使用 `--decorate` 参数

```bash
$ git log --oneline --decorate
4a939da (HEAD -> main1, tag: v1.1, tag: v1.0-snake, origin/main, main) second
6f7cdb5 first commit
```

## 分支切换

* 切换到 testing 分支

  `$ git checkout testing`

* `HEAD` 的移动

  当我们切换到一个旧的分支，并修改提交后，会产生分支，以便于另一个方向的开发

  ```bash
  $ git checkout master	# 切换到master
  $ echo 'snake' > readme.txt
  $ git commit -a -m 'made other changes'
  [master 60e50e3] made other changes
   1 file changed, 1 insertion(+)
   create mode 100644 readme.txt
  ```

  ![image-20210106105631870](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20210106105631870.png)

* 查看项目分支历史

  `$ git log --oneline --decorate --graph --all`

  会输出提交历史、各个分支以及项目的分支分叉情况

  ```bash
  * 5188a13 (HEAD -> master) made some changes
  | * 5f9473f (testing) made a change
  | * 60e50e3 made other changes
  |/
  * f60211e (origin/master) code
  * 4a939da (tag: v1.1, tag: v1.0-snake, origin/main, main1, main) second
  * 6f7cdb5 first commit
  ```

------

**Note |  创建新分支同时切换过去 `git checkout -b <newbranchname>`** 

------

## Git 分支 - 分支的新建与合并

* 合并分支

  1. 首先在master分支上创建一个新分支并切换

     ```bash
     snake@DESKTOP MINGW64 /d (master)
     $ git checkout -b hotfix
     Switched to a new branch 'hotfix'
     $ vscode index.html
     $ git add index.html
     $ git commit -a -m 'hotfix'
     [hotfix 8a1fb58] hotfix
      1 file changed, 1 insertion(+)
      create mode 100644 index.html
     ```

  2. 切换到 master，进行合并

     ```bash
     snake@DESKTOP MINGW64 /d (hotfix)
     $ git checkout master
     $ git merge hotfix
     Updating 5188a13..8a1fb58
     Fast-forward
      index.html | 1 +
      1 file changed, 1 insertion(+)
      create mode 100644 index.html
     ```

------

​	合并两个分支时， 如果顺着一个分支走下去能够到达另一个分支，那么 Git 在合并两者的时候， 只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧——这就叫做 “快进（fast-forward）。

------

此时可以删除 hotfix 分支，因为先 master 和 hotfix 指向同一个位置

<img src="C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20210106114621307.png" alt="image-20210106114621307" style="zoom: 50%;" /> **合并前后**<img src="C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20210106114656458.png" alt="image-20210106114656458" style="zoom:50%;" />

```bash
$ git branch -d hotfix
Deleted branch hotfix (was 8a1fb58).
```

### [遇到冲突时的分支合并](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)