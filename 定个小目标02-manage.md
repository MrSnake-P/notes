[TOC]

## 创建一个取图片的站点

    # nginx.conf.default 路径:/www/server/nginx/conf
    server {
        listen       80;
        server_name  localhost;
    
        #charset koi8-r;
    
        #access_log  logs/host.access.log  main;
    
        location / {
            root   html;
            index  index.html index.htm;
        }
添加

    location /目录1/ {	
    # 其中目录1为网站下的根目录，我的就是mrsnake.top下创建的一个文件夹
    	root   /www/wwwroot/mrsnake.top;	# 网站目录
    	autoindex on;	# 开启浏览功能
    }
    location /目录2/ {
        root   /WWW/wwwroot/mrsnake.top;
        autoindex on;
    }
## 创建页面左边的小图标

```
<link rel="icon" type="image/icon" href="<?php $this->options->themeUrl('/favicon3.ico'); ?>" />
```

