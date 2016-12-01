---
layout: post
title: "滚动条到达底部触发的事件"
date: 2016-12-1 14:25:00
image: '/assets/img/'
description: '滚动条到达底部触发的事件'
tags:
- javascript 
- php 
categories:
- I love Jekyll
twitter_text: 'How to install and use this template'
---

#滚动条到达底部触发的事件

```javascript
//获取滚动条当前的位置
function getScrollTop() {
    var scrollTop = 0;
    if (document.documentElement && document.documentElement.scrollTop) {
        scrollTop = document.documentElement.scrollTop;
    } else if (document.body) {
        scrollTop = document.body.scrollTop;
    }
    return scrollTop;
}
//获取当前可是范围的高度
function getClientHeight() {
    var clientHeight = 0;
    if (document.body.clientHeight && document.documentElement.clientHeight) {
        clientHeight = Math.min(document.body.clientHeight, document.documentElement.clientHeight);
    } else {
        clientHeight = Math.max(document.body.clientHeight, document.documentElement.clientHeight);
    }
    return clientHeight;
}
//获取文档完整的高度
function getScrollHeight() {
    return Math.max(document.body.scrollHeight, document.documentElement.scrollHeight);
}
window.onscroll = function() {
    if (getScrollTop() + getClientHeight() == getScrollHeight()) { //判断滚动条到达底部的条件,并自动触发下面AJAX

    }
}
    
```

#Thinkphp中文分页问题

把$this->url = U(ACTION_NAME, $this->parameter);注释掉

换成$this->url = $this->clin_page_url($this->parameter); // 生成标准的url

```php

private function clin_page_url($parameter){
        $url = U('');
        $url = str_replace('.html', '?', $url);
        foreach ($parameter as $key => $value) {
            $url .= $key.'='.$value.'&';
        }
        $url = substr($url, 0,-1);
        return $url;
    }

```