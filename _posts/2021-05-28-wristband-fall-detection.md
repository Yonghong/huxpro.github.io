---
layout:     post
title:      "About Wearable Fall Detection"
subtitle:   "paper, dataset, device, etc., wearable devices fall detection "
date:       2021-05-28 20:20:20
author:     "Dr.X"
header-img: "img/post-txb-20210528.jpg"
commenting: open
tags:
    - machine leanring
    - algorithm
---

<h1> Online Information about Wearable Fall Detection </h1>

<h2> 1. Public Dataset - G-Sensor</h2>

### 1.1  [UP-Fall Detection Dataset](https://www.mdpi.com/1424-8220/19/9/1988/htm)

A paper called "UP-Fall Detection Dataset: A Multimodal Approach ([download PDF](https://www.mdpi.com/1424-8220/19/9/1988/pdf))", the authors made the data available publicly for access at [http://sites.google.com/up.edu.mx/har-up/](http://sites.google.com/up.edu.mx/har-up/).

Another paper [Fall Detection: Threshold Analysis of Wrist-Worn Motion Sensor Signals](https://scholar.smu.edu/cgi/viewcontent.cgi?article=1156&context=datasciencereview), utilize a subset of the publicly available dataset - *UP-Fall Detection dataset* - to analyze acceleration and angular velocity signals measured on wrist-worn sensors to simulate smartwatch placement and give a 3-phased threshold analysis model approach (AVC to K-Means to SVM) whose accuracy is about 78 percent.

### 1.2 [UR Fall Detection Dataset](http://fenix.univ.rzeszow.pl/~mkepski/ds/uf.html)

This dataset contains 70 (30 falls + 40 activities of daily living) sequences. Fall events are recorded with 2 Microsoft Kinect cameras and corresponding accelerometric data. ADL events are recorded with only one device (camera 0) and accelerometer. Sensor data was collected using PS Move (60Hz) and x-IMU (256Hz) devices.

The related paper is:

>Bogdan Kwolek, Michal Kepski, Human fall detection on embedded platform using depth maps and wireless accelerometer, Computer Methods and Programs in Biomedicine, Volume 117, Issue 3, December 2014, Pages 489-501, ISSN 0169-2607.

The above paper "*Human fall detection on embedded platform using depth maps and wireless accelerometer*" can be downloaded from [HERE](http://home.agh.edu.pl/~bkw/research/pdf/2014/KwolekKepski_CMBP2014.pdf).


<h2> 2. Public Dataset - Camera</h2>

### 2.1 [Fall Detection Dataset](http://falldataset.com/)

The datasets that are used for the simulation purpose are raw RGB and Depth images of size 320x240 recorded from a single uncalibrated Kinect sensor after resizing from 640x480. The Kinect sensor is fixed at roof height of approx 2.4m. The datasets contain a total of  21499 images. Out of total datasets of 22636 images, 16794 images can be used for training,  3299 images can be used for validation and 2543 images can be used for the test. The images in the dataset are recorded in 5 different rooms which consist of 8 different view angles. There are *5 different participants* out of which there are two male participants of age 32 and 50 and three female participants of age 19, 28 and 40. All the activities of the participants represent *5 different categories of poses* that are standing, sitting, lying, bending and crawling. There is only one participant in each image. Some images in the datasets are empty which are categorised as 'other'.  We have used images of 2 participants: the male of age 32 and the female of age 28 combining total of 16794 images for training, and 3299 images for validation which contains a male participant of age 32 from training set but is in a different room to that of training and testing set. Similarly, the test set contains images of 3 participants out of which 2 female participants are of age 19 and 40 and a male participant is of age 50. These images are recorded in a different room that is not seen in training or validation set. These total of 22636 images are in sequence but have not repeated anywhere in the sequence and all the sets have original and its horizontal flipped images added in sequence to increase the number of images in a set.

The related paper is [HERE - PDF version](https://core.ac.uk/reader/84144508).
> K. Adhikari, H. Bouchachia and H. Nait-Charif, "Activity recognition for indoor fall detection using convolutional neural network," 2017 Fifteenth IAPR International Conference on Machine Vision Applications (MVA), 2017, pp. 81-84, doi: 10.23919/MVA.2017.7986795.

### 2.2 [Multiple cameras fall dataset](http://www.iro.umontreal.ca/~labimage/Dataset/)

This dataset contain 24 scenarios recorded with 8 IP video cameras. The first 22 first scenarios contain a fall and confounding events, the last 2 ones contain only confounding events.

All technical information is given in this [pdf file](http://www.iro.umontreal.ca/~labimage/Dataset/technicalReport.pdf). All videos are gathered in one zipped file in [this file](http://www.iro.umontreal.ca/~labimage/Dataset/dataset.zip). 

> E. Auvinet, C. Rougier, J.Meunier, A. St-Arnaud, J. Rousseau, "Multiple cameras fall dataset", Technical report 1350, DIRO - Université de Montréal, July 2010.

### 2.3 [Le2i Fall detection Dataset]()

Le2i Fall detection Dataset百度网盘链接:[https://pan.baidu.com/s/188k4fKlQVKa0Jf7lr8qukg](https://pan.baidu.com/s/188k4fKlQVKa0Jf7lr8qukg) 提取码:qnu0

Note: the original data source is not available now. i.e., one cannot open this site: [http://le2i.cnrs.fr/Fall-detection-Dataset?lang=fr](http://le2i.cnrs.fr/Fall-detection-Dataset?lang=fr)

数据集大小：共有191个视频，视频中存在假摔部分，且存在视频中没有人的帧数

数据集内容：
<ul>
<li>Video,视频部分</li>
<li>Annotations,标签格式为txt格式</li>
<li>标签前两行为跌倒帧数的起始与终止点，也存在没有这两行的文件</li>
<li>之后一行就是六个字段，帧编号， XX，人物bbx</li>

<li>DATASET (Home, Coffee room, Office, Lecture room) </li>
<li>LE2I DIJON UMR6306</li>
<li>FORMAT 320x240 25FPS</li>
<li>For each subset GROUNDTRUTH in 'Annotation_files' folder</li>
<li>For each video a ground truth file is given 'video (i).txt'</li>
<li>each file contains:</li>
<ul>
<li>the frame number of the beginning of the fall</li>
<li>the frame number of the end of the fall </li>
<li>the height, the width and the coordinates of the center of the bounding box for each frame.</li>
</ul>
</ul>

> I. Charfi, J. Miteran, J. Dubois, M. Atri, and R. Tourki (2013), Optimized spatio-temporal descriptors for real-time fall detection: comparison of support vector machine and adaboost-based classification. Journal of Electronic Imaging 22 (4), pp. 041106.

