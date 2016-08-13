---
layout: post
title: "mac上升级python遇到easy_install,pip无法使用的解决办法"
date: 2016-07-01 15:32:24
categories: python安装
tags: python pip
author: xiaovan
---

* content
{:toc}


##问题
因遇到问题后stackoverflow和网站上没有发现有人写，自己就记录一下，首先按照[python升级参考][ref]将mac上的python版本升级到3.5

升级后，easy_install和pip命令均无法使用，报错如下:

```bash
prompt@easy_install sth

File "/usr/bin/easy_install", line 31
continue
^
TabError: inconsistent use of tabs and spaces in indentation
```

##解决办法

```bash
sudo rm -f /usr/bin/easy_install*
sudo ls -s /System/Library/Frameworks/Python.framework/Versions/3.5/bin/easy_install /usr/bin/install
sudo ls -s /System/Library/Frameworks/Python.framework/Versions/3.5/bin/pip3 /usr/bin/pip
```

重建链接后，问题解决

[ref]: http://shyhornet.github.io/2015/10/21/mac(OSX%2010.11)%E4%B8%8A%E6%9B%B4%E6%96%B0Python/
