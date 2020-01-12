---
layout:     post
title:      Kubernetes 本质
subtitle:   Overview
date:       2020-01-12
author:     Galvin
header-img: baidu2.jpg
catalog: true
tags:
    - Kubernetes
---

## Docker
一个“容器”，实际上是一个由 Linux Namespace、Linux Cgroups 和 rootfs 三种技术构建出来的进程的隔离环境。

**一个正在运行的 Linux 容器，其实可以被“一分为二”地看待：**

- 一组联合挂载在 /var/lib/docker/aufs/mnt 上的 rootfs，这一部分我们称为“容器镜像”（Container Image），是容器的静态视图；
- 一个由 Namespace+Cgroups 构成的隔离环境，这一部分我们称为“容器运行时”（Container Runtime），是容器的动态视图。


## 容器编排
最具代表性的容器编排工具，当属 Docker 公司的 Compose+Swarm 组合，以及 Google 与 RedHat 公司共同主导的 Kubernetes 项目。

kubernetes 全局架构

![img](https://raw.githubusercontent.com/Galvin-wjw/Galvin-wjw.github.io/master/img/kubernetes_inf.jpg)

- Master：控制节点 
    - 负责容器编排的 kube-controller-manager
    - 负责 API 服务的 kube-apiserver
    - 负责调度的 kube-scheduler
    - 整个集群的持久化数据，则由 kube-apiserver 处理后保存在 Etcd 中。


- Node：计算节点
    - 核心组件 kubelet
    - kubelet 主要负责同容器运行时（比如 Docker 项目）打交道。而这个交互所依赖的，是一个称作 CRI（Container Runtime Interface）的远程调用接口，这个接口定义了容器运行时的各项核心操作，比如：启动一个容器需要的所有参数。
    - kubelet 还通过 gRPC 协议同一个叫作 Device Plugin 的插件进行交互。这个插件，是 Kubernetes 项目用来管理 GPU 等宿主机物理设备的主要组件，
    - kubelet 的另一个重要功能，则是调用网络插件和存储插件为容器配置网络和持久化存储。这两个插件与 kubelet 进行交互的接口，分别是 CNI（Container Networking Interface）和 CSI（Container Storage Interface）。

* * * 


![img](https://raw.githubusercontent.com/Galvin-wjw/Galvin-wjw.github.io/master/img/kubernetes_component.jpg)


**Kubernetes 项目最主要的设计思想是，从更宏观的角度，以统一的方式来定义任务之间的各种关系，并且为将来支持更多种类的关系留有余地。**

在 Kubernetes 项目中，交互频繁的容器则会被划分为一个“Pod”，Pod 里的容器共享同一个 Network Namespace、同一组数据卷，从而达到高效率交换信息的目的。**Pod 是 Kubernetes 项目中最基础的一个对象**

Pod 绑定一个 Service 服务，而 Service 服务声明的 IP 地址等信息是“终生不变”的。这个 Service 服务的主要作用，就是作为 Pod 的代理入口（Portal），从而代替 Pod 对外暴露一个固定的网络地址。

Kubernetes 定义了新的、基于 Pod 改进后的对象。比如 Job，用来描述一次性运行的 Pod（比如，大数据任务）；再比如 DaemonSet，用来描述每个宿主机上必须且只能运行一个副本的守护进程服务；又比如 CronJob，则用于描述定时任务等等。

在 Kubernetes 项目中，管理功能实现的方法是：
- 首先，通过一个“编排对象”，比如 Pod、Job、CronJob 等，来描述你试图管理的应用；
- 然后，再为它定义一些“服务对象”，比如 Service、Secret、Horizontal Pod Autoscaler（自动水平扩展器）等。这些对象，会负责具体的平台级功能。

这种使用方法，就是所谓的“声明式 API”。**这种 API 对应的“编排对象”和“服务对象”，都是 Kubernetes 项目中的 API 对象（API Object）。**