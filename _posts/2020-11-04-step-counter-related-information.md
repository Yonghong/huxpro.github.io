---
layout:     post
title:      "step counter algorithms"
subtitle:   "with related infromation"
date:       2020-11-04 12:24:00
author:     "Dr.X"
header-img: "img/img-pedometer.png"
commenting: open
tags:
    - research
    - algorithm
---

<h1>Pedometer Algorithms and Related ...</h1>

<h2>Source code on <a href="http://github.com">Github</a></h2>

1. <a href="https://github.com/jiahongfei/TodayStepCounter">Today Step Counter</a>: Android计步模块（类似微信运动，支付宝计步，今日步数），记录当天从0点到23:59的步数.
    * Related Chinese Blog: <a href = "https://www.jianshu.com/p/1b53937150ad">Android计步模块优化（今日步数）V2.0.0</a>
2. <a href="https://github.com/MarcusNordstrom/C-Step-Counter">C-Step-Counter</a>: A bachelor thesis repo (C).
3. <a href="https://github.com/zhouguangfu09/StepCounter">StepCounter</a>: a small example about android pedometer.
4. <a href="https://github.com/MikeCodesDotNET/My-StepCounter">My-StepCounter</a>: Xamarin iOS & Android Starter pedometer sample.
5. <a href="https://github.com/nerajbobra/embedded_pedometer">embedded_pedometer</a>: Accelerometer-based pedometer algorithm for embedded applications (C). 
    * The related paper: <a href =https://www.microsoft.com/en-us/research/wp-content/uploads/2016/12/p3225-morris.pdf>2014 RecoFit: Using a Wearable Sensor to Find, Recognize, and Count Repetitive Exercises</a>, or <a href = "https://yonghong.github.io/file/2014-RecoFit_Using-a-Wearable-Sensor-to-Find-Recognize-and-Count-Repetitive-Exercises.pdf">direct download</a>.
6. <a href="https://github.com/ecruhue/Pedometer-Step-Counting-Algorithm">Pedometer-Step-Counting-Algorithm</a>: collected real time walking data(patterns) with gyroscope, use Fast Fourier Transformation to extract the clustering features, and build the step counting algorithm for pedometer (Python). 
7. <a href="https://github.com/ecruhue/Pedometer-Step-Counting-Algorithm">Pedometer-Step-Counting-Algorithm</a>: Lightweight pedometer app for Android using the hardware step sensor (Java).
8. <a href = "https://github.com/Akshayvs/gyroscope-step-counter-library">gyroscope-step-counter-library</a>: This code is a mini implementation of the step counting logic implemented in most of the fitness appliciations on our smartphones and fitness bands like FitBit.
9.  <a href = "https://github.com/zhouguangfu09/StepCounter">Step-Counter</a>: A small example about android pedometer.
    * Realted Chinese blog: <a href ="https://blog.csdn.net/guang09080908/article/details/41411679">Android 计步器开发</a>


<h1>Algorithm adopted in Matlab</h1>

<a href ="https://ww2.mathworks.cn/help/supportpkg/arduino/ref/gyroscope-based-pedometer.html">Matlab Gyroscope based pedometer page</a>: The pedometer algorithm is implemented inside the MATLAB function block in the Arduino model.

- Required Hardware
    * Arduino MKR1000/MKR WiFi 1010
    * Android device
    * MPU-9250 sensor

<h2>The algorithm file link</h2>

- <a href = "https://ieeexplore.ieee.org/document/6553971">A gyroscopic data based pedometer algorithm</a> (IEEE official site) or <a href="https://yonghong.github.io/file/2013-A-Gyroscope-based-Accurate-Pedometer-Algorithm.pdf">Direct Download Link</a>.
- <a href = "https://blog.csdn.net/lean_siege_lion/article/details/40087379">一种基于陀螺仪传感器的准确计步器算法</a>: The Translated Chinese Version.

