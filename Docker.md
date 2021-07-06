### windows下修改默认安装位置

```bash
mklink /j "C:\Program Files\Docker" "D:\software\Docker"
```

### 修改镜像源，修改Docker Engine

```bash
{
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
  ],
  "insecure-registries": [],
  "debug": true,
  "experimental": false
}
```

### Windows | 修改默认的镜像安装路径

* 查看运行状态

  `wsl --list --verbose`

  ```bash
  D:\>wsl --list --verbose
    NAME                   STATE           VERSION
  * docker-desktop         Running         2
    docker-desktop-data    Running         2
  ```

* 默认镜像存放路径

  `C:\Users\Administrator\AppData\Local\Docker\wsl`中的data和distro

1.删除所有容器

2.退出Docker Desktop

3.关闭服务

`wsl --shutdown`

4.检查所有服务关闭

`wsl --list --verbose`

5.备份image数据

`D:\>wsl --export docker-desktop-data D:\AppData\Docker\docker-desktop-data.tar`

6.注销当前docker-desktop-data发行版

`wsl --unregister docker-desktop-data`

7.重新导入备份，并指定新的路径

```bash
wsl --import docker-desktop-data D:\AppData\Docker\data D:\AppData\Docker\docker-desktop-data.tar --version 2
第一个参数：发行版本docker-desktop-data
第二个参数：新的镜像安装路径
第三个参数：备份的路径
```

8.重新运行docker-desktop