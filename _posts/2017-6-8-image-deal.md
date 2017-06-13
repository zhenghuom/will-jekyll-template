---
layout: post
title: "前端图片处理成base64"
date: 2017-6-8 18:00:00
image: '/assets/img/'
description: '前端图片处理成base64'
tags:
- js
- html
- css
categories:
- just do it

---

#前端把图片处理成base64
- 插件 lrz.all.bundle.js

- 代码 

```javascript

// 图片上传
$(document).on("change",".file",function(e){
	e.stopPropagation();
	e.preventDefault();
	var uploadPic = this.files;
	var that = $(this);
	index=that.parents('li').index();
	console.log(index);
	var objUrl = getObjectURL(uploadPic[0]) ; //获取图片的路径，该路径不是图片在本地的路径
	if (objUrl) {
		//that.siblings("#img").attr("src", objUrl); //将图片路径存入src中，显示出图片

		for (var i = 0; i < uploadPic.length; i++) {
			lrz(uploadPic[i],{width:800})
				.then(function (rst) {
					// 处理成功会执行
					imgList[index] = rst.base64;
					console.log(imgList);
				})
				.catch(function (err) {
					// 处理失败会执行
				})
				.always(function () {
					// 不管是成功失败，都会执行
				});

		}

	}


	if(index>-2){
		$('.pic:last').css('display','none');
	}
		html=' <li class="pic"><img id="img" src="'+objUrl +'" alt=""/>  <span></span></li>'
		$('.uploadPic ul').prepend(html);


	//点击删除图片
	$('.uploadPic .pic>span').click(function () {
		var that=$(this);
		$('.delete_box').css('display','block');
		$('.mc').css('display','block');
		$('.btn .qx').click(function () {
			$('.mc').css('display','none');
			$('.delete_box').css('display','none');
		});
		$('.btn .qd').click(function () {
			$('.mc').css('display','none');
			$('.delete_box').css('display','none');
			that.parent('.pic').remove();
			console.log(that.parent('.pic'))

		})

	});

})

```

#透明圆角图片

```php

//圆角图片
function get_lt_rounder_corner($radius)
{
    $img = imagecreatetruecolor($radius * 2, $radius * 2);  // 创建一个正方形的图像
    $bgcolor = imagecolorallocate($img, 255, 255, 255);   // 图像的背景
    $fgcolor = imagecolorallocate($img, 0, 0, 0);
    imagefill($img, 0, 0, $bgcolor);

    imagefilledarc($img, $radius, $radius, $radius * 2, $radius * 2, 180, 180, $fgcolor, IMG_ARC_PIE);
    // 将弧角图片的颜色设置为透明
    imagecolortransparent($img, $fgcolor);
    // 变换角度
    // $img = imagerotate($img, 90, 0);
    // $img = imagerotate($img, 180, 0);
    // $img = imagerotate($img, 270, 0);
    // header('Content-Type: image/png');
    // imagepng($img);
    return $img;
}

```

