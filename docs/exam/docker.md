# docker

## 问题

- 什么是容器?
- 什么是 Docker?
- 为什么要用 Docker ?
- 容器和虚拟机的区别?
- 容器的三个基本概念？
- docker常见命令？


## 答案

### 什么是容器?

!!! tip "什么是容器?"

容器就是将软件打包一个轻量的可执行的独立软件包，包括环境、配置、代码都一起打包，然后开箱即用，不用再配置。

### 什么是 Docker?

!!! tip "什么是 Docker?"

- 一个容器
- google开发的，基于go
- 可以把自己的应用放入容器，连带配置，可以修改、复制、版本控制、分享


### 为什么要用 Docker ?

!!! tip "为什么要用 Docker ?"

- 一致的运行时环境，连代码带环境配置一起打包好了
- 启动非常快
- 环境隔离，你的配置不会影响到其他容器，也不会受外部影响
- 很容易迁移
- 容易扩展

### 容器和虚拟机的区别?

!!! tip "容器和虚拟机的区别?"

- 虚拟机要虚拟整个操作系统，没有容器轻量
- 所以启动也要更慢
- 虚拟机隔离的是系统或者用户，容器隔离的应用

### 容器的三个基本概念？

!!! tip "容器的三个基本概念？"

- [x] 镜像

就是image,相当于容器打包好的静态文件，就像是java的类

- [x] 容器

container，容器的运行时的实体，相当于类的实例，所有有生命周期，可以启动，运行，停止，暂停

- [x] 仓库

存放镜像文件的地方，类似于maven仓库，或者是github，里面各种开源的配置好的镜像给大家下载使用


### docker常见命令？

!!! tip "docker常见命令？"

```docker
docker images //查看所有镜像
docker container ls //查看所有容器
docker ps //查看正在运行的容器
docker search mysql //从仓库搜索镜像
docker pull mysql:5.7 //从仓库拉取镜像到本地
docker rmi f6509bac4980 //删除镜像
docker run -it [containerId] //启动容器
docker rm [containerId] //删除容器
docker exec -it [containerId] /bin/bash //进入容器后台
```

