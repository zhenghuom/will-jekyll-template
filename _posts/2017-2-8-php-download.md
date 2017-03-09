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


>如果你的程序提示没有这个类，如：Class 'ZipArchive' not found，
请自己行安装php_zip.dll扩展，这里有个参考链接： http://jingyan.baidu.com/article/636f38bb3e538ad6b84610e6.html

#单个文件下载
```php

/**
 *  下载文件
 * @param type $file_dir 文件所在目录
 * @param type $file_name 文件名
 * @return boolean
 */
function downloadFile($file_dir, $file_name)
{
    if (DEBUG)
        file_put_contents("download.txt", "下载的文件路劲：$file_dir/$file_name..\n", 1);

    $file_name = iconv('UTF-8', 'GBK//IGNORE', $file_name);

    $file_dir = rtrim($file_dir); //去掉路径中多余的空格
    //得出要下载的文件的路径
    if ($file_dir != '')
    {
        $file_dir = iconv('UTF-8', 'GBK//IGNORE', $file_dir);
        $file_path = $file_dir;
        if (substr($file_dir, strlen($file_dir) - 1, strlen($file_dir)) != '/')
            $file_path .= '/';
        $file_path .= $file_name;
    } else
        $file_path = $file_name;

    //判断要下载的文件是否存在
    if (!file_exists($file_path))
    {
        echo '对不起,你要下载的文件不存在。';
        return false;
    }
    $file_size = filesize($file_path); // 得到文件大小
    header("Content-type: application/octet-stream");
    // header("Content-type: application/zip");
    header("Accept-Ranges: bytes");
    header("Accept-Length: $file_size");
    header("Content-Disposition: attachment; filename=" . $file_name);

    $fp = fopen($file_path, "r");
    $buffer_size = 1024;
    $cur_pos = 0;

    while (!feof($fp) && $file_size - $cur_pos > $buffer_size)
    {
        $buffer = fread($fp, $buffer_size);
        echo $buffer;
        $cur_pos += $buffer_size;
    }

    $buffer = fread($fp, $file_size - $cur_pos);
    echo $buffer;
    fclose($fp);

    return true;
}

```

