[TOC]

## MySQL 插入时间不正确

* 查看目前时间

  `select now()`

* 查看时区

  ` show variables like '%time_zone%';`

* 设置时区

  `set time_zone='+8:00';`

时区为cst

## MySQL 设置远程登陆

`sudo mysql_secure_installation`

```sql
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
mysql>FLUSH PRIVILEGES;
```

## MySQL 忘记密码

* 输入

  `sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf`

* 在 skip-external-locking 下面添加

  `skip-grant-tables`

* 命令行输入mysql，use mysql

  ```
  UPDATE mysql.user SET authentication_string=password('你想设置的密码') 
  WHERE User='root' AND Host ='localhost';
  ```

  

* 修改用户认证方式

  `UPDATE user SET plugin="mysql_native_password";`

* 立即执行

  `flush privileges;`

* 回到配置文件注释 `skip-grant-tables`

## MySQL 导入导出时编码错误 1273

* 打开sql文件

  **`utf8mb4_0900_ai_ci` -> `utf8_general_ci`**

  **`utf8mb4` -> `utf8`**

## 移动 MySQL 到指定位置

```python
1.移动
mv /var/lib/mysql /home/hp/diskC/
2.修改
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
datadir = /home/hp/diskC/mysql
3.配置
sudo vim /etc/apparmor.d/tunables/alias
最后一行添加
alias /var/lib/mysql/ -> /home/hp/diskC/mysql/,
4.重启
sudo service apparmor restart
sudo /etc/init.d/mysql start
5.查看
mysql> select @@datadir
    -> ;
+-----------------------+
| @@datadir             |
+-----------------------+
| /home/hp/diskC/mysql/ |
+-----------------------+
1 row in set (0.00 sec)
```

## 修改密码

```
set password for username @localhost = password(newpwd);
# 修改本地密码
update mysql.user set authentication_string=password('123456') where user = 'root';
# 修改远程登陆密码
flush privileges	# 立即执行
```

## 开启log_bin

```mysql
nano /etc/mysql/mysql.conf.d/mysqld.cnf
server-id               = 12345
log-bin                 = /var/log/mysql/mysql-bin.log


mysql> show variables like "%log_bin%"
+---------------------------------+--------------------------------+
| Variable_name                   | Value                          |
+---------------------------------+--------------------------------+
| log_bin                         | ON                             |
| log_bin_basename                | /var/log/mysql/mysql-bin       |
| log_bin_index                   | /var/log/mysql/mysql-bin.index |
| log_bin_trust_function_creators | OFF                            |
| log_bin_use_v1_row_events       | OFF                            |
| sql_log_bin                     | ON                             |
+---------------------------------+--------------------------------+
```

## 查看密码认证

```mysql
use mysql;
select user,plugin from user where user='root';
```

## 卸载命令

`sudo apt autoremove`

## Redis Linux环境下的安装

* 下载

  `wget https://download.redis.io/releases/redis-6.0.9.tar.gz`

* 解压

  `tar -zvxf redis-6.0.9.tar.gz`

* 移动

  `mv ./redis-6.0.9 /usr/local/redis`

* 编译

  `make`

* 安装

  `make PREFIX=/usr/local/redis install`

* 启动

  `./bin/redis-server& ./redis.conf`

* 客服端

  `redis-cli.exe -h 127.0.0.1 -p 6379`

* 设置密码

  `config set requirepass qwe`

  * 查看密码

    `config get requirepass`

  

* 验证

  `auth 密码`

* 删除密码

  `config set requirepass ''`

### 常用操作

* 存放键值对并设置TTL

  `SET KEY VALUE EX=3` ex为TTL，表示3秒后删除这条数据

* 批量存放键值对

  `mset key1 1 key2 2 key3 3`

* 查看所有键

  `keys *`

* 查看所有配置

  `config get *`

* 绑定主机地址

  `bind *`

* 判断是否存在

  exists key

* 配置文件地址

  `vim /usr/local/redis/redis.conf`

  * 守护进程 daemonize
  * 保护模式 protected-mode

### 确认 Redis 是否在运行

1. 查看进程

   `ps -aux | grep redis`

2. 查看端口

   `netstat -lanp | grep 6379`