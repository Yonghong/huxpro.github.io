---
layout:     post
title:      "Step Counter on Bosch BMI-160 Sensor"
subtitle:   "independent implementation guide on MCU"
date:       2020-11-12 17:18:00
author:     "Dr.X"
header-img: "img/bmi160.jpg"
commenting: open
tags:
    - hardware
    - research
---

<h1>Step Counter Based on Bosch BMI-160</h1>

<h2>博世BMI160自带的计步功能</h2>

博世的传感器BMI160自带的计步功能，使用传感器自带的功能具有以下优势：

1. 功耗低：不超过20μA；
2. 不需要额外的算法开发和验证：只需要将硬件和参数做好配置即可，无需自己写代码收集数据并进行计算；
3. 稳定性高：内置于传感器中的计步算法，经过长时间和大量测试，稳定性高。

<h2><a href = "https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bmi160-ds000.pdf">BMI160规格书</a>第28页2.6.3节Step Detector（Accel）</h2>

为了获得鲁棒的走步检测结果，需要配置合加速度的最小阈值min_threshold（超过这个阈值才会考虑开始检测是否走步）和两个连续峰值之间的最少延迟min_steptime（连个连续峰值之间的时间间隔超过这个阈值才会被认为走了一步），基于这2个参数的不同配置，可以形成3种不同的走步检测模式：

- 普通模式：默认设置，适应于大多数情况
- 敏感模式：专门针对体重较轻、身高较低的人
- 鲁棒模式：针对有较多误检（将不是走步记为走步）的情况

更多细节信息可以参考寄存器(0x7A-0x7B)STEP_CONF以及相应的计步器应用。走步检测器触发计步器进行计步。更多细节请参考2.7节。

<h2><a href = "https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bmi160-ds000.pdf">BMI160规格书</a>第41页2.7节Step Counter</h2>

计步器通过累加走步检测中断进行计步。基于更加复杂的算法，计步器具有比走步检测器更高的准确性和比走步检测中断更大的延迟。

计步器可以通过寄存器(0x7A-0x7B)STEP_CONF中的step_cnt_en进行使能和重置。计步器与走步检测中断寄存器(0x7A-0x7B)STEP_CONF共享配置参数。

<h2>寄存器图</h2>

<img src="/img/bmi160-register.png"  alt="Register Map" />


<h3>(0x7A-0x7B)STEP_CONF寄存器</h3>

- 包含2个字节：
  - (0x7A)STEP_CONF_0和
  - (0x7B)STEP_CONF_1
- 寄存器地址：0x7A（2个字节）
- 重置：na
- 模式：R
- 描述：包含走步检测相关参数配置
- 具体定义如下：

<img src="/img/bmi160-STEP-CONF-0-0x7A.png"  alt="Register STEP_CONF" />

<img src="/img/bmi160-STEP-CONF-1-0x7B.png"  alt="Register STEP_CONF" />

<h4>寄存器(0x7A)STEP_CONF[0]</h4>

<table border="1">
  <tr>
    <th>bit</th>
    <th>7</th>
    <th>6</th>
    <th>5</th>
    <th>4</th>
    <th>3</th>
    <th>2</th>
    <th>1</th>
    <th>0</th>
  </tr>
  <tr>
    <td>0x7A</td>
    <td></td>
    <td></td>
    <td></td>
    <td bgcolor=yellow>min_threshold</td>
    <td bgcolor=yellow>min_threshold</td>
    <td bgcolor=lightgreen>steptime_min</td>
    <td bgcolor=lightgreen>steptime_min</td>
    <td bgcolor=lightgreen>steptime_min</td>
  </tr>
</table>

- steptime_min: 选择最小可检测的走一步的时间间隔，偏置（offset）为1。占三个二进制位(0x7A低三位)：steptime_min[0], steptime_min[1], steptime_min[2]。
- min_threshold：检测走步的最小合加速度，占两个二进制位（0x7A低第四和第五位）：min_threshold[0]和min_threshold[1]。

<h4>寄存器(0x7B)STEP_CONF[1]</h4>

<table border="1">
  <tr>
    <th>bit</th>
    <th>7</th>
    <th>6</th>
    <th>5</th>
    <th>4</th>
    <th>3</th>
    <th>2</th>
    <th>1</th>
    <th>0</th>
  </tr>
  <tr>
    <td>0x7B</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td bgcolor=yellow>step_cnt_en</td>
    <td bgcolor=lightgreen>min_step_buf</td>
    <td bgcolor=lightgreen>min_step_buf</td>
    <td bgcolor=lightgreen>min_step_buf</td>
  </tr>
</table>

- min_step_buf：最小步数缓冲区大小。用于减少开始走步之前的错误计步。如果检测到真实的走步，这里面的步数将会计入总步数中。因此，走步的时候，最初的步数输出值最小为min_step_buf。
有三种模式的计步设置，从敏感（假阴性false negative）到鲁棒（假阳性false positive）进行平衡。
    - 正常模式
        - STEP_CONF[0]: 0x15 (0b0001 0101)
        - STEP_CONF[1]: 0x03 (0b0000 0011) (the step_cnt_en bit is set to 0)
    - 敏感模式（更少假阴性，但更多假阳性）
        - STEP_CONF[0]: 0x2D (0b0010 1101)
        - STEP_CONF[1]: 0x00 (0b0000 0000) (the step_cnt_en bit is set to 0)
    - 鲁棒模式（更少假阳性，但更多假阴性）
        - STEP_CONF[0]: 0x1D (0b0001 1101)
        - STEP_CONF[1]: 0x07 (0b0000 0111) (the step_cnt_en bit is set to 0)
