---
title: Halstead 复杂度
tags:
  - Halstead
  - 软件测试
categories:
  - 计算机科学
description: Halstead 复杂度
mathjax: true
date: 2018-04-22 09:38:41
---



## 1. Halstead

### 1.1 概念

Halstead complexity measures（霍尔斯特德软件复杂度度量方法）是一种软件度量方法。

> 霍尔斯特德：软件复杂度度量，应该要反映不同的程序语言中算法的实现方式，又要独立于使用的平台语言。这些度量可以由静态代码中的计算而得。

### 1.2 计算

Halstead根据语句行的操作符和操作数的数量计算程序复杂度

- 操作符和操作数的数量越大，程序结构越复杂
  - **操作符**包括语言**保留字**、**程序调用**、**数学运算符**、以及有关**分隔符**
  - **操作数**可以是**常数**和**变量**
- Halstead复杂度度量
  - 设$n_1$表示程序中不同的操作符个数，$n_2$表示不同操作数的个数，$N_1$表示程序中出现的操作符的总数，$N_2$表示程序中出现操作数的总数
  - Halstead 程序词汇表长度 Program vacabulary: $n = n_1 + n_2$
  - 实际Halstead长度 Program length: $N = N_1 + N_2$ 
  - 以N^表示程序的预测长度 Calculated program length: N^ $= n_1 log_2(n_1) + n_2 log_2(n_2) $ 
  - Halstead的重要结论之一是：程序的时间长度N和预测长度N^非常接近，这表明即时程序还未编写完y呢预先估算出程序的实际长度N
  - 其他计算公式
    1. 程序的体积，容量 Volume: $V = (N) log_2(n)$ ，表示程序的词汇复杂度
    2. 程序级别 Level: $\text {L^} = (2/n_1) \times (n_2/N_2)$ ，表示程序最紧凑形式的程序量和实际程序量的比，反映程序的效率
    3. 程序难度 Difficult: $D = 1 / \text {L^}$ ， 表示程序算法的困难程度
    4. 编程工作量 Effort: $ E = V \times D = V / \text {L^} $ 
    5. 智能级别 $I = \text {L^} \times E$ 
    6. 语言级别 $ L' = \text {L^} \times \text {L^} \times V$
    7. 编程时间(hours) $\text{T^} = E / (S\times f), \ \ \  S = 60\times 60, f = 18$  
    8. 平均语句大小 $N/语句数$
    9. 程序错误预测值 $ B = N \times log_2 (n_1 + n_2)/3000$ 
- 缺点
  - 仅考虑程序数据量和程序体积，不考虑程序控制流的情况。
  - 不能从根本上反映程序复杂性。

## 2. Example

- 计算下列程序的 Halstead 复杂度的10项内容

```c++
#include <stdio.h>
#include <math.h>
int main() {
    float a, b, c, mean;
    scanf("%f %f %f", &a, &b, &c);
    mean = a * b * c;
    mean = pow(mean, 1.0 / 3.0);
    printf("Geometric Mean = %f", mean);
    return 0;
}
```

| 指标                                                         | 数值          | 项                                                           |
| ------------------------------------------------------------ | ------------- | ------------------------------------------------------------ |
| 不同的操作符个数 $n_1$                                       | 16            | `#,include, <>, int, main, (), {}, float, scanf, &, =, *, pow, /, printf, return` |
| 不同的操作数个数 $n_2$                                       | 11            | `stdio.h, math.h, a, b, c, mean, "%f %f %f", 1.0, 3.0, "Geometric Mean = %f", 0` |
| 操作符个数 $N_1$                                             | 26            | -                                                            |
| 操作数个数 $N_2$                                             | 21            | -                                                            |
| 程序词汇表长度 $n = n_1 + n_2$                               | 27            | -                                                            |
| 简单长度 $N = N_1 + N_2$                                     | 47            | -                                                            |
| 程序预测长度 $\text{N^} = n_1log(n_1) + n_2log(n_2)$         | 102.053747805 | -                                                            |
| 程序的体积，容量 Volume: $V = (N) log_2(n)$                  | 67.2740969155 | -                                                            |
| 程序级别 Level: $\text {L^} = (2/n_1) \times (n_2/N_2)$      | 0.06547619047 | -                                                            |
| 程序难度 Difficult: $D = 1 / \text {L^}$                     | 15.2727272727 | -                                                            |
| 编程工作量 Effort: $ E = V \times D = V / \text {L^} $       | 1027.458993   | -                                                            |
| 智能级别 $I = \text {L^} \times E$                           | 67.2740969155 | -                                                            |
| 语言级别 $ L' = \text {L^} \times \text {L^} \times V$       | 0.2842        | -                                                            |
| 编程时间(hours) $\text{T^} = E / (S\times f), \ \ \  S = 60\times 60, f = 18$ | 0.01585585    | -                                                            |
| 平均语句大小=$N/语句数$                                      | 5.222222222   | -                                                            |
| 程序错误预测值 $ B = N \times log_2 (n_1 + n_2)/3000$        | 0.07449323753 | -                                                            |

