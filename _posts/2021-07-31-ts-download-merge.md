---
layout:     post
title:      "Video Stream Download and Merge"
subtitle:   " video is divided into small files with extension .ts, to download and the merge them into one large mp4 file "
date:       2021-07-31 21:26:20
author:     "Dr.X"
header-img: "img/20210731waic2021.jpg"
commenting: open
tags:
    - video download
    - ts file merge
---

<h1> Find .ts Files </h1>

找到视频流文件的方法：在视频播放页面，点击右键，选择**inspect**，在打开的右侧窗口里面的**Network**，可以看到有***.m3u8_???.ts的文件，其中问号代表数值，是文件序号。点击该文件，选择“复制”&rarr;“复制链接地址”即是所需要的下载地址。

<h1>Write Shell File to Download the .ts Files</h1>

Below is an example for downloading the .ts files from the WAIC2021 conference.

to download the session:
> “未来办公 文本赋能” 智能语义分析应用论坛 
> 前面有一些录像会议还没开始，所以略掉，从108.ts开始下载，但是截止数通常需要猜测，然后再验证。https://myun-hw-s3.myun.tv/n856xo5m/0mopm314/0m1y7g40/5rybozdl/origin.m3u8_108.ts 

此处的downloadVideo.sh文件所在的文件夹有一个子文件夹名称为"ts_list"，注意，这里下载的时候，如果没能下载成功，或者下载速度太慢，可以重新运行命令
./downloadVideo.sh，这个命令不会导致重新下载之前已经下载过的.ts文件，因为代码已经有考虑。但是，如果上次下载不全的文件，应该先删除以后再运行上面的命令。所有中间缺失的文件都会重新下载。

```
#!/bin/bash

FILE="./ts_list/origin.m3u8_"
fext=".ts"
for i in `seq 108 3000` 
    do 
        fname="$FILE$i$fext"
        echo $fname
        if [ -f "$fname" ];then
            echo "file exists"
        else
            echo "file doesn't exist"
            str1=$i\.ts
            echo $str1
            wget https\:\/\/myun-hw-s3\.myun\.tv\/n856xo5m\/0mopm314\/0m1y7g40\/5rybozdl\/origin\.m3u8_$str1 -P ./ts_list/
            sleep 0.1s	# 延迟1s，视网速而定
        fi
    done
```

<h1>Merge the .ts Files into One Video File</h1>

shell命令非常容易就能够把零散的多个ts文件合并为一个文件。
将多个文件名称列在一起，前面加上cat，然后跟一个大于号，后面跟保存的位置和文件名即可。
此处的mergeVideo.sh文件所在目录有一个子目录名称为"ts_list"，所有的ts文件都保存在此文件夹中。

```
#!/bin/bash
# str=""
str2=""
str1=".ts "
str3="./ts_list/origin.m3u8_"

for i in `seq 108 1133`
    do
        str2=$str2$str3$i$str1
    done

str0="1134.ts"
str2=$str2$str3$str0
echo $str2
cat $str2 >/Volumes/Untitled/test.mp4
```

<h1>执行命令及注意事项</h1>

<h2>修改文件权限</h2>
完成上述两个文件代码的撰写以后，需要修改文件权限才能运行：
```
chmod ugo+x mergeVideo.sh
```

<h2>运行</h2>
在.sh文件所在目录，./mergeVideo.sh，或者sh mergeVideo.sh
否则给出完整路径。

<h1>参考</h1>

> https://blog.csdn.net/wayne17/article/details/87873566