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

推荐一个学习django的学习地址 http://djangobook.py3k.cn/2.0

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
任何类型的安全审计。

更改主机或端口

默认情况下， runserver 命令在 8000 端口启动开发服务器，且只监听本机连接。
要想要更改服务器端口的话，可将端口作为命令行参数传入：

```javascript

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

django-admin.py startapp books

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
在setting.py配置数据

```python
#数据库
DATABASE_ENGINE = 'django.db.backends.mysql'
DATABASE_NAME = 'python'
DATABASE_USER = 'root'
DATABASE_PASSWORD = 'root'
DATABASE_HOST = 'localhost'
DATABASE_PORT = '3306'
```

#在Django中使用数据库遇到的问题

###第一个模型
第一步是用Python代码来描述它们。 打开由`` startapp`` 命令创建的models.py 并输入下面的内容

```python

from django.db import models

# Create your models here.

class Publisher(models.Model):
    name = models.CharField(max_length=30)
    address = models.CharField(max_length=50)
    city = models.CharField(max_length=60)
    state_province = models.CharField(max_length=30)
    country = models.CharField(max_length=50)
    website = models.URLField()

class Author(models.Model):
    salutation = models.CharField(max_length=10)
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=40)
    email = models.EmailField();
    headshot = models.ImageField(upload_to='/tmp')

class Book(models.Model):
    title = models.CharField(max_length=100)
    authors = models.ManyToManyField(Author)
    publisher = models.ForeignKey(Publisher)
    publication_date = models.DateField()
    
```

编辑 settings.py 文件

MIDDLEWARE_CLASSES =>MIDDLEWARE
```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.locale.LocaleMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    'books',
)
```


你在验证你的model所写的字段时，可能会执行python3 manage.py validate ，
然而你可能会看到提示Unknown command:‘validate‘Type ‘manage.py help‘ 
for usage.，因为这个命令比较旧，现在不用了，所以你要用如下这个命令：
python3 manage.py check来验证
--提示，你使用该命令时，可以会出现下面错误

```python
RuntimeError: Model class django.contrib.contenttypes.models.
ContentType doesn't declare an explicit app_label and isn't 
in an application in INSTALLED_APPS.
```

这是因为没有django依赖包，这时你又得装一下django了:sudo apt-get install python3-django

然后你还想生成sql语句，你就运行了python manage.py sqlall books,错误提示是
Unknown command: ‘sqlall‘Type ‘manage.py help‘ for usage.同样如果
你想提交sql语句到数据库而运行syncdb，错误提示是Unknown command: ‘syncdb‘
Type ‘manage.py help‘ for usage. 为什么没有这些命令，因为它们被淘汰了。
所以你只需运行如下的命令：
```python
python manage.py makemigrations books    #用来检测数据库变更和生成数据库迁移文件

python manage.py migrate     #用来迁移数据库

