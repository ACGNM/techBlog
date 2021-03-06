---
title: "Jvm_learning"
date: 2021-03-07T21:24:28+09:00
draft: false
toc: false
images:
tags:
  - jvm
---
## JVM与JAVA体系结构

### 主要研究官方提供的HotSpot虚拟机
![HotSpot](https://i.ibb.co/n3pqprG/2020-10-11-15-05-59.png)

### 不管什么语言，只要提供编译器编译出遵循JVM规范的字节码文件，就可以在Java虚拟机运行。
![结构](https://i.ibb.co/bdvRS13/2020-10-11-15-06-28.png)

### 在Java虚拟机中执行的指令成为Java字节码指令

### 虚拟机分为系统虚拟机和软件虚拟机
系统虚拟机模拟硬件环境，软件虚拟机运行在操作系统之上，不与硬件进行直接交互。


### JVM的位置
![JVM的位置](https://i.ibb.co/K5vgxq8/2020-10-11-21-50-24.png)

java代码(.java文件)编译成字节码文件(.class文件)给JVM执行
![JVM, JRE, JDK](https://i.ibb.co/MpQbPdq/2020-10-11-21-57-30.png)

### JVM整体结构
![JVM整体结构](https://i.ibb.co/hydtksj/2020-10-12-00-01-34.png)
执行引擎用来解释运行。其中只用解释器的话对于反复被执行的热点代码的优化不够，所以引入JIT编译器(Just-In-Time Compiler)来提前将字节码文件编译成二进制机器命令。同时加入了垃圾回收机制。
将各种语言的代码编译为字节码文件的编译器是前端编译器，执行引擎的JIT编译器是后端编译器。
字节码指令不等同于机器指令，将字节码指令翻译为机器指令的部分就是执行引擎。

### Java代码执行流程
![Java代码执行流程](https://i.ibb.co/B2HCzgK/2020-10-12-00-58-55.png)
一般都采用的是解释执行和即时编译并存的执行引擎。(即图中Java虚拟机最左端部分)
JIT编译器将反复执行的热点代码编译为机器指令后还会将其缓存起来。

### 由于跨平台的设计，Java的指令都是根据栈来实现的
![指令集](https://i.ibb.co/m47vkZz/2020-10-12-01-42-48.png)
指令集可以分为基于栈的指令集架构和基于寄存器的指令集架构。
零地址指令 没有操作的地址，只有操作数。
一地址指令 有一个地址，然后是操作数
二地址三地址以此类推

栈：跨平台性，指令集小，指令数量多，执行性能比寄存器差

### JVM发展(各种虚拟机)
* Sun Classic VM (HotSpot里内置了)： 只提供解释器
* Exact VM： 可以知道某个内存中的数据是什么类型，编译器(负责执行性能)解释器(负责响应时间)混合工作
* HotSpot VM：独有方法区(JRockit，J9都没有)
* JRockit：专注于服务端应用，不包含解释器。世界上最快的JVM。MissionControl服务套件。
* IBM的J9：三大商用JVM之一
* Dalvik VM：应用于安卓，没有遵守Java虚拟机规范，基于寄存器架构。
* Graal VM：跨语言全栈虚拟机。最有希望取代HotSpot VM。

## 类加载子系统




