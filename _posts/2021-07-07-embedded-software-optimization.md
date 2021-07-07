---
layout:     post
title:      "About Wearable Device Algorithm Code Optimization"
subtitle:   "embedded, algorithm, optimization, etc., wearable devices fall detection "
date:       2021-07-07 18:18:18
author:     "Dr.X"
header-img: "img/20210707softwareoptimization.jpg"
commenting: open
tags:
    - embedded software
    - optimization
---

<h1> Embedded Fall Detect Algorithm Optimization </h1>

reference: [https://blog.csdn.net/KingMingle/article/details/107356011](https://blog.csdn.net/KingMingle/article/details/107356011)

<h2> 一. 算法优化原则</h2>

1. 等效原则：优化前后程序实现的功能一致；

2. 有效原则：优化后要比优化前运行速度快或占用存储空间小，或二者兼有；

3. 经济原则：优化程序要付出较小的代价，取得较好的结果。

<h2>二. 算法优化方法</h2>

1. 系统优化
- 编译器优化等级配置（-O0/-O1/-O2/-O3）
- 流水线多线程结构（pipeline）

1. 算法优化（需要理解算法原理）
- 使用快速算法
- 算法流程和结构调整

3. 代码优化
- 定点化（Q格式/乘以10x）
- 查表
- 除法优化（改为移位/避免多次计算同一除法）
- 尽量减少if…else
- 数据拷贝优化（使用快速拷贝算法/使用指针）
- 函数调用参数个数尽量不超过４个
> ARM调用时，4个以下的形参通过寄存器传递，第5个以上的形参通过存储器栈传递。如果有更多的参数调用，则可将相关的参数组织在一个结构体内，用传递结构体指针来代替参数。
- switch语句用法的优化
> 对case值按照可能性排序，将最可能发生的情况放在第一个，最不可能的情况放在最后一个，可以提高switch语句块的执行速度。
- 少用全局变量，多用局部变量
> 全局变量是放在数据存储器中的，定义了全局变量，MCU就少了一个可以利用的数据存储器空间，太多的全局变量，会导致编译器 无足够的内存分配；而局部变量则大多定位于MCU内部的寄存器中。在绝大多数的MCU中，使用寄存器的操作速度比数据存储器快，指令也更灵活，有利于生成质量更高的代码，而且局部变量所占用的寄存器和数据存储器在不同的模块中可以重复利用。
- 尽量使用小的数据类型
> 在所定义的变量满足使用要求的条件下，优先使用顺序为：字符型(char)>整型(int)>长整型(long int)>浮点型(float)。对除法来说，使用无符号数比有符号数会有更高的效率。在实际调用中，尽量减少数据类型的强制转换；少用浮点运算，如果运算的结果能够控制在误差之内，则可用长整型代替浮点型。
- 自加自减
> 例：
优化前a=a+1、a=a-1
优化后++a、–ａ

4. 使用硬件资源（需要熟悉芯片架构及资源）
- 多核（多arm核/多DSP、多GPU）
- 多线程
- 芯片自带硬件算法（海思IVE等）
- 异构芯片的单个DSP/GPU
- 快速存储器（cache/DTCM/ITCM/CMX/三级缓存/乒乓buffer）+DMA

5. 汇编
- 伪汇编（intrinsic asm/VPU向量处理单元）（Neon指令/Movidius伪汇编指令）
> If…else优化：判断并行的多个数据是否同时满足条件，若满足则再做相同的处理，若不满足，则以条件的结果为系数乘以算法处理的结果。
- 汇编（最后选择）