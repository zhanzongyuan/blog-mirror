---
title: CUDA和cuDNN
tags:
  - CUDA
  - cuDNN
  - GPU
categories:
  - 计算机科学
description: CUDA和cuDNN
mathjax: true
date: 2018-04-18 11:17:07
---



## 1. CUDA，cuDNN概念

> NVIDIA官网CUDA: http://www.nvidia.cn/object/cudazone-cn.html
>
> NVIDIA官网cuDNN: https://developer.nvidia.com/cuDNN
>
> NVIDIA官网深度学习SDK: https://developer.nvidia.com/deep-learning-software



### 1.1 深度学习SDK

> The NVIDIA Deep Learning SDK provides powerful tools and libraries for designing and deploying GPU-accelerated deep learning applications. It includes libraries for deep learning primitives, inference, video analytics, linear algebra, sparse matrices, and multi-GPU communications.

NVIDIA的深度学习SDK提供一些有力的工具和库用于GPU加速深度学习应用，包括一些库关于深度学习、推导、视频分享、线性代数、稀疏矩阵、和多GPU通信



### 1.2 cuDNN

> **Deep Learning Primitives (cuDNN)**: High-performance building blocks for deep neural network applications including convolutions, activation functions, and tensor transformations

一个高性能库为深度神经网络应用包括卷积、激活函数、张量转换等



### 1.3 CUDA

> The Deep Learning SDK requires [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit), which offers a comprehensive development environment for building new GPU-accelerated deep learning algorithms, and dramatically increasing the performance of existing applications 


深度学习SDK需要CUDA工具包，它提供了一个全面的开发环境，用于构建新的GPU加速深度学习算法，并显着提高现有应用程序的性能。深度学习SDK需要CUDA工具包，它提供了一个用于构建新GPU的全面开发环境加速的深度学习算法，并显着提高现有应用程序的性能



### 1.4 题外话

> SDK : **软件开发工具包**（**S**oftware**D**evelopment**K**it,**SDK**）一般是一些被软件工程师用于为特定的[软件包](https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E5%8C%85)、[软件框架](https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E6%A1%86%E6%9E%B6)、硬件平台、[作业系统](https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E4%BD%9C%E6%A5%AD%E7%B3%BB%E7%B5%B1)等创建应用软件的开发工具的集合。
>
> API：**应用程序接口**（英语：**A**pplication**P**rogramming**I**nterface，简称：**API**），又称为**应用编程接口**，就是[软件](https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6)系统不同组成部分衔接的约定。
>
> Library:用于开发[软件](https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6)的[子程序](https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E5%AD%90%E7%A8%8B%E5%BA%8F)集合。库和[可执行文件](https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E5%8F%AF%E6%89%A7%E8%A1%8C%E6%96%87%E4%BB%B6)的区别是，库不是独立[程序](https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E7%A8%8B%E5%BA%8F)，他们是向其他程序提供服务的代码。
>
> Framework:通常指的是为了实现某个业界标准或完成特定基本任务的[软件组件](https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E8%BB%9F%E4%BB%B6%E7%B5%84%E4%BB%B6)规范，也指为了实现某个软件组件规范时，提供规范所要求之基础功能的软件产品。
>
> **toolkit** (*plural* **toolkits**)
>
> 1. An [assembly](https://en.wiktionary.org/wiki/assembly) of [tools](https://en.wiktionary.org/wiki/tools).
> 2. ([computing](https://en.wiktionary.org/wiki/computing)) A set of basic [components](https://en.wiktionary.org/wiki/component) for developing [software](https://en.wiktionary.org/wiki/software).



### 1.5 CUDA vs cuDNN


- CUDA（Compute Unified Device Architecture），是显卡厂商NVIDIA推出的运算平台。 CUDA™是一种由NVIDIA推出的通用并行计算架构，该架构使GPU能够解决复杂的计算问题。
- cuDNN是专门针对Deep Learning框架设计的一套GPU计算加速方案，目前支持的DL库包括Caffe，ConvNet, Torch7等。
- 两者关系：cuda针对并行计算加速，cudnn针对神经网络的计算加速，cudnn需要在有cuda的基础上进行。


## 2. 安装

> 这里不做安装教程了，网上具体都有
>
> 官网CUDA安装教程：https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html
>
> 官网cuDNN安装教程：https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html

```bash
nvcc -V # 集群cuda驱动检查安装
```



## 3. 使用

http://pytorch.org/docs/master/notes/cuda.html