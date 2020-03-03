---
layout:     post
title:      "git command line encoding setting for Chinese characters"
subtitle:   "git utf-8 encoding"
date:       2020-03-03 13:10:00
author:     "Dr.X"
header-img: "img/post-bg-20200302.jpg"
commenting: open
tags:
    - 休闲
    - 学习
---

# 起源
git命令行莫名其妙的出现乱码，中文显示不正确，多次设置不成功。很烦人啊。

# git命令行编码设置
其实就是一个编码设置的问题，包括两个方面的设置，命令行自身的设置和git的设置。

## 命令行（command terminal）设置

Terminal &rarr; Preferences &rarr; Profiles &rarr; Advanced &rarr; International &rarr; Text encoding: Unicode (UTF-8)

<img src="http://yonghong.github.io/img/terminal-config.jpg" width="350"/>


## git设置
搜索到.gitconfig文件的位置，一般位于Users/your-user-name/.gitconfig
修改编码规则
```
[gui]  
    encoding = utf-8  
    # 代码库统一使用utf-8  
[i18n]  
    commitencoding = utf-8  
    # log编码  
[svn]  
    pathnameencoding = utf-8  
    # 支持中文路径  
```
并在[core]中增加一行
```
[core]
    quotepath = false 
    # status引用路径不再是八进制（反过来说就是允许显示中文了）
```
<img src="http://yonghong.github.io/img/gitconfig-config.png" width="350"/>

# 结束

重启命令行(terminal)，然后用git status或者其他git命令即可看到能够正确显示中文路径了。