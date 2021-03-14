---
title: "Networking_coursera"
date: 2021-03-14T23:21:58+09:00
draft: false
toc: false
images:
tags:
  - networking
---
# The Bits and Bytes of Computer Networking

## First Week

### protocol
a define set of standards that computers must follwo in order to communicate properly.

### TCP/IP Five Layer Network Model
#### Overview 
![five layers](https://github.com/ACGNM/pics/raw/master/networking/TCP:IP%20five%20layers.png)

#### Physical layer
Represents the physical devices that interconnect computers

#### Data link layer
Responsible for defining a common way of interpreting(解释, 翻译) these signals so network devices can communicate

- 在数据连接层有许多协议，最常见的是以太网（Ethernet）
	- the Ethernet standards also define a protocol responsible for getting data to nodes on the same network or link.
-  其他的还有越来越多的比如Wifi。

#### Network layer
- Allows different networks to communicate with each other through devices known as routers. (使得由路由器相连的不同网络之间可以互相通讯)
- Internetwork
	- A collection of networks connected together routers
	- the most famous of these being Internet

Data link layer 用来让数据跨越单个链接，Network layer 用来让数据在网络的集合中传输（在节点之间传输）

#### Transport layer
- Sorts out which client and server programs are supposed to get that data （分发数据给对应的客户/服务程序）
- TCP: Transmission Control Protocol
- UDP: User Datagram Protocol

#### Applicaiton layer
用来让用户可以发送邮件和浏览网页等等行为的的协议

#### 类比物流
![layers](https://github.com/ACGNM/pics/raw/master/networking/layers.png)

### Basic of Networking Devices

#### Cables
