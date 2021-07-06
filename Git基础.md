[TOC]

## Git 基础 - 获取 Git 仓库

* 在一个目录中初始化仓库

  1. 进入一个目录 `cd D:\git`

  2. 执行 `$ git init` 生成 `.git `

     Initialized empty Git repository in D:/git/.git/

## Git 克隆仓库

`git clone url`

## Git 操作

### 1.检查当前文件状态

`$ git status`

### 2.创建一个新的未跟踪文件

`$ echo 'Mr Snake' > README`

```
$ git status
On branch master	# 默认分支
Your branch is up to date with 'origin/master'.
Untracked files:	# 生成一个未跟踪的文件
  (use "git add <file>..." to include in what will be committed)
        README
nothing added to commit but untracked files present (use "git add" to track)
```

### 3.跟踪文件

精确地将内容添加到下一次提交中

1. `$ git add README`	# 已暂存状态

```
On branch master
Your branch is up to date with 'origin/master'.
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README
```

2. `$ git add 目录`	#  加目录就是跟踪目录下所有文件

3. `$ git add 文件 `    #  暂存已修改的文件

   ```
   On branch master
   Your branch is up to date with 'origin/master'.
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           new file:   readme
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           mrsnake.txt
   ```

   `git add mrsnake.txt` 

   `git add *`	# 暂存所有文件

4. `git status -s`  # 缩短显示的命令

   ```
   # git status -s
    M README		# 修改了但未暂存
   MM Rakefile		# 修改了暂存后又作了修改
   A  lib/git.rb	# 新添加到暂存区
   M  lib/simplegit.rb	# 修改且暂存
   ?? LICENSE.txt	# 
   ```

5. | 标记 | 解释                               |
   | ---- | ---------------------------------- |
   | ？？ | 未跟踪文件                         |
   | A    | 新添加到暂存区中的文件             |
   | M    | 修改过的文件                       |
   | MM   | 左边暂存区的状态，右边工作区的状态 |

### 4.忽略文件

有些文件不需要文件纳入 Git 的管理，忽略他们。创建`.gitignore`

```
$ cat .gitignore	# windows可以直接编辑
*.[oa]	# 忽略所有以.o和.a结尾的文件
*~		# 忽略~结尾的文件
```

```
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

### 5.查看已暂存和未暂存的修改

* 查看尚未暂存的文件差异

  `git diff`

* 查看已暂存的文件差异

  `git diff --cached`

  ```
  $ git diff --cached
  diff --git a/.gitignore b/.gitignore
  new file mode 100644
  index 0000000..cf708a1
  --- /dev/null
  +++ b/.gitignore
  @@ -0,0 +1 @@
  +"My Project"
  diff --git a/mrsnake.md b/mrsnake.md
  new file mode 100644
  index 0000000..e69de29
  ```

### 6.提交更新

`git commit`

`git commit -m "提交信息"` # 根据输入的提交说明生成一次提交

```
$ git commit -m "提交信息"
[master (root-commit) 254cc80] 提交信息
 3 files changed, 2 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 README
 create mode 100644 mrsnake.md
