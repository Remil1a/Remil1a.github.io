---
title: Learn_the_Rudiments_of_Docker
date: 2020-05-20 10:24:37
categories: Docker
tags: Docker
---

# 什么是Docker

我以初学者的角度来说，网络上各种解释已经非常到位了。然而作为初学者，看到那些个架构图还是非常云里雾里的。因为学习都是从最基础的开始，个人偏好以实战为主。所以简单来张图吧。现存的Docker教程起手都是对架构一通解释，其实没有太大必要，个人认为初学者需要先做出效果然后再学习对应架构中的位置会更加清晰，当然，这需要有点计算机基础。

<!---more---->

就三张图吧 图片来源网络，也是感觉挺不错就拿过来了，前提是了解过kvm以及其他的虚拟化比如vmware、hyper-v之类的

![1](Learn-the-Rudiments-of-Docker/1.jpg)



![2](Learn-the-Rudiments-of-Docker/2.jpg)

![3](Learn-the-Rudiments-of-Docker/3.jpg)

Docker实际上就是一个应用程序+一堆运行库+一个rootfs组成的环境，由于没有hypervisor层，所以各方面性能都会比VM要强，但是隔离性也差，目前作为初学者知道这些就可以了，更适用于部署应用的场景，如果衍生到云计算可以说是PaaS的角色。



# 如何安装Docker

Docker的安装非常简单，在[这个链接](https://docs.docker.com/get-docker/)里可以找到官网的安装方法。Docker提供了rpm，deb和二进制包的安装方法，在桌面系统如MacOS和Windows还有桌面版应用（Windows中的不太好用）。这边采用官网的方法+阿里云的源去安装（主要是国外源太慢了）。

## Docker的版本



Docker一共有三个版本，这边先引用一段官方的解释。



>Docker Engine has three types of update channels, **stable**, **test**, and **nightly**:
>
>- The **Stable** channel gives you latest releases for general availability.
>- The **Test** channel gives pre-releases that are ready for testing before general availability (GA).
>- The **Nightly** channel gives you latest builds of work in progress for the next major release.

简单来说 Docker目前有三个版本，Stable（稳定），Test（测试）和Nightly(实时更新版本)



前两个比较好理解 有点像游戏的正式服和测试服，而nightly主要是开发者维护的一个版本，一般是白天将各自的代码提交到一个中心，晚上做编译得到的版本，这种版本功能新，bug也多，适合一些关注Docker的开发者和有一定动手解决bug能力的人。作为初学者使用Stable版就够了。



## 事前准备

官网的流程有一个卸载旧版本docker的命令，如果需要的话可以执行，这篇教程使用的是新部署的CentOS7，所以没有这一步。不过还是把命令贴上来

```Shell
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```



接下来就是安装了。由于不管是光盘中的源还是CentOS的自带源都没有安装Docker的部分，所以我们还需要指定一个源让系统去下载Docker的安装包

```shell
sudo yum install -y yum-utils
sudo yum-config-manager \
--add-repo \
http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
sudo yum makecache fast
```



然后就可以进行docker安装了

```shell
sudo yum install -y docker-ce-cli docker-ce containerd.io
```



## 启动docker

跟其他服务一样 使用systemctl就可以启动docker了

```shell
systemctl start docker
systemctl enable docker
```

之后可以使用 `docker version`命令验证docker是否安装成功

```
docker version
Client: Docker Engine - Community
 Version:           19.03.12
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        48a66213fe
 Built:             Mon Jun 22 15:46:54 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.12
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       48a66213fe
  Built:            Mon Jun 22 15:45:28 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

如果出现以上一些版本信息说明安装到位了。

## 更改Docker Registry源位置

首先了解一下何为Docker Registry。就如同目前我们使用的CentOS一样，Docker需要一个远程的Image仓库。我们启动各种各样的基于Docker的服务是先从Registry将Image下载下来，再将Image放到相应的Container里。当然Image，Registry都可以自己制作，不依赖外网也是完全可以运行的。不过这边先用国内的仓库感受一下Docker的基本操作和用法。然后再学习如何搭建自己的Registry

首先注册一下阿里云，用支付宝或者淘宝账户就能登录了。



然后找到控制台，找容器镜像服务，开通一下就行。届时会给一个属于自己的加速链接，后面会用上。

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["你的链接地址"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# 跑一下Hello World

```shell
docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:d58e752213a51785838f9eed2b7a498ffa1cb3aa7f946dda11af39286c3db9a9
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```



这里能看到