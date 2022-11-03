---
title: RD Client搭建低延迟远程桌面
date: 2022-11-04 01:19:29
tags: 分享
categories: 分享
cover: https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/wallhaven-gpjmyq.jpg

---

## 概述

日葵、Todesk这样的远程软件相信大家都用过

但是在使用过程中可能遇到以下**问题**

- 速度慢，画面卡
- 远程桌面的原理是客户端发送一帧一帧的截图到服务端再到客户端，带宽占用大
- 手机平板蓝牙鼠标键盘映射极其糟糕

今天给大家介绍一个完美替代市面上远程软件的方案——**RD Client**

**优点**如下

- RD Client是window自带的工具，兼容性强，低带宽低延迟
- 蓝牙映射的逻辑更加底层，是从远程直接连接到另一边的主机，所以兼容性很好，亲测除了win键都能对的上



下面给大家介绍具体步骤

## 一、内网穿透

RD Client只能在同一个局域网内使用，意思是你两台设备得连同一个wifi。既然都在同一个局域网了，那还远程个毛啊，所以我们需要做一个内网穿透，使用frp建立一个代理服务器，实现反向代理。

> 需要一个有公网ip的云服务器

### frp

> frp 是一个专注于内网穿透的高性能的反向代理应用，支持 TCP、UDP、HTTP、HTTPS 等多种协议。可以将内网服务以安全、便捷的方式通过具有公网 IP 节点的中转暴露到公网。

frp在github上开源

#### linux环境(云服务器,用作frp服务端)

wget命令远程下载

```shell
wget https://github.com/fatedier/frp/releases/download/v0.44.0/frp_0.44.0_linux_amd64.tar.gz

```

解压

```scala
tar -zxvf frp_0.41.0_linux_amd64.tar.gz
```

删掉压缩包,然后查看解压后的文件内容

frpc frpc.ini 这两个文件没有用，是可以删去的(图里我已经删掉了)。

![image-20221103222206073](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221103222206073.png)

然后配置frps.ini文件

```shell
vi frps.ini
```

编辑内容

```shell
[common] bind_port = 7000 //客户端和服务端连接的端口
dashboard_port = 7500 //服务端仪表板的端口
token = 1234567890 //客户端和服务端连接的口令，可自行修改
dashboard_user = username //仪表板页面登录的用户名，可自行修改
dashboard_pwd = 0987654321 //仪表板页面登录的密码，可自行修改
vhost_http_port = 10080 //用不到这个功能，可写可不写，直接删去也可以
vhost_https_port = 10443 //同上
```

`:wq`退出保存,然后输入`./frps -c frps.ini`即可运行服务端

此时访问ip:7500输入之前设置的dashboard_user和dashboard_pwd即可访问仪表盘界面。

`Ctrl+C`停止运行,用`nohup`指令后台运行

```shell
nohup ./frps -c frps.ini &
```

输出`nohup: ignoring input and appending output to 'nohup.out'`代表运行正常。

#### windows环境(用作frp客户端)

与服务器端类似，从github上下载windows相应版本的frp程序，解压在本地，同样可以删除frps和frps.ini文件。
编辑frpc.ini文件，与服务器端类似

[frp的github链接](https://github.com/fatedier/frp)

```shell
[common]
server_addr = x.x.x.x ;服务器ip地址或域名
server_port = 7000 ;填入服务器上填写的bind_port
token = 1234567890 ;更改与服务器端填写的一致
[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 3389 ;配置远程桌面的接口，Windows的RD Client一般为3389接口
```

随后命令行输入`frpc.exe -c frpc.ini`显示succeess表示成功

此时访问ip:7500仪表盘看到客户数量为1,那么你就成功了

> 可以写一个开机自启动的脚本让客户端一直开着

## 二、Windows远程桌面设置

#### 绿色方法解锁远程桌面

> windows远程桌面是专业版特有的，专业版可以跳过这一步

非专业版我们采用工具解锁远程桌面

工具的[github链接](https://github.com/stascorp/rdpwrap/releases)

解压后打开文件夹，以管理员身份运行install.bat文件。

接着打开RDPConf.exe，如果是下图状态，即配置成功。

![image-20221103223730733](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221103223730733.png)

如果出现not listening not supported，大概率是rdpwrap.ini有问题。在[issue](https://github.com/stascorp/rdpwrap/issues?page=6&q=is:issue is:open)页找对应版本的rdpwrap.ini。

我这里提供一份热心网友整理的ini合集,上传在我的[个人网盘](http://43.143.121.13:8080/s/9pCs2wZipn5E2bi),想要搭建个人网盘可以看我上一篇博客

下载好rdpwrap.ini后，
1）以管理员身份运行cmd，输入命令`net stop termservice`, 回车，终止运行Remote Desktop Service。
2）用下载得到的对应版本的的rdpwrap.ini去替换C:\Program Files\RDP Wrapper 目录里面旧的 rdpwrap.ini
3）在cmd命令行输入命令`net start termservice`, 回车，恢复运行Remote Desktop；
4）点击“RDPConf.exe”，全绿。

此时可以用RDPCheck检测一下，有下图所示登录回显即表示成功。
![image-20221103224016348](https://figurebed48981.oss-cn-hangzhou.aliyuncs.com/img_for_blog/image-20221103224016348.png)

## 三、在ipad上连接

在App Store上下载RD Client，添加电脑，电脑名称即为服务器ip地址加上在windows上frpc.ini中填写的remote_port端口号，然后在用户账户上添加你的windows域\账户或账户@域，再输入密码即可远程控制你的windows桌面了！
命令行键入whoami查看自己电脑的域\账户

iPad连接上电脑后，电脑会自动进入锁屏状态，如果此时远程控制将电脑睡眠，依然不会影响iPad的远程控制。并且电脑的音频会远程传输到iPad上，不用担心你的PC会忽然发出声音。



> frp的相关配置可以查阅[文档 | frp (gofrp.org)](https://gofrp.org/docs/)

> 优化操作参考[这篇文章](https://blog.csdn.net/csdn_life18/article/details/108250846)

