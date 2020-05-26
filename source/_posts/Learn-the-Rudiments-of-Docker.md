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

Docker的安装非常简单