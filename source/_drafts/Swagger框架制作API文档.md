---
title: Swagger框架制作API文档
description: 介绍编写Swagger编写API文档的基本语法，及swagger-editor的简单使用
mathjax: true
date: 2018-04-22 00:01:41
tags:
	- RESTful
	- Swagger
categories:
	- 计算机科学
---

## 1. 基本概念 - RESTful

> references:
>
> [Representational State Transfer](https://zh.wikipedia.org/wiki/具象状态传输)
>
> [理解RESTful架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)
>
> [RESTful API设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)

### 1.1 名词解释

- RESTful（representational state transfer）是一种软件架构风格，目的是便于不同的软件或程序在一个网络中互相传递资源。


- resources：资源。网络上的一个资源就代表一个实体或信息；一个资源可以代表一个图片，一个文件，一种服务；每个资源都有一个URI（统一资源标识符）指向它，每个资源都对应一个特定的URI；获取某个资源的方法，即是访问这个资源的URI即可。*URI最常见形式是URL（统一资源定位符）*
- representation：表现层。资源是一种信息实体，对外可以有各种各样的形式表现，比如一段文本可以用html格式、xml格式、txt格式，这些形式就是表现层（明确的说，表现层代表资源实体的表现形式）；URI代表的是一个资源的ID，而没有包含表现层信息，所以表现层的具体表现形式应该是在HTTP请求头部的Accept和Content-Type字段，这两个字段才是对表现层的具体描述
- state transfer：状态转换。客户端和服务端的交互需要改变用户状态，而HTTP协议是无状态的（即对客户端操作得到的状态是无法通过HTTP协议保留的），所以状态都保留在服务端（用户状态的实质就是服务端的数据）。

> REST的名称由来
>
> REST-compliant web services allow the requesting systems to access and manipulate textual representations of [web resources](https://en.wikipedia.org/wiki/Web_resource) by using a uniform and predefined set of [stateless](https://en.wikipedia.org/wiki/Stateless_protocol) operations.
>
> 通过一组统一的、预先定义的无状态操作，REST风格的web服务允许请求系统（客户端）访问和操作文本（html，xml，json。。）表现形式的web资源
>
> 
>
> The term is intended to evoke an image of how a well-designed Web application behaves: it is a network of Web resources (a virtual state-machine) where the user progresses through the application by selecting links, such as `/user/tom`, and operations such as GET or DELETE (state transitions), resulting in the next resource (representing the next state of the application) being transferred to the user for their use.
>
> 
> 该术语旨在唤起设计良好的Web应用程序的行为形象：它是一个Web资源（虚拟状态机）的网络。在这个网络中，用户通过选择链接（例如/ user / tom）和诸如GET或DELETE（状态转换）之类的操作在应用程序中推进，导致下一个资源（表示应用程序的下一个状态）被传送给用户以供其使用。