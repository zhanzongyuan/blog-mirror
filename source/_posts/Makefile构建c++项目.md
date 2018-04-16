---
title: Makefile构建c++项目
date: 2018-04-10 18:21:43
tags:
    - makefile
    - g++
    - gcc
categories:
    - 计算机科学
description: 重新写makefile，重新了解g++的编译链接过程
---

> 做计算机视觉作业时，要用到CImg库，发现居然Xcode不能链接到CImg头文件里面引用的X11链接库，于是在网上找了各种方法。最终重新捡起Makefile搭建c++项目。

## 1. g++

说到g++，这里我们先解释gcc：

### 1.1 gcc是啥

> gcc : GNU CC(简称gcc)是GNU项目中符合ANSI C标准的编译系统，能够编译用C、C++、Object 
> C、Jave等多种语言编写的程序。gcc又可以作为交叉编译工具，它能够在当前CPU平台上为多种不同体系结构的硬件平台开发软件，非常适合在嵌入式领域的开发编译，如常用的arm-linux-gcc交叉编译工具
>
> 参考：https://blog.csdn.net/zhaoyue007101/article/details/7699554
>
> 简而言之gcc是个编译器

### 1.2 gcc和g++

引用一段知乎的回答：

> gcc 最开始的时候是 GNU C Compiler,  如你所知，就是一个c编译器。但是后来因为这个项目里边集成了更多其他不同语言的编译器，GCC就代表 the GNU Compiler Collection，所以表示一堆编译器的合集。 g++则是GCC的c++编译器。作者：李锋链接：https://www.zhihu.com/question/20940822/answer/16667772来源：知乎著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### 1.3 gcc 参数

> 对于c++工程而言，gcc就是调用的g++编译器
>
> 对于普通的c工程，gcc用的不是g++编译器，由于g++编译器也会把c文件当作c++文件编译，只是链接的时候链接库不同罢了

命令格式：`gcc [option] [file]`

#### 1.3.1 详细编译流程

1.预处理-Pre-Processing

```
gcc  -E  hello_world.c  -o  hello_world.i    //.i文件
```

2.编译-Compiling

```
gcc  -S  hello_world.i  -o   hello_world.s  //.s文件
```

3.汇编-Assembling     

```
gcc  -c  hello_world.s  -o  gcc  -c  hello_world.s  -o  hello_world.o     //.o文件
```

4.链接-Linking          

```
gcc  hello_world.o  -o  hello_world //bin文件，可执行文件
```

#### 1.3.2 项目惯用流程

1. 编译

```
gcc -c hello_world.c -c hello_world.o
```

2. 链接

```
gcc hello_world.o -c hello_world
```

> 对于c++项目是一样命令，只是gcc改成g++

#### 1.3.3 参数解释

> 大小写有区分

- -E参数
  - -E 选项指示编译器仅对输入文件进行预处理。当这个选项被使用时, 预处理器的输出被送到标准输出而不是储
- -S参数
  - -S 编译选项告诉 GCC 在为 C 代码产生了汇编语言文件后停止编译。 GCC 产生的汇编语言文件的缺省扩展名
- -c参数
  - -c 选项告诉 GCC 仅把源代码编译为目标代码。缺省时 GCC 建立的目标代码文件有一个 .o 的扩展名。
- -o参数
  - -o 编译选项来为将产生的可执行文件用指定的文件名。
- -O参数
  - -O 选项告诉 GCC 对源代码进行基本优化。这些优化在大多数情况下都会使程序执行的更快。 -O2 选项告诉：
    - GCC 产生尽可能小和尽可能快的代码。 如-O2，-O3，-On（n 常为0--3）；
    - -O  主要进行跳转和延迟退栈两种优化；
    - -O2 除了完成-O1的优化之外，还进行一些额外的调整工作，如指令调整等。
    - -O3 则包括循环展开和其他一些与处理特性相关的优化工作。
    - 选项将使编译的速度比使用 -O 时慢， 但通常产生的代码执行速度会更快。
- 调试选项-g和-pg
  - GCC 支持数种调试和剖析选项，常用到的是 -g 和 -pg 。
  - -g 选项告诉 GCC 产生能被 GNU 调试器使用的调试信息以便调试你的程序。GCC 提供了一个很多其他 C 编译器里没有的特性, 在 GCC 里你能使-g 和 -O (产生优化代码)联用。
  - -pg 选项告诉 GCC 在编译好的程序里加入额外的代码。运行程序时, 产生 gprof 用的剖析信息以显示你的程序的
- -l参数和-L参数（前面是小写的L）
  -  -l参数就是用来指定程序要链接的库，-l参数紧接着就是库名，那么库名跟真正的库文件名有什么关系呢？-L指定库文件的所在的目录，结合-L就可以链接第三方库文件。
  - pthread例子：`-lpthread`
  - CImg的例子：` -L./src/lib/X11/lib -lX11`，这个表示在`#include<X11>`的时候用`./src/lib/X11/lib`里的库文件libX11，一般在链接的时候用
- -I（这是大写的i）
  - 指定头文件

> 最后留几个问题：
>
> 交叉编译？GNU？
>
> 链接库和引用头文件的区别

## 2. Makefile

语法：

```
target: [target1] [target2] ....
	command 1
	command 2
```

target是指具体某个文件，或者是某个命令的变量名如：run

这里相当于target0需要target1和target2等依赖，必须先运行target1，target2下的命令才能运行target0

command是shell命令，这里可以直接`g++ -c xxx.cpp -o xxx.o`, `g++ xxx.o -o xxx`

> 待续：
>
> 文件不更新时，不会重复编译？
>
> Makefile 的宏的高级用法

## 3. C++项目结构

以Computer Vision的项目为例

```
.
├── Makefile
├── build
│   ├── canny.o
│   └── cv_hw2.o
├── docs
│   ├── Ex2.docx
├── images
│   ├── bigben.jpg
│   ├── bmp
│   ├── lena.jpg
│   ├── stpietro.jpg
│   └── twows.jpg
├── output
│   ├── bigben-edge.bmp
│   ├── lena-edge.bmp
│   ├── stpietro-edge.bmp
│   └── twows-edge.bmp
└── src
    ├── cv_hw2
    ├── cv_hw2.cpp
    ├── include
    ├── lib
    └── test
```

