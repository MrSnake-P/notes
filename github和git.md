[TOC]



## 生成 SSH 公钥

* windows 输入指令

`$ ssh-keygen -o`

```
$ ssh-keygen -o
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\91372/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):		# 使用密钥时输入口令，不需要留空
Enter same passphrase again:	# 再次输入口令
Your identification has been saved in C:\Users\91372/.ssh/id_rsa.
Your public key has been saved in C:\Users\91372/.ssh/id_rsa.pub.
```

* 复制 `/.ssh/` 目录下 `.pub` 文件

`ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABURAr2+5SsN84hLTLLM+JeQg6tKdvqFPZcwgUWO1QaHG+/oLe6WU9apOAltuYmu4JilkUAkG81+YIHorrtnPLOopxXGd75hDZUCMxM7Q0+WDL/jBf1cId74/JNfGobbTjMJMR5u9GZePgb5pxbeeC···`

## Github 添加公钥

* 点击右上角头像选择设置

  ![image-20201222132650284](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20201222132650284.png)

* 选择 SSH and GPG keys，添加公钥

  ![image-20201222132747689](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20201222132747689.png)

* 取一个容易记忆的名字，方便以后管理，将上述的公钥复制进去

  ![image-20201222132947369](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20201222132947369.png)

  ![image-20201222133234854](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20201222133234854.png)

## 开启两部验证，加强安全性

* 选择 Account security

![image-20201222135428167](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20201222135428167.png)

* 选择 two-factor authentication

  ![image-20201222135521345](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20201222135521345.png)

* 根据提示操作

## 在 Github 上关联本地 Git

```
echo "# example" >> README.md
git init
git add README.md
git commit -m "first commit"	# 根据信息提交
git branch -M main	# 创建一个分支
git remote add origin git@github.com:MrSnake-P/example.git	# 关联远程库
git push -u origin main	# 将全部文件推送到远端main分支上
# 之后在本地提交后,再次输入指令，推送最新的更改
```

### 删除远程关联

`git remote rm orgin`

