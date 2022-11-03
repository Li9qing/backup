---
title: 阿里云&PicGo|Typora图床
date: 2022-10-22 18:08:57
tags: 分享
categories: 分享
cover: https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/wallhaven-weog1q.jpg
---

## 概述

利用阿里云的**对象存储OSS**和PicGo搭建图床

在**typora**中启用图床,实现自动上传.

## 第一步:购买对象存储OSS资源包

![image-20221022181340458](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022181340458.png)

## 第二步:创建Bucket

点击创建

![image-20221022181637855](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022181637855.png)

名称随便取,地域选离自己近的,存储类型选择标准存储.

HDFS,同城冗余以及版本控制没必要开通.

读写权限一定要选择公共读.

日志和备份不开通,骗流量的东西.

![](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022181700208.png)

## 第三步:创建Access Key

入口在页面右上角的头像那里,位置有点偏僻

![image-20221022182238716](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022182238716.png)

点击继续

![image-20221022182410157](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022182410157.png)

点击创建,复制id和key,我之前创建过这里就不演示了

## 第四步:下载,配置PicGo

仓库地址:[Molunerfinn/PicGo: A simple & beautiful tool for pictures uploading built by vue-cli-electron-builder (github.com)](https://github.com/Molunerfinn/PicGo)

安装打开

![image-20221022183057134](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022183057134.png)

点击图床设置,找到阿里云OSS

![image-20221022183232104](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022183232104.png)

KeyId和KeySecret就是刚才我们配置的Access Key

储存空间在这个地方,不要带上aliyuncs.com

![image-20221022183403354](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022183403354.png)

指定储存路径就是你在bucket里创建的文件夹路径

![image-20221022183508709](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022183508709.png)

点击确认,设置为默认图床

**到这里图床已经搭好了,拖拽即可完成上传**

## 第五步:关联Typora

改一下这两个参数,然后点击验证,

![image-20221022183646232](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022183646232.png)

验证成功的话返回PicGo或者你的阿里云面板里可以看到示例图片

这里就不放图片了



使用方法如下

![](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022183852979.png)
