---
title: shadowsocks-libev+CentOS 7 搭梯
tags:
  - shadowsocks
  - VPS
categories:
  - 计算机科学
description: 搭一次，忘一次。。。
date: 2018-02-01 20:51:45
---



## 一、VPS

>  供应商：[Vultr](https://www.vultr.com)、[DigitalOcean](https://www.digitalocean.com)

1. [Vultr](https://www.vultr.com)

优点：

- 节点位置多
- 提供节点[测速](https://www.vultr.com/faq/#downloadspeedtests)
- 邀请码优惠：老推新，新人享10刀
- 最低消费：2.5 dollars per month（虽然都显示告罄，还是有5刀的可以选）
- 可以支付宝

缺点：

- 首次必须充值最低10刀

个人经验：

> 不要相信别人说的亚洲节点好，其实在Vultr里亚洲节点最不稳定
>
> 实际性能还是要自己测试还是需要自己测试

- 尽量一个节点多测试几次，一个是看时延长短，一个是看时延波动情况
- Silicon Valley（硅谷）节点时延几乎最低，最稳定，稳定在170+ms
- 新人通过老用户的链接获取的10刀必须在账户首次充值后有效，首次充值最低10刀，所以天下没有免费的午餐

<br>

2. [DigitalOcean](https://www.digitalocean.com)

优点：

- 邀请码：老推新，新人享10刀
- [GitHub学生认证大礼包](https://education.github.com/pack)特惠码：50刀（[各种学生优惠大礼包](https://github.com/ivmm/Student-resources)）
- 可以支付宝
- 首次必须充值最低5刀

缺点：

- 几乎所有节点都是延时250+ms，除了旧金山节点可以进到200ms内，但是不稳定
- 及其不稳定，一个节点的时延波动极大，实测可能还会丢包
- 一旦一个邀请码或特惠码使用后不能用第二个特惠码，即老用户邀请的新用户无法使用更GitHub学生大礼包的50刀
- VPS系统内核升级很难

<br>

个人经验：

- 不是很推荐，除了50刀诱惑人（可以用一年）
- 我可是连续开了三个号才弄清GitHub学生大礼包的优惠码不能喝邀请码同时使用

<br>

3. 系统选择

尝试了CentOS 7，“据说”比较稳定。

<br>

4. 总结

>  偏向于Vultr，便宜而且部分节点很稳。DigitalOcean除了有大礼包吸引我，就没别的了🤦‍♀️

**个人推荐1：Vultr + CentOS 7 + Silicon Valley**

**个人推荐2：Vultr (CentOS 7 + Silicon Valley) + DigitalOcean (CentOS 7 + San Francisco) ** *由于可能容易被墙识别然后block，就用大礼包的免费50刀开个备用线路*



## 二、CentOS 7 + shadowsocks-libev

> 集各家之精华
>
> Reference：
>
> [shadowsocks.org](http://shadowsocks.org/en/download/servers.html)
>
> [GitHub](https://github.com/shadowsocks/shadowsocks-libev#os-x)
>
> [CentOS 7安装ShadowSocks（libev版本）加上KCP加速](https://www.racecoder.com/index.php/archives/218/)
>
> [CentOS 7 配置 shadowsocks-libev 服务器端进行科学上网](https://roxhaiy.wordpress.com/2017/08/04/430/)
>
> [CentOS 7 下安装 Shadowsocks 服务端](https://zzz.buzz/zh/gfw/2017/08/14/install-shadowsocks-server-on-centos-7/)
>
> [CentOS 搭建 Shadowsocks-libev 环境](https://blog.ikke.moe/posts/centos-shadowsocks-libev-simple-obfs/)

### 1. 更新基本编译依赖

```
# yum install epel-release -y
# yum update
# yum install gcc gettext autoconf libtool automake make openssl-devel pcre-devel asciidoc xmlto zlib-devel openssl-devel libsodium-devel udns-devel libev-devel -y
```

### 2. 安装

添加单独的仓库，可能要另外安装wget`yum install wget`。

完成下面命令`/etc/yum.repos.d/`目录下出现`librehat-shadowsocks-epel-7.repo`

```
# wget https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo
# cp librehat-shadowsocks-epel-7.repo /etc/yum.repos.d/
```

安装shadowsocks-libev

```
# yum update
# yum install shadowsocks-libev
```

> 如果出现：
>
> No package udns-devel available.
> No package mbedtls-devel available.
>
> 尝试
>
> ```
> # yum -y install yum-utils
> # yum-config-manager --enable epel
> # yum update
> ```

### 3. 编辑配置文件

编辑配置文件`/etc/shadowsocks-libev/config.json`

```
{
    "server": "34.24.24.241",
	"server_port": 43943,
	"local_address": "127.0.0.1",
	"local_port": 1080,
	"password": "zzz.buzz",
    "method":"aes-256-gcm",
    "timeout": 600
	"mode": "tcp_and_udp"
}
```

加密推荐aes-256-gcm，尽量不要和网上样选aes-256-cfb

端口推荐五位，不要常用端口如：8888，6666，2333

上面措施都是为了防止被墙学习到，然后导致`Connection has been reset by perr.`（其实可能也没什么用。。）

### 4. 启动服务器

这里用运行系统服务的方式（其实也可以直接`ss-server`启动）

系统服务的方式缺点就是看不到各种进过服务器流量的日志和错误日志。

```
# systemctl start shadowsocks-libev
# systemctl status shadowsocks-libev -l
# systemctl stop shadowsocks-libev
```

将该服务放入系统默认启动服务

```
# systemctl enable shadowsocks-libev
```

下面几句查看状态，错误时可以查看（虽然作为服务可以看到的log信息不多）

```
# 如果遇到故障，可以查看最近一次启动后的系统日志：
# journalctl -b

# 或者指定查看 shadowsocks 最近一次启动后的日志：
# journalctl -b -u shadowsocks

# 或者实施跟踪最近一次启动后的实时日志：
# journalctl -f -b -u shadowsocks
```

顺便查看系统服务

```
#  systemctl list-units –type=service
or
# systemctl list-unit-files | grep enabled
```







### 5. 客户端安装

自己用的是gui客户端：[ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG/releases)

> [其他版本](http://shadowsocks.org/en/download/clients.html)

<br>



## 三、Firewalls

- iptables

iptables和iptables-services不一定同时都有，iptables-services装了才能当作服务运行

下面第三句保存过滤规则到系统服务中

```
# yum install iptables iptables-services -y
# iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport [server port] -j ACCEPT
# service iptables save
```

启动服务，注册到开机基本服务中，使得开机自启

```
# systemctl start iptables.service
# systemctl enable iptables.service
```

熟悉的几句，查看状态

```
# systemctl status iptables.service
```

> 这几句同样奏效
>
> ```
> # service iptables start
> # service iptables status
> # service iptables stop
> # service iptables restart
> ```



- firewalld

没用到╮(╯_╰)╭



## ServerSpeeder/ ~~BBR~~/ ~~Kcptun~~/ ~~FinalSpeed~~

- ServerSpeeder锐速
  - 系统内核需要在级别`kernel-3.10.0-229.1.2.el7.x86_64.rpm`

```
# rpm -ivh http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-3.10.0-229.1.2.el7.x86_64.rpm --force
# reboot
```

安装

```
#wget -N --no-check-certificate https://github.com/91yun/serverspeeder/raw/master/serverspeeder.sh && bash serverspeeder.sh
```

使用

```
# service serverSpeeder start
# service serverSpeeder status
```



- [BBR](https://github.com/iMeiji/shadowsocks_install/wiki/开启TCP-BBR拥塞控制算法)
  - linux内核4.9+版本自带，有需要则可以开启
- [Kcptun](https://github.com/xtaci/kcptun)
  - 服务端配置简单一条龙服务
  - 客户端结合ss客户端使用麻烦
  - [windows gui](https://github.com/dfdragon/kcptun_gclient/releases)
- [FinalSpeed](https://github.com/d1sm/finalspeed/tree/fileshare)
  - 没有尝试



## ~~SSR~~

~~混淆弄了很久，是在搞不动。。~~