---
title: Centos7.x将Python2升级到Python3.x
date: 2023-01-12 22:29:08
comments: true
toc: true
tags: [linux,python]
categories: [Linux]
---

### 查看Python版本

```shell
python -V
```

### 更新[yum](https://so.csdn.net/so/search?q=yum&spm=1001.2101.3001.7020)源

```shell
yum update
```

### 安装依赖

```shell
yum install yum-utils
yum-builddep python3
```

### 下载python

```shell
wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz
```

### 安装Python相关依赖

```shell
yum -y install zlib-devel bzip2-devel openssl-devel ncursesdevelsqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel
```

<!-- more -->

### 安装c，c++

已经安装可跳过此步骤。

```shell
yum -y install gcc g++
```

### 创建安装目录

```shell
mkdir /usr/local/python3
```

### 解压

```shell
tar xf Python-3.8.5.tgz
```

### 编译

```shell
cd Python-3.8.5/
# 配置安装目录
./configure --prefix=/usr/local/python3
# 编译
make
```

### 安装

```shell
make install
```

### 创建软链接

```shell
ln -s /usr/local/python3/bin/python3 /usr/bin/python
# 注意这样是修改Python3为默认，那么这样还需要修改yum配置，后面会提到
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip
```

### 完成

```shell
python3 -V
pip3 -V
```

### 更改yum配置(非必要)

取决于你是否将python3设置为了默认，如果是可以执行下面操作。

因为yum要用到python2.x，否则会导致yum不能正常使用（不管安装 python3的那个版本，都必须要做的）

```shell
vim /usr/bin/yum 
把 #! /usr/bin/python 修改为 #! /usr/bin/python2
vim /usr/libexec/urlgrabber-ext-down 
把 #! /usr/bin/python 修改为 #! /usr/bin/python2
```

