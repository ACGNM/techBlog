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

#### Hubs and Switches

- Hub是物理层设备，交换机是数据链路层设备
- Hub在接收到信号时会发送至连接到Hub的所有其他设备。而交换机则会根据以太网协议只发送给目标节点。所以当所有节点同时发送信息时，Hub的信号会互相干扰所以会有很长的等待时间，而交换机避免了这样的事。
- The primary devices used to connect computers on a single network, usually referred to as a **LAN**, or **Local Area Network**.

#### Routers
- A router is a device that knows how to forward data between independent networks. (网络层设备)

- **BGP (Border Gateway Protocol)**

	- Routers share data with each other via a protocol known as BGP, that let's them learn about the most optimal paths to forward traffic.

### The Physical Layer

#### Modulation 
A way of varing the voltage of this charge moving across the cable

- Used in twisted pair (双绞线) known as **Line coding**
- 利用双绞线可以实现全双工（full-duplex）和半双工（half-duplex）
	- 全双工两边可以同时进行通信，半双工在一段时间内只有一方可以传递数据
	- 一到两对双绞线给双向通讯中的某个单项通讯保证留有信道

![cables](https://github.com/ACGNM/pics/raw/master/networking/cables.png)

#### Network Port
- 最常见的接口是RJ450接口（Register Jack 45）
	- 左右两个LED。左边的闪说明两边都连接着启动中的设备。右边闪说明在进行数据传输。
![RJ45](https://github.com/ACGNM/pics/raw/master/networking/RJ45.png)

#### Patch Panel
有许多端口，仅仅起集线作用，输出端一般连接到交换机。
![Patch Panel](https://github.com/ACGNM/pics/raw/master/networking/Patch_panel.png)

### The Data Link Layer

一开始以太网一次只能由一台节点的发送信息。因为所每次发送信息都会发送到网络中除发送方之外的的所有节点。则如果两台电脑同时发送信息则会造成碰撞。

- Ethernet as protocol solved this problem by using a technique known as **carrier sense multiple access with collision detection**.
-  **CSMA/CD**  used to determine when the commucations channels are clear, and when a device is free to transmit data.
-  所以在collision domain中任意在网络中的节点都会接受到所有网络其他节点发送的信息

####  **所以需要一种方式#来识别传输朝向哪个节点**

这就是**Media Acess Control Address (MAC地址)**

- A golablly unique identifier attached to an individual network interface
- 48-bit number normally represented by six groupings of two hexadecimal numbers **由48位的六组两位十六进制数表示**
- **Octet: any number that can be represented by 8 bits**
- Two hexadecimal digits can represent the same numbers that 8 bits can **两位的十六进制数可以表示8位所代表的数字 (FF=2^8-1)**

MAC address的形式
- 前三个octet是Organization Unique Identifier(OUI)
- 后三位Vendor Assigned
- 相当于分成两个24位的部分


