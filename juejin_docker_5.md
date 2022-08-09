---
title: 掘金小册-开发者必备的 Docker 实践指南-笔记:Dockerfile
date: 2022-08-09 15:30:00
categories:
- Docker
tags:
- Docker
- 读书笔记
toc: true
---
写在前面：该[小册](https://juejin.cn/book/6844733746462064654)买过好久，笔记也早就整理好放在幕布上了。现整理在blog中，以便翻阅。
## Docker实践指南-笔记:Dockerfile
<!--more-->

### 关于Dockerfile
- 简介：
    - Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。
    - 包含了构建镜像过程中需要执行的命令和其他操作
- 优势：
    - Dockerfile 的体积远小于镜像包，更容易进行快速迁移和部署。
    - 环境构建流程记录了 Dockerfile 中，能够直观的看到镜像构建的顺序和逻辑。
    - 使用 Dockerfile 来构建镜像能够更轻松的实现自动部署等自动化流程。
    - 在修改环境搭建细节时，修改 Dockerfile 文件要比从新提交镜像来的轻松、简单。

### 编写Dockerfile
- 文件内容：
    - 注释
    - 指令
- Dockerfile的指令分类：
这些指令分类，不是一定都要出现在一个Docerfile里的！
    - 基础指令：用于定义新镜像的基础和性质。
    - 控制指令：是指导镜像构建的核心部分，用于描述镜像在构建过程中需要执行的命令。
    - 引入指令：用于将外部文件直接引入到构建镜像内部。
    - 执行指令：能够为基于镜像所创建的容器，指定在启动时需要执行的脚本或命令。
    - 配置指令：对镜像以及基于镜像所创建的容器，可以通过配置指令对其网络、用户等内容进行配置。

### 常见Dockerfile指令
- FROM
    - 作用：
         - 指定一个基础镜像
         - 接下来的所有指令都是基于这个镜像展开的
    - 三种形式：
     >无论哪种形式，其核心逻辑就是指出能够被Docker识别的那个镜像，好让Docker从那个镜像之上开始构建工作
         - FROM <image> [AS <name>]
         - FROM <image>[:<tag>] [AS <name>]
         - FROM <image>[@<digest>] [AS <name>]
    - 说明：
         - Docfile中的第一条指令必须是FROM指令，如果没有基础镜像，一切构建都无法开展
         - 可以多次出现FROM指令
              - 第二次或之后出现时：
                   - 表示在此刻构建时，要将当前指出镜像的内容合并到此刻构建镜像的内容里
                   - 方便合并两个镜像的功能
- RUN
    - 作用：
         - 用于向控制台发送命令
         - 构建时Docker会执行RUN之后的这些命令，并将它们对文件系统的修改记录下来，形成镜像的变化
    - 格式：
         - RUN <command>
         - RUN ["executable", "param1", "param2"]
    - 说明：
         - 支持\换行，单行太长，可以进行切割，方便阅读
- ENTRYPOINT 和 CMD
    - 作用：
         - 定义容器启动时，进程号为1的进程命令
    - 格式：
         - ENTRYPOINT ["executable", "param1", "param2"]
         - ENTRYPOINT command param1 param2
         - CMD ["executable","param1","param2"]
         - CMD ["param1","param2"]
         - CMD command param1 param2
    - 说明：
         - 二者用法近似，都是给出需要执行的命令，并且都可以为空
         - 当二者同时给出时，CMD中的内容会作为ENTRYPOINT定义命令的参数，最终执行的是ENTRYPOINT中给出的命令
- EXPOSE
    - 作用：
         - 为镜像指定要暴露的端口
    - 格式：
         - EXPOSE <port> [<port>/<protocol>...]
    - 说明：
         - 通过EXPOSE配置了端口暴露定义，基于这个镜像所创建的容器，被其他容器通过--link选项连接时，就能够直接允许来自其他容器对这些端口的访问了
- VOLUME
    - 作用：
         - 定义基于此镜像的容器所自动建立的数据卷
    - 格式：
         - VOLUME ["/data"]
    - 说明：
         - 在 VOLUME 指令中定义的目录，在基于新镜像创建容器时，会自动建立为数据卷，不需要我们再单独使用 -v 选项来配置了。
- COPY 和 ADD
    - 作用：
         - 使用 COPY 或 ADD 指令能够帮助我们直接从宿主机的文件系统里拷贝内容到镜像里的文件系统中
    - 格式：
         - COPY [--chown=<user>:<group>] <src>... <dest>
         - ADD [--chown=<user>:<group>] <src>... <dest>
         - COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
         - ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]
    - 说明：
         - 为什么需要COPY或ADD？
              - 在制作新的镜像的时候，我们可能需要将一些软件配置、程序代码、执行脚本等直接导入到镜像内的文件系统里
         - COPY和ADD区别：
              - 指令定义完全一样
              - ADD能够支持使用网络端的URL地址作为src源
              - 在源文件被识别为压缩包时，ADD命令会自动进行解压

