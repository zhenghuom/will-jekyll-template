---
layout: post
title: "ajax上传文件"
date: 2016-11-1 18:56:00
image: '/assets/img/'
description: 'ajax上传文件'
tags:
- jekyll 
- template 
categories:
- I love Jekyll
---

#ajax上传文件

```script

$(‘#id’).change(function(){ 
    //找到文件
    var file = $(“#file”)[0].files[0]; 
    if(file == ”){ 
        alert(‘请选择文件’); 
        return false; 
    }

    var fileExt = file.name.substr(file.name.indexOf('.')+1);
    if(fileExt != 'txt'){
        alert('请选择文件txt');
        return false;
    }
    var form = new Form(this);
    form.append('file',file);
    //id为token的值
    form.append("token", $(this).find("#token").val());
    //id为name的值
    form.append('name',$(this).find("#name").val())
    $.ajax({
        type:'post',
        url:'url',
        data:form,
        dataType:'json',
        contentType:false,
        processData:false,
        success:function(aData){
            //处理数据
        }
        error:function(XMLHttpRequest, textStatus, errorThrown){
            alert('请求失败!');
        }
    });
});
```



