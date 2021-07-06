---
[git官方文档](https://git-scm.com/doc)
---

[TOC]

## Git 分布式版本控制系统

* Git 不仅提取最新版本的文件快照，还把代码仓库完整地镜像下来，包括完整的历史记录。
* 能指定和若干不同的远端代码仓库进行交互。
* 在同一个项目中，和不同的小组相互协作。

## Git 的三种状态

* 已提交（committed）表示修改了文件，但还没保存到数据库中。
* 已修改（modified）表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
* 已暂存（staged）表示数据已经安全地保存在本地数据库中。

### 工作流程

1. 在工作区修改文件。
2. 将下次想要提交的更改选择性地暂存，只会将更改的部分添加到暂存区。
3. 提交更新，找到暂存区的文件，将快照永久性存储到Git目录。

## 初始化 Git

* 设置用户名和邮箱

  ```
  > git config --global user.name "Mr Snake"
  > git config --global user.email jjpython@163.com
  ```

* 查看全局配置

  ```
  > git config --list
  diff.astextplain.textconv=astextplain
  filter.lfs.clean=git-lfs clean -- %f
  ···
  user.name=Mr Snake
  user.email=jjpython@163.com
  ```

* 查看配置文件的路径

  `git config --list --show-origin`

* 查看指定的配置

  ```
  > git config user.name
  Mr Snake
  ```

* 配置文本编辑器（vscode）

  `git config --global core.editor core-insiders`	# 需要配置 vscode 的环境变量

## 获取帮助

```
1.git help		# 全面版
2.git help config	# git config帮助
3.git add -h	# 简洁版
```