```

* 跳过使用暂存区域

  `git commit -a`	# 可以提交已经跟踪过的文件，而不一定是暂存区的文件

### 7.移除文件

* 移除，不在纳入版本管理

  `git rm mrsnake.md`

* 移除已经修改过或已经放到暂存区的文件

  `git rm -f xxx`

* 保留文件，并且停止追踪

  `git rm --cached README`

### 8.移动文件

* 更名 `$ git mv README readme`

  ```
  $ git mv README readme
  $ git status
  On branch master
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
          renamed:    README -> readme
  ```

## Git 基础 - 查看提交历史

1. 查看提交历史 `$ git log`

   ```
   $ git log
   commit 254cc800a95ba720dfe7fb09165e5a3188bf4bde (HEAD -> master)
   Author: Mr Snake <jjpython@163.com>
   Date:   Mon Dec 21 18:41:29 2020 +0800
   
       提交信息
   ```

2. 以补丁的形式查看 `git log -p -1`

   ```
   $ git log -p -1	# -1限定最近的一次提交
   commit 254cc800a95ba720dfe7fb09165e5a3188bf4bde (HEAD -> master)
   Author: Mr Snake <jjpython@163.com>
   Date:   Mon Dec 21 18:41:29 2020 +0800
       提交信息
       
   diff --git a/.gitignore b/.gitignore
   new file mode 100644
   index 0000000..cf708a1
   --- /dev/null
   +++ b/.gitignore
   @@ -0,0 +1 @@
   +"My Project"
   diff --git a/mrsnake.md b/mrsnake.md
   new file mode 100644
   index 0000000..e69de29
   ```

3. 每次提交的简略统计信息 `$ git log --stat`

   ```
   $ git log --stat
   commit 254cc800a95ba720dfe7fb09165e5a3188bf4bde (HEAD -> master)
   Author: Mr Snake <jjpython@163.com>
   Date:   Mon Dec 21 18:41:29 2020 +0800
   
       提交信息
   
    .gitignore | 1 +
    README     | 1 +
    mrsnake.md | 0
    3 files changed, 2 insertions(+)
   ```

## Git 基础 - 撤销操作

当提交完发现漏掉几个文件没有添加，或者提交信息写错了。

* 撤销操作 `$ git commit --amend`

  ```
  $ git commit -m 'initial commit'	# 提交完发现漏掉文件
  $ git add forgotten_file		# 跟踪漏掉的文件
  $ git commit --amend			# 重新提交
  ```

  **note：原理上并不是通过用改进后的提交“原位替换”旧的提交方式，效果上来说是这样。**

* 取消暂存的文件 `git reset HEAD file`

  ```
  git add *	# 误操作暂存了不需要的文件
  git reset HEAD CONTRIBUTING.md
  ```

* 撤销对文件的修改 `git checkout -- file`

  `git checkout -- CONTRIBUTING.md`

  **note：这会使那个文件在本地的任何修改都会消失，Git 会用最近提交的版本覆盖掉它。**

## Git 基础 - 远程仓库的使用

### 1.远程仓库的使用

查看远程仓库 URL

`$ git remote -v`

### 2.添加远程仓库

`$ git remote add <shortname> <url>`	# 可以设置一个简写名

### 3.可以使用简写名代替整个 URL

`$ git fetch shortname`	# fetch从远程仓库中获得数据

### 4.推送到远程仓库

`$ git push <remote> <branch>`	# 将master分支推送到origin服务器

**note：使用 `git clone` 获得到仓库自动会添加 origin 和 master 分支**

### 5.查看某个远程仓库

`$ git remote show orgin`

```
$ git remote show orgin
* remote origin
  Fetch URL: git@github.com:MrSnake-P/BLOG.git
  Push  URL: git@github.com:MrSnake-P/BLOG.git
  HEAD branch: master
  Remote branches:
    main   tracked
    master tracked
  Local branch configured for 'git pull':	
  # git pull抓取所有的远程引用，并且将远程分支master合并到本地分支
  # git pull --rebase 强制生成文件
 master
    main merges with remote master
  Local ref configured for 'git push':
  # git push 将本地推送到远程仓库
    main pushes to main (up to date)
