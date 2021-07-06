## Windows | Docker 部署mysql5 | 宿主机已安装mysql

1.拉取镜像

```dockerfile
docker pull mysql:5.7 	// 指定版本，不指定拉取最新版本
```

2.创建镜像

```dockerfile
docker run --name mysql5.7 -e MYSQL_ROOT_PASSWORD=123456 -p 3307:3306 -d mysql:5.7
// 各个参数的意义
--name 创建容器的名字
-e 设置mysql的root密码
-p 设置容器的映射端口，由于宿主机已经安装mysql，3306端口是被占用的
-d 表示后台运行
```

>**其中-p 映射端口，3307：3306，前者是宿主机端口，后者是容器端口，默认容器中mysql默认端口为3306，后者端口不设置3306会导致，宿主机无法连接到容器中mysql，除非修改容器mysql的默认端口。**

3.查看运行成功

![image-20210129115508545](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20210129115508545.png)

> **其中0.0.0.0:3307映射到容器中3306，没问题！**

3.设置mysql远程登陆

```mysql
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
mysql>FLUSH PRIVILEGES;
// 其中123456，可以修改为你的密码
```

4.查看docker的ip，并用navicat连接

```bash
D:/>ipconfig
以太网适配器 vEthernet (WSL):
   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::d1c5:cd1e:708a:dd93%102
   IPv4 地址 . . . . . . . . . . . . : 192.168.208.33
   子网掩码  . . . . . . . . . . . . : 255.255.255.240
   默认网关. . . . . . . . . . . . . :
```

5.使用navicat连接

![image-20210129120149840](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20210129120149840.png)

6.将宿主机拷贝到容器中

```bash
docker cp D:\internship\sql\jobbolearticle.sql mysql5.7:/
// cp 宿主机文件路径 容器name:容器中路径（上面就是根目录）
```

7.打开容器中mysql的命令行，并直接导入sql文件

```bash
docker exec -it mysql5.7 bash # 其中mysql5.7就是创建时容器名字

root@bc0324e7e75c:/# ls	# 可以看到根目录下有拷贝过来的文件
jobbolearticle.sql  proc  run   srv   tmp  var ...

root@bc0324e7e75c:/# mysql -u root -p
Enter password:		# 输入密码
mysql> create database articlespider;
mysql> use articlespider;
mysql> source /jobbolearticle.sql;
```