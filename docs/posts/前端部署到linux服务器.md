---
title: 前端部署到linux服务器
date: 2021-07-13 20:43:15
permalink: /pages/df4ab5/
categories:
  - 项目部署
tags:
  - linux
  - nginx
  - vue
---

## 项目打包

1. 在当前项目目录下运行`npm run build`命令将项目打包，生成`dist`文件夹
2. 如果是第一次操作，需要在linux服务器的目录`/usr/local/nginx/html/`中新建放置该项目的文件夹

## 配置nginx

1. 打开`/usr/local/nginx/conf/nginx.conf`文件
2. 新增一个`server`，设置其端口号、域名、ssl证书、文件路径、代理等

```
server {
  listen       443 ssl; # 443 端口号    ssl 开启HTTPS
  server_name  www.example.com; # 域名
  ssl_certificate /usr/local/nginx/cert/www.example.com.pem;     # 证书文件
  ssl_certificate_key /usr/local/nginx/cert/www.example.com.key; # 证书文件
  charset utf-8;

  location / {
    root   /usr/local/nginx/html/projectName; # 文件路径
    try_files $uri $uri/ /index.html;
    index  index.html index.htm;
  }

  location /prod-api/ { # 代理地址
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass https://localhost:8080/;  # 后端接口
  }

  error_page   500 502 503 504  /50x.html;
  location = /50x.html {
    root   html;
  }
}
```

- 在使用ssl证书的情况下，访问地址会以`https`开头，不使用则是已`http`开头
- `https`的默认端口为`443`，如果同时配置`ssl证书`和`443`端口，访问网址为`https://www.example.com`
- `http`的默认端口为`80`，如果没有配置`ssl证书`且设置`80`为端口，访问网址为`http://www.example.com`
- 配置域名需要预先在域名所在平台进行域名解析：[阿里云域名解析](https://blog.csdn.net/can3981132/article/details/118051616)
- 配置ssl证书需要先申请证书：[阿里云ssl证书申请](https://blog.csdn.net/yunweifun/article/details/123409692)

## 启动服务

- nginx还未运行的情况下，在`/usr/local/nginx/sbin/`目录运行`./nginx`命令
- 如果配置文件`/usr/local/nginx/conf/nginx.conf`有修改，运行`./nginx -s reload`命令
- 如果只是更新了项目文件，则不需要进行操作，nginx会自动更新
- 如果在浏览器中访问发现没有更新，需要刷新几次或者清除缓存