<h2> The matlab code</h2>

Authors: Sampath Jayalath, Nimsiri Abhayasinghe and Iain Murray Contact: <a href="mailto:adasdjsampath@gmail.com">adasdjsampath@gmail.com</a>

Zero Crossing With Signal Strength (MATLAB code)

```matlab
%Zero Crossing with Signal Strength Algorithm%
clc
%Reading the Data File in .csv Format
fileName='walk.csv'; start=1;
stop=1600;
fs=100; m=stop-start+1; t=0:1/fs:(m-1)*(1/fs);
Gx=csvread(fileName, start, 3, [start,3,stop,3]); Gyro=Gx;
%///////////////////Defining Variables/////////////////%
in=[0,0,0,0,0,0,0]; Ou=[0,0,0,0,0,0,0];
global counter1; counter1=0;
global counter2; counter2=0;
global var_k; var_k=0;
global i; i=0;
global gyro_up_peak; gyro_up_peak=0;
global gyro_down_peak; gyro_down_peak=0;
%Variable Set by the Training Algorithm
global threshold1; threshold1=0.05;
%Variable Used to Update Step Count
global maincount; maincount=0;
global gyro_count_enable;
gyro_count_enable=0;
%///////////////////End of Variable Definition/////////////////

%///////////////////Gyroscope-X axis Data Filtering with Butterworth Lowpass Filter at 2Hz/////////////////
for i=start:stop
in(1)=in(2);
in(2)=in(3); in(3)=in(4); in(4)=in(5); in(5)=in(6); in(6)=in(7); in(7)=Gyro(i);
Ou(1)=Ou(2); Ou(2)=Ou(3); Ou(3)=Ou(4); Ou(4)=Ou(5); Ou(5)=Ou(6); Ou(6)=Ou(7);
filt1(i)=((in(1)+in(7))*0.000000000853160)+((in(2)+in(6))*0.000000005118957 )+((in(3)+in(5))*0.000000012797393)+((in(4))*0.000000017063191 )+((Ou(1))*(- 0.7844171769))+((Ou(2))*(4.8969248914))+((Ou(3))*(- 12.7416173292))+((Ou(4))*(17.6873761799))+((Ou(5))*(- 13.8155108061))+((Ou(6))*(5.7572441862));
Ou(7)=filt1(i);
i=i+1;
%Detecting a Zero Cross
%Gyro Countdown
if((Ou(7)<0) && (Ou(6)>0)&& (gyro_count_enable==1)) counter1=1;
counter2=0;
var_k=i;
if(gyro_up_peak >threshold1) maincount=maincount+1;
end end
%Detecting a Zero Cross
%Gyro Countup
if((Ou(7)>0) && (Ou(6)<0) && (gyro_count_enable==1)) counter2=1;
counter1=0;
var_k=i;
if(gyro_down_peak <-threshold1) maincount=maincount+1;
end end
%Detecting Upward Peak
if((Ou(7)<Ou(6)) && (Ou(6)>Ou(5) && (counter2==1))) gyro_up_peak = Ou(6);
end
%Detecting Downward Peak
if((Ou(7)>Ou(6)) && (Ou(6)<Ou(5) && (counter1==1))) gyro_down_peak = Ou(6);
end
%Sample Time Out
if(((counter1==1)&&(i<var_k+15))||((counter2==1)&&(i<var_k+15))) gyro_count_enable=0
else
end
end
maincount
%//////////////////End Of The Main Algorithm//////////////////
%Plotting the Gyro-X axis data
figure;
plot(t,filt1);
grid;
xlabel('Time (s)'); ylabel('Gyro-X(rad/s)');
title('Gyro Sensor X-axis Reading');
```


<h1>Related papers</h1>

