---
title: session和cookie
tags:
  - session
  - cookie
categories:
  - 计算机科学
description: 解决HTTP协议无状态而提出的解决方案
mathjax: true
date: 2018-04-13 20:44:29
---

## 1. cookie

**概念：**  服务器和客户端直接维持状态的解决方案

- HTTP无状态：指的是每次客户端向服务器发送HTTP方法的时候，对于服务器而言都是一次新的操作，之前的操作不会对后面的操作有任何影响。都是把每个请求当作一个新的请求。

> 打个比方：
>
> 无状态：每次去同一家餐厅吃饭，无状态的店员都把你当作新客人，每次都重新推荐菜品
>
> 有状态：每次去同一家餐厅吃饭，有状态的店员对你之前来过有印象，就当作老顾客，能熟络的叫出你的名字，甚至都知道你喜欢的哪几道菜

**目的：**cookie的出现就是为了简单的保留你的登录状态，而不用每次都重复登录

**特点：**

- 状态（历史操作记录，cookie）数据保留在客户端
- 分为会话cookie和持久cookie

**机制：**

![cookie](session和cookie/cookie.png)



## 2. session

> session同样是解决HTTP无状态问题的机制，只是将历史数据保留在了服务器端，由服务器来管理

**概念：** 服务器和客户端直接维持状态的解决方案，有时也指这种解决方案的存储结构（与cookie不同在于状态信息管理的位置不同，一个在服务器端，一个在客户端）

**特点：**

- sessionId是全局的、唯一的
- 一个客户对应一个sessionId
- Session在服务端器存储，管理
- Session是有生命周期的，会被产生，摧毁

**机制：**



![session](session和cookie/session.png)



> references : https://github.com/astaxie/build-web-application-with-golang/blob/master/zh/preface.md