- 计步器寄存器可以从寄存器(0x78-0x79)STEP_CNT读取;
- 计步器可以通过发送0xB2到寄存器(0x7E)CMD进行重置。

<h3>寄存器(0x78-0x79)STEP_CNT</h3>

<table border="1">
  <tr>
    <th>bit</th>
    <th>7</th>
    <th>6</th>
    <th>5</th>
    <th>4</th>
    <th>3</th>
    <th>2</th>
    <th>1</th>
    <th>0</th>
  </tr>
  <tr>
    <td>0x79</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
  </tr>
  <tr>
    <td>0x78</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
    <td bgcolor=yellow>step_cnt</td>
  </tr>
</table>

- 地址：0x78（2字节）
- 重置：[0]0b0000-0000; [1]0b0000-0000
- 模式：R
- 描述：包含步数
- 定义：

<img src="/img/bmi160-STEP-CNT-0-0x78.png"  alt="Register STEP_CNT" />

<img src="/img/bmi160-STEP-CNT-1-0x79.png"  alt="Register STEP_CNT" />

- step_cnt:自从上次POR（Power-On Reset）或者步数重置后的总步数。


<h4>寄存器(0x7A)STEP_CONF[0]</h4>

<table border="1">
  <tr>
    <th>bit</th>
    <th>7</th>
    <th>6</th>
    <th>5</th>
    <th>4</th>
    <th>3</th>
    <th>2</th>
    <th>1</th>
    <th>0</th>
  </tr>
  <tr>
    <td>item</td>
    <td></td>
    <td></td>
    <td></td>
    <td bgcolor=yellow>min_threshold</td>
    <td bgcolor=yellow>min_threshold</td>
    <td bgcolor=lightgreen>steptime_min</td>
    <td bgcolor=lightgreen>steptime_min</td>
    <td bgcolor=lightgreen>steptime_min</td>
  </tr>
</table>

- steptime_min: 选择最小可检测的走一步的时间间隔，偏置（offset）为1。占三个二进制位(0x7A低三位)：steptime_min[0], steptime_min[1], steptime_min[2]。
- min_threshold：检测走步的最小合加速度，占两个二进制位（0x7A低第四和第五位）：min_threshold[0]和min_threshold[1]。

<h4>寄存器(0x7E)CMD</h4>

- 寄存器名称：(0x7E)CMD
- 地址： 0x7E
- 重置：na
- 模式：R
- 描述：命令寄存器触发类似于软重置、NVM（Non-volatile Memory，固定存储器，非易失存储器）编程等的操作等。
- 定义

<img src="/img/bmi160-CMD-0x7E.png"  alt="Register CMD" />

<table border="1">
  <tr>
    <th>bit</th>
    <th>7</th>
    <th>6</th>
    <th>5</th>
    <th>4</th>
    <th>3</th>
    <th>2</th>
    <th>1</th>
    <th>0</th>
  </tr>
  <tr>
    <td>item</td>
    <td bgcolor=yellow>cmd</td>
    <td bgcolor=yellow>cmd</td>
    <td bgcolor=yellow>cmd</td>
    <td bgcolor=yellow>cmd</td>
    <td bgcolor=yellow>cmd</td>
    <td bgcolor=yellow>cmd</td>
    <td bgcolor=yellow>cmd</td>
    <td bgcolor=yellow>cmd</td>
  </tr>
</table>

命令执行期间，寄存器(0x7E)CMD被占用，占用期间的所有写入都被丢弃，当有写入被丢弃时，设置寄存器(0x02) ERR_REG的值drop_cmd_err。

用于软重置的时间需要考虑300μs的额外系统启动时间，以用于PMU切换。

重置计步器：

CMD：step_cnt_clr: 0xB2

触发计步器计数重置，适应于所有的三种计步模式。

<h2> 总结</h2>
设置好计步器的参数以后（三个模式选择一个），即可在需要的时候从步数寄存器中读取当前时间段的步数，并在适当的时候重置步数寄存器。



<h2>附录</h2>

1. The paper <a href="https://media.digikey.com/pdf/Data%20Sheets/DFRobot%20PDFs/SEN0250_Web.pdf">Gravity: BMI160 6-Axis Inertial Motion
Sensor SKU: SEN0250</a> provided by DFROBERT has step count algorithm embeded. 
The library is uploaded in Github <a href = "https://github.com/DFRobot/DFRobot_BMI160">DFRobot/DFRobot_BMI160</a> which includes step count algorithm. The official business website of DFRobot is <a href = "https://wiki.dfrobot.com/Gravity__BMI160_6-Axis_Inertial_Motion_Sensor_SKU__SEN0250">HERE</a>.
The BMI160 datasheet is <a href = "https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bmi160-ds000.pdf">HERE</a>, which describes that there is a step detector and step counter in the sensor.

2. BMI160 has its own step counter, and the algorithm has already embedded into the sensor. To enable the step counter function, the only thing one need to do is to read out the step number from the sensor. The UNO Arduino library has been developed and is open sourced. There are two open source libraries:
   1. <a href = "https://github.com/Yonghong/DFRobot_BMI160">The *DFRobot_BMI160* Library</a>.
   2. <a href = "https://github.com/Yonghong/MH-BMI160">The *MH-BMI160* Library</a>




&emsp;&emsp;
<br/>