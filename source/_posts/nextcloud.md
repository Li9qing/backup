---
title: 快速搭建个人网盘
date: 2022-10-22 15:38:50
tags: 分享
categories: 分享
cover: https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/wallhaven-o3k1j9.jpg
---
## 第一步:安装docker

```shell
# 通过yum源安装docker
sudo yum -y install docker
# 启动docker
sudo systemctl start docker
# 开机自启
sudo systemctl enable docker
```

## 第二步: 获取启动nextcloud镜像

```shell
docker run -d -p 8080:80 nextcloud
```

公网ip+端口访问

![image-20221022180001608](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221022180001608.png)

拖拽上传即可
