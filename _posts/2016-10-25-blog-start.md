---
layout: post
title: "搭建博客所做的笔记"
date: 2016-10-25 16:02:00
image: '/assets/img/'
description: '搭建博客所做的笔记'
tags:
- jekyll 
- template 
categories:
- I love Jekyll
---

1.在github上建一个仓库repository,命名规则为你的github用户名,例如：zhenghuom

2.自已写页面或在网在找一些jekyll的模板然后做一些修改，然后把模板放到你在github
创建好的仓库上，这时你就可以访问你的博客了，访问的域名为https://你所在github上
注册的名字.github.io/，如https://zhenghuom.github.io/  

> 注意，引入的样式和js协议要是https的，http可能会报错，最好把这些文件都
> 放在自己的目录下

3.若不是在网上找模板的，你要在自己本地环境上安排jekyll

*jekyll 中文网站 `http://jekyll.com.cn/`

*安装jekyll 地址 `http://jekyll.com.cn/docs/installation/`  
1.使用apt安装   
    `sudo apt install jekyll`   
2.使用gem安装   
    `gem install jekyll`
 
#Jekyll基本结构

*`_config.yml`   配置文件，用来定义你想要的效果，设置之后就不用关心了。

*`_drafts`       drafts 是未发布的文章。这些文件的格式中都没有 title.MARKUP 数据。学习如何使用 drafts.

*`_includes`     可以用来存放一些小的可复用的模块，方便通过{ % include file.ext %}（去掉前两个{中或者{与%中的空格，下同）灵活的调用。这条命令会调用_includes/file.ext文件。

*`_layouts`      这是模板文件存放的位置。模板需要通过YAML front matter来定义，后面会讲到，{ { content }}标记用来将数据插入到这些模板中来。

*`_posts `       你的动态内容，一般来说就是你的博客正文存放的文件夹。他的命名有严格的规定，必须是2012-02-22-artical-title.MARKUP这样的形式，MARKUP是你所使用标记语言的文件后缀名，根据_config.yml中设定的链接规则，可以根据你的文件名灵活调整，文章的日期和标记语言后缀与文章的标题的独立的。

*`_site `        这个是Jekyll生成的最终的文档，不用去关心。最好把他放在你的.gitignore文件中忽略它。

#代码高亮样式

js和css和配置可以网上找
如highlight：https://highlightjs.org/

系统默认高亮代码配置
`markdown: kramdown`
系统默认的高亮代码格式

> { % highlight javascript % }  
> var a = 1;    
> var b = 2;    
> var c = a + b;    
> { % endhighlight % }

效果

{% highlight javascript %}  
var a = 1;    
var b = 2;    
var c = a + b;    
{% endhighlight %}


#若想把高亮变成这做格式

>\`\`\`PHP    
>/*  
>* 师傅未处理的订单  
>*/  
>function untreatedOrder($masterId){    
>&emsp;&emsp;if(!$masterId){   
>&emsp;&emsp;&emsp;&emsp;return [];    
>&emsp;&emsp;}   
>&emsp;&emsp;$list = D('UserOrder')
->field('orderAddr,FROM_UNIXTIME(addTime,"%Y-%m-%d %H:%i:%s") as addTime')
->where(['masterID'=>$masterId,'status'=>array('not in','2,3'),'isOn'=>1,'isFalseOrder'=>0])
->select();      
>}  
>\`\`\`

效果

```php
/*  
 * 师傅未处理的订单 
 */ 
function untreatedOrder($masterId){
    if(!$masterId){
        return [];
    }
    $list = D('UserOrder')->field('orderAddr,FROM_UNIXTIME(addTime,"%Y-%m-%d %H:%i:%s") as addTime')
        ->where(['masterID'=>$masterId,'status'=>array('not in','2,3'),'isOn'=>1,'isFalseOrder'=>0])
        ->select();

    return $list;
}
```

这里_config.yml配置更改成
`markdown: redcarpet`

\#设置高亮   
`redcarpet:`    
  `extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "strikethrough", "superscript", "tables"]`   
  `render_options:`
  
注意：都次更改_config.yml都要重启jekyll才有效



