---
layout: post
title: "php批量下载"
date: 2017-1-12 18:51:00
image: '/assets/img/'
description: 'php批量下载服务器的图片'
tags:
- php
- 批量下载 
- 多图片下载
categories:
- just do it

---

#php批量下载

```php

function exportImg(){
        $list = array(
            [0] => array(
                'name' =>'图片1',
                'img' => '1.jpg',
            ),
            [1] => array(
                'name' =>'图片2',
                 'img' => '2.jpg',
            ),
            [2] => array(
                'name' =>'图片3',
                    'img' => '3.jpg',
            ),
            [3] => array(
                'name' =>'图片4',
                'img' => '4.jpg',
            ),
        );

       
        //创建压缩包的路径
        $filename = '/Public/images.zip';

        $zip = new \ZipArchive();
        $zip->open($filename,$zip::CREATE);
        $zip->addEmptyDir('images');
        foreach ($list as $value){
            if(empty($value['img'])) continue;
            //获取图片
            $fileData = file_get_contents($value['img']);
           
            if ($fileData) {
                //图片名字
                $imgName = $value['name'] . '.' . substr(strrchr($value['img'], '.'), 1);
                //把图片添加和压缩包
                $add = $zip->addFromString('images/'.$imgName, $fileData);
            }

        }
        $zip->close();

        //下载文件
        ob_end_clean();
        header("Content-Type: application/force-download");
        header("Content-Transfer-Encoding: binary");
        header('Content-Type: application/zip');
        header('Content-Disposition: attachment; filename=师傅头像.zip');
        header('Content-Length: '.filesize($filename));
        error_reporting(0);
        readfile($filename);
        flush();
        ob_flush();

        exit;
    }

```