### 构建镜像：
- docker build
    - 示例：
         - sudo docker build ./webapp
    - 参数：
         - 后面的参数是目录路径，而非Dockerfile文件的路径
         - 这个目录会作为构建的环境目录
              - 使用COPY或ADD拷贝文件到构建的新镜像时，会以这个目录作为基础目录
    - 说明：
         - 默认情况 下，docker build也会在参数目录下寻找名为Dockerfile的文件
         - 如果Dockerfile不在这个目录下，或者有别的文件名，可以通过-f指定Dockerfile的路径
              - sudo docker build -t webapp:latest -f ./webapp/a.Dockerfile ./webapp
         - 增加-t选项可以用来指定新生成镜像的名称：
              - sudo docker build -t webapp:latest ./webapp

### 构建中使用变量
- 参数变量：
    - ARG命令创建变量名
         - ARG TOMCAT_VERSION
    - 使用变量：
         - $TOMCAT_VERSION
    - 指定变量的值：
         - docker build时传递--build-arg TOMCAT_VERSION=1.2
    - 示例：
         - Dockerfile
          ```bash
               ARG  CODE_VERSION=latest
               FROM base:${CODE_VERSION}
               CMD  /code/run-app

               FROM extras:${CODE_VERSION}
               CMD  /code/run-extras
          ```

         - build
          ```bash
            sudo docker build --build-arg TOMCAT_MAJOR=8 --build-arg TOMCAT_VERSION=8.0.53 -t tomcat:8.0 ./tomcat
          ```

- 环境变量：
    - ENV指定环境变量：
         - ENV TOMCAT_VERSION 8.0.53
    - 使用环境变量：
         - $TOMCAT_VERSION
    - 对比参数变量：
         - 环境变量不仅能够影响构建，还能够影响基于此镜像创建的容器
         - 环境变量设置的实质，其实就是定义操作系统环境变量
              - 在运行的容器里，一样拥有这些环境变量，而容器中运行的程序也能够得到这些变量的值
         - 环境变量的值不是在构建指令中传入的，而是在Dockerfile编写的
              - 如需修改，需要修改Dockerfile
         - 如果变量名与参数变量名称相同，ENV指令定义的变量，永远会覆盖ARG所定义的变量
              - 即使它们定义时的顺序是相反的！
    - 说明：
         - 在容器运行时如果想覆盖环境变量的值，可以在创建容器时使用-e 或者  --env
              - sudo docker run -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.7

### 合并命令：

- RUN命令
    - 尽量使用RUN命令时，采用换行方式分隔多个命令
    - 而不是多条RUN命令
- 原因：
    - 看似连续的构建过程，其实由多个小段组成
    - 每一条能够形成对文件系统改动的指令在被执行前，会先基于上条命令的结果启动一个容器
    - 在容器中运行这条指令的内容，之后将结果打包成一个镜像层
    - 如此反复，最终形成镜像
    - 图示：
      ![分层示例](http://data.tcbang.xyz/uPic/BTWLY5.jpg)

- 示例：
     ```bash
     RUN apt-get update; \
     apt-get install -y --no-install-recommends $fetchDeps; \
     rm -rf /var/lib/apt/lists/;
     RUN apt-get update
     RUN apt-get install -y --no-install-recommends $fetchDeps
     RUN rm -rf /var/lib/apt/lists/
     ```

### 搭配ENTRYPOINT和CMD
- 组合形式：
     ![搭配ENTRYPOINT和CMD](http://data.tcbang.xyz/uPic/H5vpdA.jpg)
- 两个命令设计目的：
    - ENTRYPOINT指令主要用于对容器进行一些初始化
         - 使用脚本文件作为ENTRYPOINT内容是常见的做法，因为容器运行初始化的命令相对较多，全部放置在ENTRYPOINT后会特别复杂
    - CMD指令用于真正定义容器中主程序的启动命令
- 说明：
    - 创建容器时可以改写容器主程序的启动命令，这个覆盖只会覆盖CMD中定义的内容，而不会影响ENTRYPOINT中的内容
    - ENTRYPOINT没有CMD常用
