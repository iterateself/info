# Docker 组件


- Docker客户端和服务器，也成为Docker引擎；
- Docker镜像；
- Registry；
- Docker容器。

## Docker 客户端和服务器

Docker是一个客户端/服务器（C/S）架构的程序。Docker客户端只需向Docker服务器或守护进程发出请求，服务器或守护进程将完成所有工作并返回结果。

Docker守护进程有时也称为Docker引擎。Docker提供了一个命令行工具docker以及一整套RESTful API来与守护进程交互。用户可以在同一台宿主机上运行Docker守护进程和客户端，也可以从本地的Docker客户端连接到运行在另一台宿主机上的远程Docker守护进程。

![mark](http://images.iterate.site/blog/image/20200202/vPjtapl8L7jn.png?imageslim)


## Docker 镜像

镜像是构建Docker世界的基石。用户基于镜像来运行自己的容器。镜像也是Docker生命周期中的“构建”部分。镜像是基于联合（Union）文件系统的一种层式的结构，由一系列指令一步一步构建出来。例如：

```
添加一个文件；
执行一个命令；
打开一个端口。
```


也可以把镜像当作容器的“源代码”。镜像体积很小，非常“便携”，易于分享、存储和更新。在本书中，我们将会学习如何使用已有的镜像，同时也会尝试构建自己的镜像。

不清楚的：

- <span style="color:red;">镜像到底是什么？镜像文件内容到底是什么？是怎么起作用的？</span>


## Registry

Docker用Registry来保存用户构建的镜像。Registry分为公共和私有两种。

Docker公司运营的公共Registry叫作Docker Hub。用户可以在Docker Hub注册账号，分享并保存自己的镜像。

根据最新统计，Docker Hub上有超过10 000注册用户构建和分享的镜像。需要Nginx Web服务器的Docker镜像，或者Asterix开源PABX系统的镜像，抑或是MySQL数据库的镜像？这些镜像在Docker Hub上都有，而且具有多种版本。

用户也可以在Docker Hub上保存自己的私有镜像。例如，包含源代码或专利信息等需要保密的镜像，或者只在团队或组织内部可见的镜像。

用户甚至可以架设自己的私有Registry。具体方法会在第4章中讨论。私有Registry可以受到防火墙的保护，将镜像保存在防火墙后面，以满足一些组织的特殊需求。

不清楚：

- <span style="color:red;"> 如何架设自己的 Registry？</span>

## 容器

Docker可以帮用户构建和部署容器，用户只需要把自己的应用程序或服务打包放进容器即可。

我们刚刚提到，容器是基于镜像启动起来的，容器中可以运行一个或多个进程。我们可以认为，镜像是Docker生命周期中的构建或打包阶段，而容器则是启动或执行阶段。

## 总结

总结起来，Docker容器就是：

- 一个镜像格式；
- 一系列标准的操作；
- 一个执行环境。

Docker借鉴了标准集装箱的概念。标准集装箱将货物运往世界各地，Docker将这个模型运用到自己的设计哲学中，唯一不同的是：集装箱运输货物，而Docker运输软件。

每个容器都包含一个软件镜像，也就是容器的“货物”，而且与真正的货物一样，容器里的软件镜像可以进行一些操作。例如，镜像可以被创建、启动、关闭、重启以及销毁。


和集装箱一样，Docker在执行上述操作时，并不关心容器中到底塞进了什么，它不管里面是Web服务器，还是数据库，或者是应用程序服务器什么的。所有容器都按照相同的方式将内容“装载”进去。

Docker也不关心用户要把容器运到何方：用户可以在自己的笔记本中构建容器，上传到Registry，然后下载到一个物理的或者虚拟的服务器来测试，再把容器部署到Amazon EC2主机的集群中去。像标准集装箱一样，Docker容器方便替换，可以叠加，易于分发，并且尽量通用。

