---
title: CentOS7安装k-vim
date: 2018-12-06 17:08:13
tags:
	- vim
	- centos
categories:
	- vim
---

### 卸载老版本vim

``` bash
rpm -qa | grep vim
rpm -e vim
```

### yum安装编译依赖

``` bash
yum install gcc git -y
yum install ncurses-devel -y
yum install lua lua-devel python-devel -y
yun install "Development tools"
```

### 源码安装vim

- 下载vim源码包，解压
``` bash
./configure --with-features=huge \ 
    --enable-rubyinterp=yes \      # 编译支持ruby
    --enable-pythoninterp=yes \    # 编译支持python2
    --enable-luainterp=yes \       # 编译支持lua
    --enable-cscope \
    --prefix=/usr/local/vim8.1

make && make install

```
- 设置PATH环境变量，在`/etc/profile.d/`下新建文件`vimpath.sh`，添加以下内容
``` bash
#!/bin/bash
export PATH=$PATH:/usr/local/vim8.1/bin/
```
- 卸载
``` bash
make uninstall
make clean
```

### 安装k-vim

克隆仓库[k-vim](https://github.com/wklken/k-vim)
``` bash
git clone https://github.com/wklken/k-vim
cd k-vim
./install.sh
```
脚本安装过程编译YCM可能出现失败，需要手动编译

### 编译YouCompleteMe

``` bash
cd k-vim/bundle/YouCompleteMe
python install.py --clang-completer
```
如果网络现在clang速度太慢可以使用本地系统clang库
``` bash
python install.py --clang-completer --system-libclang
```
