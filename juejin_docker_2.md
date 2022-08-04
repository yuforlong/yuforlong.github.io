---
title: 掘金小册-开发者必备的 Docker 实践指南-笔记:了解Docker的核心组成
date: 2022-08-04 11:00:00
categories:
- Docker
tags:
- Docker
- 读书笔记
toc: true
---
写在前面：该[小册](https://juejin.cn/book/6844733746462064654)买过好久，笔记也早就整理好放在幕布上了。现整理在blog中，以便翻阅。
## Docker实践指南-笔记:了解Docker的核心组成
<!--more-->
### 四大组成对象：
- 镜像 Image
    - 定义：
         - 可以理解为一个只读的文件包，包含了虚拟环境运行最原始文件系统的内容
    - Docker镜像
         - Docker基于AUFS作为底层文件系统实现
         - 实现了增量式的镜像结构
            ![增量式镜像结构](http://data.tcbang.xyz/uPic/nQtaES.jpg)
            ![分层](http://data.tcbang.xyz/uPic/tSHxqL.jpg)
         - 镜像层：
              - 每次对镜像内容的修改，Docker都会将这些修改铸造成一个镜像层
                >​实际上Docker的镜像是无法被修改的，因为修改是会产生新镜像，而不是原镜像的更新
              - 一个镜像其实是由其下层所有的镜像层组成
              - 每个镜像层单独拿出来，与它下面的镜像层都可以组成一个镜像
- 容器 Container
    - 定义：
         - 用于隔离虚拟环境的基础设施，在Docker里，被引申为隔离出来的虚拟环境
    - 容器应该有三项内容组成：
         - 一个Docker镜像
         - 一个程序运行环境
         - 一个指令集合
    - 容器 VS 镜像：
         - 如果把镜像理解为编程中的类，那么容器就可以理解为类的实例
         - 镜像内存放的是不可变化的东西，当以它们为基础的容器启动后，容器内也就成为了一个“活”的空间
- 网络 Network
    - Docker中可以对每个容器的网络进行设置
        ![容器网络](http://data.tcbang.xyz/uPic/0Pkh9Y.jpg)
         - 还能在容器间建立虚拟网络，将数个容器包裹其中，同时与其他网络环境隔离
    - 利用一些技术，Docker 能够在容器中营造独立的域名解析环境
         - 这使得我们可以在不修改代码和配置的前提下直接迁移容器，Docker 会为我们完成新环境的网络适配
         - 我们甚至能够在不同的物理服务器间实现，让处在两台物理机上的两个 Docker 所提供的容器，加入到同一个虚拟网络中，形成完全屏蔽硬件的效果
- 数据卷 Volume
    - 虚拟机与文件系统：
         - 虚拟机文件系统：
              - 以往的虚拟机中，通常直接采用虚拟机的文件系统作为应用数据等文件的存储位置
              - 当虚拟机或者容器出现问题导致文件系统无法使用时：
                   - 通过镜像重置文件系统可以恢复应用运行，但是之前存放的数据也消失了
         - 单独挂载文件系统存放数据：
              - 在虚拟机中繁琐
                   - 不但要搞定挂载在不同宿主机中实现的方法
                   - 还要考虑文件系统兼容性
                   - 以及虚拟操作系统配置等问题
    - Docker：
         - 受益于Union File System
         - 可以实现：
              - 能够从宿主操作系统中挂载目录
              - 能够建立独立的目录持久存放数据
              - 在容器间共享
    - 定义：
         - 通过这几种方式进行数据共享或持久化的文件或目录，我们都称为数据卷 ( Volume )
### Docker Engine
- 工业线容器引擎，实现了Docker中最核心的软件，官方维护
- 核心：
    - docker daemon
         - 被称为Docker服务，通常以服务形式运行，以便静默提供如下功能：
              - 容器管理
              - 应用编排
              - 镜像分发等
         - 其内实现了镜像模块、容器模块、数据卷模块和网络模块
         - 向外暴露了一套RESTful API，可以通过这套接口对docker daemon进行操作
            >更确切地说，是通过这套RESTful API对docker daemon中运行的容器和其他资源进行管理
            
    - docker Cli
         - 控制台程序
         - 通过docker daemon提供的Rest ful API衔接daemon和cli
### 一览图
  ![官网一览图](https://docs.docker.com/engine/images/architecture.svg)

#### Docker Build 的上下文

就上面的一览图，来理解下Docker Build的上下文。

1. 当运行 docker build 命令时，当前工作目录被称为构建上下文。
2. docker build 默认查找当前目录的 Dockerfile 作为构建输入，也可以通过 –f 指定 Dockerfile。
   >docker build –f ./Dockerfile
3. 当 docker build 运行时，首先会把构建上下文传输给 docker daemon，把没用的文件包含在构建上下文时，会导致传输时间长，构建需要的资源多，构建出的镜像大等问题。
4. 可以通过.dockerignore文件从编译上下文排除某些文件

> 不要将不需要的文件，放在Docker 的上下文中

参考：[深入理解 Docker 构建上下文](https://blog.csdn.net/qianghaohao/article/details/87554255)