[TOC]

## 克隆一个新仓库

`$ git clone --bare git my_images.git`

## 上传到服务器指定目录

```shell
git remote add origin
root@www.example.com:/www/wwwroot/mrsnake.top/images/
```

## 报错

`fatal: refusing to merge unrelated histories`

```
git pull origin master --allow-unrelated-historie
```

