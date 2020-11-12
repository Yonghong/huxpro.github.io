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
    * The related paper: <a href ="https://www.microsoft.com/en-us/research/wp-content/uploads/2016/12/p3225-morris.pdf">2014 RecoFit: Using a Wearable Sensor to Find, Recognize, and Count Repetitive Exercises</a>, or <a href = "https://yonghong.github.io/file/2014-RecoFit_Using-a-Wearable-Sensor-to-Find-Recognize-and-Count-Repetitive-Exercises.pdf">direct download</a>.
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

Zero Crossing With Signal Strength (<a href = "https://yonghong.github.io/file/2013-A-Gyroscope-based-Accurate-Pedometer-Algorithm_MATLAB_File.pdf">MATLAB code</a>)

<h1>Hardware and applications</h1>

1. The paper <a href="https://media.digikey.com/pdf/Data%20Sheets/DFRobot%20PDFs/SEN0250_Web.pdf">Gravity: BMI160 6-Axis Inertial Motion
Sensor SKU: SEN0250</a> provided by DFROBERT has step count algorithm embeded. 
The library is uploaded in Github <a href = "https://github.com/DFRobot/DFRobot_BMI160">DFRobot/DFRobot_BMI160</a> which includes step count algorithm. The official business website of DFRobot is <a href = "https://wiki.dfrobot.com/Gravity__BMI160_6-Axis_Inertial_Motion_Sensor_SKU__SEN0250">HERE</a>.
The BMI160 datasheet is <a href = "https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bmi160-ds000.pdf">HERE</a>, which describes that there is a step detector and step counter in the sensor.

2. BMI160 has its own step counter, and the algorithm has already embedded into the sensor. To enable the step counter function, the only thing one need to do is to read out the step number from the sensor. The UNO Arduino library has been developed and is open sourced. There are two open source libraries:
   1. <a href = "https://github.com/Yonghong/DFRobot_BMI160">The *DFRobot_BMI160* Library</a>.
   2. <a href = "https://github.com/Yonghong/MH-BMI160">The *MH-BMI160* Library</a>

- about BMI160 hardware settings:
  - 2.6.3 Step Detector (Accel)
    A step detection is the detection of a single step event, while the user is walking or running. The step detector is triggered when a peak is detected in the acceleration magnitude (vector length of 3D acceleration). In order to achieve a robust step detection the peak needs to exceed a configurable threshold *min_threshold* and a minimum delay time *min_steptime* between two consecutive peaks needs to be observed. 
    The step detector can be configured in three modes:
    * Normal mode (default setting, recommended for most applications)
    * Sensitive mode (can be used for light weighted, small persons)
    * Robust mode (can be used, if many false positive detections are observed)

    More details can be found in Register (0x7A-0x7B) STEP_CONF and the according step counter application note.
    The step detector is the trigger for a step counter. 
  - 2.11.37 Register (0x7A-0x7B) STEP_CONF
    ADDRESS 0x7A (2 byte)
    RESET na
    MODE R
    DESCRIPTION Contains configuration of the step detector DEFINITION

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