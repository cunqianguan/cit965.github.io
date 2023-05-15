---
sidebar_label: '云原生架构师训练营'
sidebar_position: 1
title: 云原生架构师训练营
---

## 课程大纲

### 1.go语言基础
#### 内容概述：
讲解golang语言的基础语法
#### 详细内容：
- Go语言概述：介绍Go语言的起源、特点、应用领域以及Go语言开发环境的安装配置。
- 变量和数据类型：学习变量的声明、赋值、作用域等基本概念，掌握各种数据类型的使用方法。
- 运算符和表达式：学习各种运算符和表达式的基本概念，掌握常用的算术、关系、逻辑、位运算符的使用方法。
- 控制结构：学习顺序结构、选择结构、循环结构等控制结构的基本概念和使用方法。
- 数组和切片：学习数组和切片的概念、创建和使用方法。
- 函数：学习函数的基本概念、定义和调用方法，掌握函数的参数和返回值的使用方法。
- 指针：学习指针的概念、声明和使用方法。
- 结构体：学习结构体的概念、定义和使用方法，了解结构体的嵌套和方法。
- 接口：学习接口的概念、定义和使用方法，掌握接口的类型转换和类型断言。
- 并发编程：学习Go语言中的并发编程，掌握goroutine和channel的基本概念和使用方法。
- 错误处理：学习错误处理的基本概念和使用方法。
- 测试：学习测试的基本概念和使用方法，了解Go语言的单元测试和基准测试。
- 泛型，反射
### 2.go语言进阶
#### 内容概述：
go语言的高级特性，如何开始一个go项目
#### 详细内容：
- go特有的设计模式
- 并发设计哲学
- channel 底层实现
- map底层实现
- GMP调度器原理
- 包管理，依赖管理，go mod
- 如何做单元测试 集成测试
- 如何性能分析
- 使用gin和gorm搭建一个web服务器进行crud


### 3.微服务kratos 
#### 内容概述：
讲解如何基于开源微服务框架，实现熔断限流，服务发现，链路追踪等功能
#### 详细内容：
- kratos 框架介绍
- kratos 工程结构
- kratos 项目搭建和开发环境配置
- kratos proto grpc 介绍和使用
- kratos 依赖注入库 wire
- kratos 配置中心
- kragos 中间件集成
- kratos 集成ent
- kratos 熔断限流
- kratos 服务注册与发现
- kratos 链路追踪


### 4.容器化 
#### 内容概述：
讲解docker核心原理，熟练使用docker来构建镜像，推送代码仓库
#### 详细内容：
- docker介绍
- docker安装
- 运行第一个hello world 容器
- namespace资源隔离技术原理
- cgroups资源限制方法
- 理解容器网络
- docker cs架构
- 容器镜像和镜像仓库
- dockerfile编写
### 5.分布式数据库 etcd
#### 内容概述：
介绍分布式协议 raft ，etcd 原理与使用
#### 详细内容：
- etcd 介绍，安装
- 初始化一个etcd demo 集群
- 基础操作 get watch list set key
- 创建锁 选举
- etcd 备份数据
- raft 分布式协议
- etcd 和 golang客户端接入
### 6.k8s设计原理
#### 内容概述：
介绍k8s 设计哲学，整体架构，核心组件大概
#### 详细内容：
- k8s 前世今生
- k8s 设计哲学，声明式编程，不可变基础设施
- k8s 核心组件
- k8s 对象设计与api
- k8s 创建一个pod 的生命周期

### 7.k8s上手操作 
    创建pod，service，ingress

### 8.k8s 重要组件讲解
#### 内容概述：
    client   1小时
    apiserver 1小时
#### 详细内容：
- 理解client 端操作
- 理解client 和apiserver的交互
- 使用client 来操作k8s中的各种资源    
- 理解 informer 架构
- 理解 apiserver 认证鉴权准入
- 理解 GVK
- 理解 internalVersion 、externalVersion和storageVersion
- 理解版本转换
### 9.k8s 重要组件讲解
#### 内容概述：
    schedule 1小时
    controller 1小时
#### 详细内容：
- 调度阶段 预选，优选
- 为pod设置优先级
- 调度器代码走读
- 每个插件的endpoint
- 编写自定义插件
- 控制器 replication 讲解
- 控制器 deployment 讲解
- cloud controller

### 10.k8s 重要组件讲解
#### 内容概述：
    存储，网络
    csi,cni,cri
#### 详细内容：
- containerd 和 docker
- 容器间通信，节点间通信，容器节点通信
- cni插件运行原理
- 常见网络插件
- calico介绍
- 存储插件
- out of tree csi
- pvc


### 11.k8s源码讲解 
#### 内容概述：
k8s 第一个版本，k8s1.27版本代码
#### 详细内容：
- k8s第一个版本代码讲解
- k8s v1.27 代码讲解，client-go，schedule ，controller ，apimechery ， apiserver ，kubelet
### 12.k8s 2次开发 
#### 内容概述：
什么是k8s二次开发，我们可以拓展哪些方面
#### 详细内容：
- kubectl 插件拓展
- apiserver 认证拓展
- apiserver extention 拓展
- schedule 插件
- 使用kubebuilder 开发一个 operator
  
### 13.CI/CD 
#### 内容概述：
   持续集成，持续交付，讲解一些开源项目，如zadig，kubevela，argo

#### 详细内容：

- 什么是持续集成
- 本地构建和dind构建
- github action
- 什么是持续交付
- 灰度发布
- 原地升级
- 版本回退
- kubevela，zadig ，argo ，fluxed ，gitops 等开源项目介绍

### 14.helm ，kustommize应用编排 
#### 内容概述：
学习和使用helm kustommize
#### 详细描述：
- 安装helm
- 使用helm
- helm chart
- 使用helm发布你的第一个应用
- kustommize 介绍
- kustommize 布局
- 使用kustomize管理我们的应用
### 15.istio 原理 
#### 内容概述：
学习和使用istio
#### 详细描述：
- 微服务演进
- 什么是服务网格
- 为什么使用istio
- 数据平面和控制平面
- sidecar container
- ingress 和 engress
- 流量管理
- 安全
- 可观测
### 16.istio 实践
#### 内容概述：
istio 最佳实践
#### 详细内容：
- 使用istio实现染色方案
- istio 灰度发布
- istio 流量治理
### 17.prometheus 监控和分析 
#### 内容概述：
云原生下如何做监控
#### 详细内容：
- Prometheus 基础
- 安装配置
- PromQL 详解
- Relabeling 操作
- 服务发现
- PushGateway
- 黑盒监控
- Alertmanager
- 使用 Grafana 进行可视化
- 编写指标接口与构建 Exporter
- Prometheus 与 Kubernetes
- 使用 Thanos 实现水平伸缩

### 18.k8s周边生态项目介绍，工程化平台如何搭建
#### 内容概述：
讲解云原生下相关方向的开源项目
#### 详细内容：
- 云存储
- 云编排
- 容器运行时
- api gateway
- service proxy
- remote procedure call
- service discovery
- network
- security
- container registry
- observablity
- platform
- log
- trace
- chaos
- seveless