```

### 6.远程仓库的重命名与移除

`$ git remote rename name newname`

**note：这也会修改所有远程的分支名字，即 name/master 变成 newname/master**

`$ git remote remove name or git remote rm name`

**note：这会删除所有与这个仓库有关的远程跟踪分支以及配置信息**

## Git 基础 - 打标签

使用这个功能来标记发布节点（v1.0、v2.0等等）

### 1.列出标签

* 以字母顺序列出表标签

  `$ git tag`

*  按照特定模式查找表标签

  `$ git tag -l "v1.8.5"` 	# 只对1.,8.5系列

### 2. 创建标签

* 轻量标签

  类似一个不会改变的的分支——只是某个特定提交的引用。

  `$ git tag v1.0-snake`	# 只需要提供标签名字，并且不会有额外的信息

* 附注标签

  存储在 Git 数据库中的一个完整对象，能被校验。包含：打标签者的名字、电子邮件地址、日期时间，另又一个标签信息，能使用 GNU Privacy Guard（GPG）签名并验证。

  `$ git tag -a v1.1 -m "my version 1.1"`

  ```
  $ git tag
  v1.1
  
  $ git show v1.1		# 查看标签信息和与之对相应的提交信息
  tag v1.1
  Tagger: Mr Snake <jjpython@163.com>
  Date:   Thu Dec 24 10:58:01 2020 +0800
  
  my version 1.1
  
  commit 4a939dae325cb670f3e37 (HEAD -> main, tag: v1.1, origin/master, origin/main)
  Author: Mr Snake <jjpython@163.com>
  Date:   Tue Dec 22 17:06:59 2020 +0800
  
  ```

### 3.后期打标签

```
$ git log --pretty=oneline
4a939dae325cb670880786925a865ffefb7f3e37 (HEAD -> main, tag: v1.1, tag: v1.0-snake, origin/master, origin/main) second
6f7cdb5df3be04de6a86143b8b5db67168be0236 first commit
```

提交之后忘记打标签，在命令的末尾指定提交的校验和（或部分校验和）

`git tag -a v1.2 -m "patch tag" 4a939da`

### 4.共享标签

默认情况下，`git push` 命令不会将标签传送到远程服务器，创建完标签必须显示地推送标签到共享服务器上

* 类似共享远程分支一样，执行 `git push origin v1.1`

  ```bash
  $ git push origin v1.1
  Enter passphrase for key '/c/Users/91372/.ssh/id_rsa':
  Enter passphrase for key '/c/Users/91372/.ssh/id_rsa':
  Enumerating objects: 1, done.
  Counting objects: 100% (1/1), done.
  Writing objects: 100% (1/1), 163 bytes | 163.00 KiB/s, done.
  Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
  To github.com:MrSnake-P/BLOG.git
   * [new tag]         v1.1 -> v1.1
  ```

* 一次性推送很多标签，使用带有 `--tags` 选项的 `git push` 命令

  ```bash
  $ git push origin --tags
  Enter passphrase for key '/c/Users/91372/.ssh/id_rsa':
  Enumerating objects: 1, done.
  Counting objects: 100% (1/1), done.
  Writing objects: 100% (1/1), 159 bytes | 159.00 KiB/s, done.
  Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
  To github.com:MrSnake-P/BLOG.git
   * [new tag]         v1.0-snake -> v1.0-snake
   * [new tag]         v1.0-snake2 -> v1.0-snake2
  
  ```

### 5.删除标签

删除本地仓库上的标签，`git tag -d <tagname>`

```
$ git tag -d v1.0-snake2
Deleted tag 'v1.0-snake2' (was c1285d6)
# 这么做并不会删除远端仓库的标签
```

从远程仓库移除这个标签，更新远程仓库

* 第一种方法

  ```
  git push origin :refs/tags/v1.0-snake2	# 将冒号前面的空值推送到远程标签名，从而高效地删除它
  To github.com:MrSnake-P/BLOG.git
   - [deleted]         v1.0-snake2
  ```

* 第二种方法（推荐)

  `git push origin --delete <tagname>`

### 6.检出标签

想查看某个标签所指向的文件版本

`git checkout`

```bash
$ git checkout v1.1
Note: switching to 'v1.1'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 4a939da second
```

此时处于分离头指针（detached HEAD)需要进行修复，不然仅通过哈希访问

```
$ git checkout -b main1 v1.1
Switched to a new branch 'main1'
```

## Git基础-别名

* 自定义命令行

  ```bash
  $ git config --global alias.co checkout
  # git checkout 等价 git co
  $ git config --global alias.br branch
  $ git config --global alias.ci commit 
  $ git config --global alias.st status
  ```

* 自定义取消暂存 `git config --global alias.unstage 'reset HEAD --'`

  ```bash
  $ git unstage fileA
  $ git reset HEAD -- fileA	# 这两条是等价的
  ```

* 获取最后一条提交信息 `git config --global alias.last 'log -1 HEAD'`

  ```bash
  $ git last
  commit 4a939dae325cb670880786925a865ffefb7f3e37 (HEAD -> main1, tag: v1.1, tag: v1.0-snake, origin/main, main)
  Author: Mr Snake <jjpython@163.com>
  Date:   Tue Dec 22 17:06:59 2020 +0800
  
      second
  ```

* 自定义外部命令，在命令前面加入 `!` 符号

  将 `git visual` 定义为 `gitk` 的别名

  `$ git config --global alias.visual '!gitk'`

## GIT走代理

```git
git config --global https.proxy http://127.0.0.1:1080 && git config --global https.proxy https://127.0.0.1:1080
```

取消代理

```git
git config --global --unset http.proxy && git config --global --unset https.proxy
```