1. 2018 基于MEMS加速度传感器的步数检测算法研究综述: <a href="https://yonghong.github.io/file/2018基于MEMS加速度传感器的步数检测算法研究综述.pdf">PDF download</a>
2. 2017 A New Dataset for Evaluating Pedometer Performance: <a href="https://yonghong.github.io/file/22017-A-New-Dataset-for-Evaluating-Pedometer-Performance.pdf">PDF download</a>
3. 2016 Step Detection Algorithm For Accurate Distance Estimation Using Dynamic Step Length: <a href="https://yonghong.github.io/file/2016-Step-Detection-Algorithm-For-Accurate-Distance-Estimation-Using-Dynamic-Step-Length.pdf">PDF download</a>
4. 2016 Interrupt-Based Step-Counting to Extend Battery Life in an Activity Monitor: <a href="https://yonghong.github.io/file/2016-Interrupt-Based-Step-Counting-to-Extend-Battery-Life-in-an-Activity-Monitor.pdf">PDF download</a>
5. 2015 A DECISION TREE BASED PEDOMETER AND ITS IMPLEMENTATION ON THE ANDROID PLATFORM: <a href="../file/2015-A-DECISION-TREE-BASED-PEDOMETER-AND-ITS-IMPLEMENTATION-ON-THE-ANDROID-PLATFORM.pdf">PDF download</a>
6. 2014 RecoFit: Using a Wearable Sensor to Find, Recognize, and Count Repetitive Exercises： <a href = "https://yonghong.github.io/file/2014-RecoFit_Using-a-Wearable-Sensor-to-Find-Recognize-and-Count-Repetitive-Exercises.pdf">PDF Downlaod</a> or from <a href =https://www.microsoft.com/en-us/research/wp-content/uploads/2016/12/p3225-morris.pdf>online</a>.
7. 2013 Accurate Pedometer for Smartphones: <a href="https://yonghong.github.io/file/2013-Accurate-Pedometer-for-Smartphones.pdf">PDF download</a>
8. 2013 A Gyroscope based Accurate Pedometer Algorithm: <a href="https://yonghong.github.io/file/2013-A-Gyroscope-based-Accurate-Pedometer-Algorithm.pdf">English Version</a>, <a href="../file/2013-A-Gyroscope-Based-Accurate-Pedometer-Algorithm-Chinese-Version.pdf">Chinese Version</a>, <a href="https://yonghong.github.io/file/2013-A-Gyroscope-based-Accurate-Pedometer-Algorithm_MATLAB_File.pdf">Matlab Code</a>
9. Towards the run and walk activity classification
through step detection - An android application：<a href="https://yonghong.github.io/file/2012-Towards_the_run_and_walk_activity_classi.pdf">PDF download</a>
9.  2010 利用3轴数字加速度计实现功能全面的计步器设计: <a href="https://yonghong.github.io/file/2010-利用3轴数字加速度计实现功能全面的计步器设计.pdf">PDF download</a>
10. 2010 Full-Featured Pedometer Design Realized with 3-Axis Digital Accelerometer: <a href="https://yonghong.github.io/file/2010-Full-Featured-Pedometer-Design-Realized-with-3-Axis-Digital-Accelerometer.pdf">PDF download</a>
11. 2003 Comparison of Pedometer and Accelerometer Accuracy under Controlled Conditions: <a href = "https://yonghong.github.io/file/2003-Comparison-of-Pedometer-and-Accelerometer-Accuracy-under-Controlled-Conditions.pdf">PDF download</a>

<h1>Discuss</h1>
A Smartphone pedometer algorithm based on accelerometer is discussed by Oner et al. <a href="https://yonghong.github.io/file/2012-Towards_the_run_and_walk_activity_classi.pdf">[Towards the run and walk activity classification through step detection - An android application]</a> and their algorithm demonstrated sufficient accuracies at walking speeds higher than 90 beats per second (bps), but its performance degrades as speeds fall below 90bps. Their algorithm has over counted steps and the error was approximately 20% at 80 bps, 60% at 70 bps and 90% at 60 bps.


&emsp;&emsp;
<br/>