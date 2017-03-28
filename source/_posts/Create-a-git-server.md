---
title: 在Linux新建Git服务器
date: 2017-03-27 16:10:32
tags:
       - Linux
categories: Linux
---
>这几天导师要求在实验室的服务器上(CentOS)搭建一个Git仓库用于管理实验室项目代码。正好，在搭建个人博客，就把这个过程贴上来.

<!-- more -->
#### 安装git
```
yum install git
```
#### 创建一个git用户
用来运行git服务
```
useradd -m git
```
#### 安装Gitosis管理公钥
安装命令

```
yum install -y python python-setuptools git-core
git clone https://github.com/res0nat0r/gitosis.git
cd gitosis
python setup.py install
```
软件安装完之后是一些配置。先用服务器生成的ssh公钥初始化gitosis。然后把远程PC的公钥添加进并添加管理权限，即可在远程pc管理项目
```
ssh-keygen -t rsa(一路回车即可)
cp ~/.ssh/id_rsa.pub /tmp/
```
用刚才的公钥初始化gitosis
```
su git
gitosis-init</tmp/id_rsa.pub
```
在/home/git文件夹下面会生成gitosis和repositories两个文件夹。repositories即为存放公共库的文件夹。

![GitTree](Create-a-git-server/gittree.jpg)

修改/home/git/repositories/gitosis-admin.git/hooks/post-update权限，使具有执行权限
```
chmod 775 /home/git/repositories/gitosis-admin.git/hooks/post-update
```
然后在git桌面上克隆gitosis-admin

![img1](Create-a-git-server/gittreeone.jpg)

然后把PC生成的公钥放到keydir文件夹中，在gitosis.conf中添加管理员权限就可以在PC上远程管理权限和git项目
注意：此处公钥的文件名必须改成公钥文件里面最后的用户名，如下图所示的root@cnc217：

![img](Create-a-git-server/gitone.jpg)

PC具有权限之后，就可以在自己的电脑上管理git项目和权限，如需新添加开发成员，只需把新成员电脑生成的公钥放到gitosis-admin中keydir，在gitosis.conf中在相应的项目中添加新成员即可。



