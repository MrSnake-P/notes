[TOC]

## 购买阿里云服务器

* 操作系统centos8.2（方便之后安装宝塔linux）

*  快速入门服务器[官方文档](https://help.aliyun.com/document_detail/151695.html?spm=a2c4g.11186623.6.596.7c155b98JbfIIE)

## 安装宝塔Linux

* [官方文档](https://www.bt.cn/bbs/thread-19376-1-1.html)

1.开始安装

* 首先放行服务器8888端口

![image-20201211125602503](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20201211125602503.png)

注意：
如需完整使用宝塔的所有功能 你还需要放行如下端口
20 、21、 39000-40000端口（linux 系统 ），3000-4000（windows系统）
22 (SSH)
80、443（网站及SSL）
3306 （数据库远程连接）
888 （phpmyadmin）

2.挂载磁盘

```
yum install wget -y && wget -O auto_disk.sh http://download.bt.cn/tools/auto_disk.sh && bash auto_disk.sh
```

3.Centos安装命令

```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```

4.安装成功

* 访问ip:8888

![image-20201211131823809](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20201211131823809.png)

## 安装Typecho

* 下载最新版，访问 http://typecho.org/download 获得最新的稳定版本。

1.解压后生成build，将build中所有文件添加到BTLinux中网站管理根目录下

![image-20201211135246097](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20201211135246097.png)

2.访问`ip/install.php`

根据提示安装即可