---
title: Hough Transform
tags:
  - Computer Vision
  - Hough Transform
  - 笔记
categories:
  - 计算机科学
description: 霍夫变换是图像处理中的一种特征提取的技术
mathjax: true
date: 2018-04-12 10:46:37
---



## 1. 基础知识

 ### 1.1 curve fitting 和 interpolation

- 拟合
  - 用一个数学模型拟合离散数据的过程
  - 目标是为了获得一个数学模型与离散数据点之间的平均误差最小化
  - 所有数据点不一定刚好在这个数学模型上
- 插值
  - 绘制出一条光滑的曲线，保证所有数据点都在这条曲线上



### 1.2 最小二乘法

最小二乘法是用于近似求解方程组的线性代数方法

- 曲线拟合最小化平均误差
  - 等价于导函数为零
  - 等价于求解方程组
    - 使用最小二乘求最小二乘解：

$$
Ax = b \\
A^TA x = A^T b \\
(A^TA)' A^TAx = (A^TA)' A^T b  \\
x =  (A^TA)' A^T b 
$$

### 1.3 curve fitting运用于图像边缘建模

对已有的边缘图中的边缘像素点用直线建模

- 存在困难
  - 多条直线如何通过fitting拟合？哪些点属于要拟合的这个子集（枚举？不可能）
  - 噪声点对线性拟合的影响
  - 缺失点问题
- 解决问题思想：voting
  - 不可能穷尽所有子集去拟合直线
  - 用特征点去投票，投模型
  - 噪声也可能投票，但是投票是规则的，不影响



## 2. Hough Transform （霍夫变换）

### 2.1 image space 到 hough space

- image space：图像空间是指原图像的二维空间（用坐标$(x, y)$ 表示），对于image space中的某一条直线可以用$y = m_0 x + b_0$ 表示。
- hough space：霍夫空间，也叫参数空间，是指上述直线中$(m_0, b_0)$构成的空间
- 映射关系（当hough变换是在直线坐标系下时）：

| image space(x, y)    | hough space(m, b)     |
| -------------------- | --------------------- |
| 直线：$y = m_0x+b_0$ | 点：$(m_0, b_0)$      |
| 点：$(x_0, y_0)$     | 直线：$b = -x_0m+y_0$ |

hough空间的直线：理解成在image空间里过某一点可能的直线的所有可能$m, b$ 的点构成的直线

hough空间的直线交点：理解成image空间中分别过两个点的所有直线中，重合的情况，即image空间中两点确定一条线

### 2.2 hough变换可能的存在的问题 

如何表示image space中竖直的直线？

解决：hough space用极坐标参数，在image space用极坐标表示一条直线

极坐标系直线方程：$r = a*cos\theta + b* sin\theta$，表示过极坐标点$(r, \theta)$ ，且与极坐标点与原点的连线垂直的直线，则$(a, b)$可以当做hough 空间



### 2.3 hough变换机制

> image space是离散的，是一个个单位像素点组成的
>
> hough space同理，可以用一个个cell表示，初始为0

在image space（已经canndy处理），然后对每个边缘像素点hough变换得到对应曲线，在hough space中曲线位置的cell（像素点）的值+1处理。

最后hough space会得到一个灰度图，最亮的cell（像素点）就是一个peak，就是代表一个image space中的直线。

- Difficulties
  - hough space的cell多大（分辨率多大？）
  - 最终用多少条直线
    - 通过筛选灰度值高的peak
    - 对筛选后的点做聚类
  - 哪个点属于哪个直线
    - 选择哪些点里直线近，近的就确定直线，并去掉那些确定后的点
    - 选择剩下的点重新做hough