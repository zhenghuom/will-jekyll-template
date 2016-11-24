---
layout: post
title: "初学python"
date: 2016-10-22 18:00:00
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

推荐一个学习django的学习地址 http://www.ziqiangxuetang.com/django/django-tutorial.html

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









