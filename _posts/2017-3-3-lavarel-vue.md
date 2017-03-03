---
layout: post
title: "laravel+vue"
date: 2017-3-3 11:45:00
image: '/assets/img/'
description: '初次搭建laravel5.4+vue2.0.1'
tags:
- php
- laravel5.4 
- vue
- bootstrap
categories:
- just do it

---

#这是我的环境包版本
```jascript
{
    "axios": "^0.15.2",
    "bootstrap-sass": "^3.3.7",
    "jquery": "^3.1.0",
    "laravel-mix": "^0.6.0",
    "lodash": "^4.16.2",
    "vue": "^2.0.1"
}
```

-- 在新版本laravel5.3以上是用vue作为前端框架的,，所以已经自动载入vue了，vue要用动npm管理，刚使用npm时，
总是报错，后来才知道是npm源使用不对，所以改要别的源：https://github.com/creationix/nvm
把原来的删除，注意：npm和node都用最新的，laravel中使用vue的官方文档：http://laravelacademy.org/post/6798.html
把js，css，image的资源放在resources下，把.css文件后缀改成.less，然后在webpack.mix.js
中把这些资源编译到public目录下，用法在laravel官方文档里有说里，若想使用npm下载下来的
js，可以在resources/assets/js/bootstrap.js中引入，方法为require('XXX')，就是node_modules
里面某一个模块的js文件，若想使用css，在resources/assets/sass/app.scss中引入，例如：
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap";

