+++
title = "CentOS7 | 配置Python3环境"
date = "2021-03-17"
author = "xblzbjs"
tags = [
  "Linux",
]
+++


环境：腾讯云CentOS7.5 64位

## 1. 查看Python的位置并安装相关依赖

```shell
[root@centos ~]# whereis python

python2: /usr/bin/python2 /usr/bin/python2.7 /usr/lib/python2.7 /usr/lib64/python2.7 /usr/include/python2.7 /usr/share/man/man1/python2.1.gz
```

python指向的是python2，python2指向的是python2.7，因此我们可以装个python3，然后将python指向python3，然后python2指向python2.7，那么两个版本的python就能共存了。

```shell
yum install zlib-devel libffi-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make 
```

## 2. 使用wget下载Python3源码包

```shell
wget http://npm.taobao.org/mirrors/python/3.7.9/Python-3.7.9.tar.xz

# 如果提示wget未找到命令, 那么就先使用yum安装wget
yum -y install wget
```

## 3. 编译Python3源码包

```shell
#解压
xz -d Python-3.7.9.tar.xz
tar -xf Python-3.7.9.tar
 
#进入解压后的目录，依次执行下面命令进行手动编译
cd Python-3.7.9
./configure prefix=/usr/local/python3
make && make install
 
# 如果出现can't decompress data; zlib not available这个错误，则需要安装相关库
#安装依赖zlib、zlib-devel
yum install zlib zlib
yum install zlib zlib-devel
```

## 4. 添加软链接

```shell
#将原来的链接备份
mv /usr/bin/python /usr/bin/python.bak
 
#添加python3的软链接
ln -s /usr/local/python3/bin/python3.7 /usr/bin/python
 
#测试是否安装成功了
python -V


```

此时会出现一个问题，yum命令报错。因为其要用到python2才能执行，否则会导致yum不能正常使用

```shell
[root@centos ~]# yum
  File "/usr/bin/yum", line 30
    except KeyboardInterrupt, e:
```

## 5. 更改yum配置

```shell
vi /usr/bin/yum
把#! /usr/bin/python修改为#! /usr/bin/python2
 
vi /usr/libexec/urlgrabber-ext-down
把#! /usr/bin/python 修改为#! /usr/bin/python2
```

## 6. 测试python2、python3、yum

```shell
[root@centos ~]# python
Python 3.7.9 (default, Dec 23 2020, 10:50:06) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 

[root@centos ~]# python2
Python 2.7.5 (default, Apr  2 2020, 13:16:51) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-39)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 

[root@centos ~]# yum
Loaded plugins: fastestmirror, langpacks
You need to give some command
Usage: yum [options] COMMAND

List of Commands:
......

```

## 7. 配置pip3和pip

python3 中自带有pip3，因此只需要添加pip3的软链接即可。

```shell
# 添加pip3的软连接
ln -s /usr/local/python3/bin/pip3  /usr/bin/pip3
# pip3 更新
pip3 install --upgrade pip3
```

pip安装

```shell
# 安装源
yum -y install epel-release
# 安装pip
yum install python-pip
# 对安装好的pip进行升级
pip install --upgrade pip
```
