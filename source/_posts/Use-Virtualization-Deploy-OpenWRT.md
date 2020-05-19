---
title: Use-Virtualization-Deploy-OpenWRT
date: 2020-04-14 10:23:24
categories: Network
tags: OpenWRT
mathjax: true
---

# 生命在于折腾

OpenWRT项目是针对嵌入式设备的Linux操作系统。OpenWrt不会尝试创建单个静态固件，而是提供具有包管理功能的完全可写文件系统。这使您从供应商提供的应用程序选择和配置中解放出来，并允许您通过使用软件包来定制设备以适合任何应用程序。对于开发人员来说，OpenWrt是构建应用程序的框架，而无需围绕它构建完整的固件。对于用户来说，这意味着可以完全自定义的能力，可以以从未想到的方式使用设备。

>   以上引用自官网说明。

<!----more---->

好了不开玩笑了。OpenWRT现在普遍被应用于家用路由器上。现在的家用路由器系统类似于以前的品牌机。是刚生产出来就被做好了。作为一个外行人，我们只能用，不能魔改。而OpenWRT的出现打破了这一现象。就像我们现在去买一个台式机，一般都是自己选择配件。OpenWRT使得我们路由器的系统也可以像组一个自己的台式机一样自己定制，想要什么cpu、什么显卡、什么硬盘都是自己说了算。而在路由器中就是，想要什么功能就有什么功能。如果你足够强，甚至可以自己定制属于自己的路由器系统。这些功能包括但不限于离线下载、屏蔽广告、科学上网、NAS等。



# 环境准备

首先看过我以前文章的应该都知道，Linux环境必备Terminal（终端，用什么都行，CMD、SecureCRT、XShell，甚至你用虚拟机自带的Monitor都可以）、WinSCP（传文件的）和随便一个稍微全一点的Yum源（建议来个epel+163或者阿里）。这里就不再操作了。

-   第一步 在Linux中安装虚拟化工具

```Shell
yum install qemu-kvm libvirt libvirt-python libguestfs-tools virt-install -y
```



有200多个包比较慢 稍微等一会儿就好了。



-   第二步 去OpenWRT的官网下载

去官网下载这一步其实也比较讲究，如果是真实物理设备，建议去<https://openwrt.org/toh/start>找你对应设备的包下载。虚拟机的话直接按照官网的教程来下载https://downloads.openwrt.org/chaos_calmer/15.05/x86/64/openwrt-15.05-x86-64-combined-ext4.img.gz

我这边使用的虚拟机，所以使用

```shell
wget https://downloads.openwrt.org/chaos_calmer/15.05/x86/64/openwrt-15.05-x86-64-combined-ext4.img.gz
```

当然速度是很慢的。可以用各种方法让他快一点。下载完成之后也肯定是要解压的。



最后就会得到这么个玩意

```shell
[root@Izayo ~]# ls
anaconda-ks.cfg  openwrt-19.07.2-x86-64-combined-ext4.img
```



其实在虚拟机里安装有镜像就行了。可惜我们官网提供的都是img文件。咱们的VMware是不支持这种格式的。所以需要转换一下。转换的方式也很简单。用qemu命令就好了。

```shell
 qemu-img convert -f raw -O vmdk openwrt-19.07.2-x86-64-combined-ext4.img openwrt-19.07.2-x86-64-combined-ext4.vmdk
```

最后就会得到`openwrt-19.07.2-x86-64-combined-ext4.vmdk`这么个玩意。把这个拖到物理机中就可以准备安装了。



# 安装OpenWRT

整个过程跟安装虚拟机没什么区别，注意不要安装系统，选择使用现有的磁盘然后把之前转换好的vmdk选上，然后需要一张仅主机模式的网卡和一张桥接网卡（仅主机充当LAN，桥接充当WAN）。系统随便选一个（我选的Linux3.x内核）.最后记得在vmx配置文件里添加

`ethernet0.virtualDev = "e1000"`

`ehternet1.virtualDev = "e1000"`

不然虚拟机会找不到网卡。



openwrt会把第一张网卡当wan，第二张当wan，所以第一张网卡桥接物理网卡，第二张网卡桥接虚拟网卡vmnet8





# 进入配置界面

