---
layout: post
title: "初学python"
date: 2016-11-23 18:00:00
image: '/assets/img/'
description: '初学python遇到的问题'
tags:
- jekyll 
- template 
categories:
- I love Jekyll
twitter_text: 'How to install and use this template'
---

推荐一个学习python的学习地址  http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000

#配置环境  ubuntu  python3

安装uwsgi   sudo apt-get install uwsgi

安装pip3  sudo apt-get install python-pip

安装django  sudo apt-get install python-django -y

#创建django项目
1. 新建一个 django project
    django-admin.py startproject project-name
2. 新建 app
    python manage.py startapp app-name
或 django-admin.py startapp app-name

推荐一个学习django的学习地址 http://djangobook.py3k.cn/ 

##开发服务器

Django 带有一个内建的轻量级 Web 服务器，可供站点开发过程中使用。
我们提供这个服务器是为了让你快速开发站点，也就是说在准备发布产品之前，
无需进行产品级 Web 服务器（比如 Apache）的配置工作。该开发服务器会
监测代码变动并将其自动重载，这样一来，你可快速进行项目修改而无需作任何重启。

如果还没有进入 project-name 目录的话，现在进入其中，并运行 python manage.py runserver 命令。你将看到如下输出：

```javascript
Performing system checks...

System check identified no issues (0 silenced).
December 09, 2016 - 08:41:55
Django version 1.10.3, using settings 'hello.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

尽管对于开发来说，这个开发服务器非常得棒，但一定要打消在产品级环境中使用该
服务器的念头。在同一时间，该服务器只能可靠地处理一次单个请求，并且没有进行
任何类型的安全审计。发布站点前，请参阅第 20 章了解如何部署 Django 。

更改主机或端口

默认情况下， runserver 命令在 8000 端口启动开发服务器，且只监听本机连接。
要想要更改服务器端口的话，可将端口作为命令行参数传入：
```javascrpt
python manage.py runserver 8080
```

还可以改变服务器监听的 IP 地址。要和其他开发人员共享同一开发站点的话，
该功能特别有用。下面的命令：
```javascript
python manage.py runserver 0.0.0.0:8080
```

#使用uwsgi
在项目目录下创建uwsgi配置文件ini 如：uwsgi.ini
uwsgi.ini参数

```javascript
[uwsgi]
chdir=/home/zhm/www/python/hello
uid=nobody
gid=nobody
module=hello.wsgi:application
socket=/home/zhm/www/python/hello/uwsgi.sock
master=true
workers=5
pidfile=/home/zhm/www/python/hello/uwsgi.pid
vacuum=true
thunder-lock=true
enable-threads=true
harakiri=30
post-buffering=4096
daemonize=/home/zhm/www/python/hello/uwsgi.log
```

运行uwsgi.ini（我在uwsgi.ini所在的目录下运行）
uwsgi --ini uwsgi.ini

停止
uwsgi --stop uwsgi.pid

重启
uwsgi --reload uwsgi.pid

运行uwsgi.ini后会生成uwsgi.pid,uwsgi.sock,uwsgi.log文件

#nginx配置
```javascript
server {
    listen 8080;
    location / {
        include uwsgi_params;
        uwsgi_connect_timeout 30;
        uwsgi_pass unix:/home/zhm/www/python/hello/uwsgi.sock;
    }
}

```



#开始一个小项目
创建项目
django-admin.py startproject hello

创建APP
django-admin.py startapp test

##配置
在hello/settings.py内配置可允许访问的host

```python
ALLOWED_HOSTS = [
    '*'
]
```

APPS

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'test',
]
```

在hello/urls.py配置url

```python
from test import views as learn_views

urlpatterns = [
    url(r'^$', learn_views.index),
    url(r'^test/$', learn_views.index),
    url(r'^admin/', admin.site.urls),

]
```