python manage.py sqlmigrate books 0001 # 用来把数据库迁移文件转换成数据库语言
```
在命令行依次执行完这三个命令你就可以进行数据访问了。

###基本数据访问
一旦你创建了模型，Django自动为这些模型提供了高级的Python API。 
运行 python manage.py shell 并输入下面的内容试试看：

```python
>>> from books.models import Publisher
>>> p1 = Publisher(name='Apress', address='2855 Telegraph Avenue',
...     city='Berkeley', state_province='CA', country='U.S.A.',
...     website='http://www.apress.com/')
>>> p1.save()
>>> p2 = Publisher(name="O'Reilly", address='10 Fawcett St.',
...     city='Cambridge', state_province='MA', country='U.S.A.',
...     website='http://www.oreilly.com/')
>>> p2.save()
>>> publisher_list = Publisher.objects.all()
>>> publisher_list
[<Publisher: Publisher object>, <Publisher: Publisher object>]
```

这短短几行代码干了不少的事。 这里简单的说一下：

首先，导入Publisher模型类， 通过这个类我们可以与包含 出版社 的数据表进行交互。

接着，创建一个`` Publisher`` 类的实例并设置了字段`` name, address`` 等的值。

调用该对象的 save() 方法，将对象保存到数据库中。 Django 会在后台执行一条 INSERT 语句。

最后，使用`` Publisher.objects`` 属性从数据库取出出版商的信息，这个属性可以认为是包含出版商的记录集。 这个属性有许多方法， 这里先介绍调用`` Publisher.objects.all()`` 方法获取数据库中`` Publisher`` 类的所有对象。这个操作的幕后，Django执行了一条SQL `` SELECT`` 语句。

这里有一个值得注意的地方，在这个例子可能并未清晰地展示。 当你使用Django modle API创建对象时Django并未将对象保存至数据库内，除非你调用`` save()`` 方法：

```python
p1 = Publisher(...)
# At this point, p1 is not saved to the database yet!
p1.save()
# Now it is.
```

如果需要一步完成对象的创建与存储至数据库，就使用`` objects.create()`` 方法。 下面的例子与之前的例子等价：

```python
>>> p1 = Publisher.objects.create(name='Apress',
...     address='2855 Telegraph Avenue',
...     city='Berkeley', state_province='CA', country='U.S.A.',
...     website='http://www.apress.com/')
>>> p2 = Publisher.objects.create(name="O'Reilly",
...     address='10 Fawcett St.', city='Cambridge',
...     state_province='MA', country='U.S.A.',
...     website='http://www.oreilly.com/')
>>> publisher_list = Publisher.objects.all()
>>> publisher_list
```

数据过滤2

我们很少会一次性从数据库中取出所有的数据；通常都只针对一部分数据进行操作。 在Django API中，我们可以使用`` filter()`` 方法对数据进行过滤：

```python
>>> Publisher.objects.filter(name='Apress')
[<Publisher: Apress>]
```
filter() 根据关键字参数来转换成 WHERE SQL语句。 前面这个例子 相当于这样：

```python
SELECT id, name, address, city, state_province, country, website
FROM books_publisher
WHERE name = 'Apress';
```
你可以传递多个参数到 filter() 来缩小选取范围：

```python
>>> Publisher.objects.filter(country="U.S.A.", state_province="CA")
[<Publisher: Apress>]
```

注意，SQL缺省的 = 操作符是精确匹配的， 其他类型的查找也可以使用：

```python
>>> Publisher.objects.filter(name__contains="press")
[<Publisher: Apress>]
```
在 name 和 contains 之间有双下划线。和Python一样，Django也使用双下划线来表明会进行一些魔术般的操作。这里，contains部分会被Django翻译成LIKE语句：

```python
SELECT id, name, address, city, state_province, country, website
FROM books_publisher
WHERE name LIKE '%press%';
```
其他的一些查找类型有：icontains(大小写无关的LIKE),startswith和endswith, 还有range(SQLBETWEEN查询）


后台:admin
你要使用你原来设置的超级用户的用户名和密码。 如果无法登录，请运行`` python manage.py createsuperuser`` ，确保你已经创建了一个超级用户。(提醒一句: 只有当INSTALLED_APPS包含'django.contrib.auth'时，python manage.py createsuperuser这个命令才可用.)

如果你的母语不是英语，而你不想用它来配置你的浏览器，你可以做一个快速更改来观察Django管理工具是否被翻译成你想要的语言。 仅需添加`` ‘django.middleware.locale.LocaleMiddleware’`` 到`` MIDDLEWARE`` 设置中，并确保它在’django.contrib.sessions.middleware.SessionMiddleware’* 之后* 

将你的Models加入到Admin管理中3
有一个关键步骤我们还没做。 让我们将自己的模块加入管理工具中，这样我们就能够通过这个漂亮的界面添加、修改和删除数据库中的对象了。我们定义了三个模块： Publisher 、 Author 和 Book 。

在`` books`` 目录下(`` mysite/books`` )，创建一个文件：`` admin.py`` ，然后输入以下代码：

```python
from django.contrib import admin
from mysite.books.models import Publisher, Author, Book

admin.site.register(Publisher)
admin.site.register(Author)
admin.site.register(Book)
```

这些代码通知管理工具为这些模块逐一提供界面。

完成后，打开页面 `` http://127.0.0.1:8000/admin/`` ，你会看到一个Books区域，其中包含Authors、Books和Publishers。  （你可能需要先停止，然后再启动服务(`` runserver`` )，才能使其生效。）
如果有报错，那就把from mysite.books.models import Publisher, Author, Book改为from books.models import Publisher, Author, Book









