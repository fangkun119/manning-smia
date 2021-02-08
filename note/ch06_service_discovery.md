[TOC]

# CH06 Service Discovery

> 本章节内容：
>
> * 服务发现的重要性，与传统负载均衡相比的优缺点
> * 配置Spring Netflix Eureka，在Eureka中注册微服务
> * 使用Spring Cloud Load Balancer来做到客户端负载均衡
>
> 重要性：(1) 水平扩展（2）减少资源闲置（3）弹性
>
> 原书章节在线阅读：[https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-6/v-8](https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-6/v-8)

## 6.1 传统集中式服务发现

### (1) 模型

><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch06_traditional_service_discovery.jpg" width="800" /></div>
>
>应用程序（服务调用方）→ DNS → Load Balancer →  服务节点（instances）
>
>负载均衡器例如：F5 (http://f5.com)，HAProxy（http://haproxy.org)
>
>特点：
>
>* 服务节点数量通常是静态的
>* 服务节点通常是永久性的（例如服务节点崩溃后，将对它进行重启还原，而IP仍然不变）
>* 辅助load  balancer通常处于空闲，通过ping主load balancer来确认它是否处于活动状态，并在主load balancer故障时接管

### (2) 适用场景

>* 这种集中式网络结构在一些大小和规模适合企业环境中仍然可以比较好的工作
>* 集中式的SSL和服务端口管理，有助于锁定入站和出站入口对位于其后所有服务器的访问，特别是对于满足PCI（支付卡行业）等合规性认证标准等场景

### (3) 不适合云环境微服务的原因

>* load balancer单点故障，水平扩展有限（冗余模型、许可证成本，容量限制）
>* 静态管理：路由集中存储，需要通过调用API来增加和删除instances，需要手动定义映射规则、增加了浮渣性
>* 在处理大量事务和冗余的云中，集中化网络扩展性弱成本效益低

## 6.2 云环境中的服务发现

> 作为一个解决方案，具有如下特点：
>
> * 高可用：”热“集群环境，节点可以互相接管，具有可靠性和伸缩性，提供故障转移
> * 点对点：节点共享服务实例的状态
> * 负载均衡，高弹性，容错

### 6.2.1 服务发现

>需要解决4个问题：1. 服务如何注册 2. 客户端如何查找服务地址 3. 服务之间如何信息共享 4. 如何进行健康监测
>

#### (1) 添加/删除服务实例

><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch06_node_add_remove.jpg" width="800" /></div>
>

>`Service Instance`启动时，将其地址（物理位置例如IP + 路径 + 端口）注册给`Service Discovery Layer`
>
>* 虽然每个实例的地址不同，但是他们都注册在同一个服务ID（唯一标识一组instance）下
>* 通常只向一个`Service Discovery Instance`注册，然后通过数据传播将其传给整个`Service Discovery Layer`，例如
>    * Consul八卦协议：[https://www.consul.io/docs/internals/gossip.html](https://www.consul.io/docs/internals/gossip.html)
>    * Brian Storti Swim可伸缩成员身份协议：[https://www.brianstorti.com/swim/](https://www.brianstorti.com/swim/)

> 当健康监控发现instance状态异常时，将其删除

#### (2) 客户端负载均衡

>如果客户端每次请求服务时，都调用`Service Discovery`会使系统变得脆弱，更好的方式是**客户端负载均衡**
>
>* 客户端会缓存服务的位置，以便不用每次都请求`Service Discovery`
>* 负载均衡在客户端进行，同时与Eureka一起使用，当发现instance故障时，会从注册表中删除该故障节点
>* 客户端load balancer定期从service dicovery更新instance列表
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch06_loadbalance_cache.jpg" width="800" /></div>
>

### 6.2.2 本章Demo及服务间调用关系

> 本章的Demo使用`Spring Netflix Eureka`作为服务发现引擎，使用`Spring Cloud Load Balancer`用于客户端负载均衡（虽然Ribbon是目前主要使用的load balancer，但是已经不再开发进入了维护模式，因此使用了Spring Cloud Load Balancer）
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch06_eureka_with_caching.jpg" width="800" /></div>
>
> 本章Demo创建了两个Service -`licensing service`和`organization service`：当licensing service调用organization service时，instance的信息来自Eureka，并且通过client side load banlancer进行选择
>
> Demo代码位置：[../chapter6/Final/](../chapter6/Final/)

## 6.3 构建Spring Eureka Service

> d
>
> d
>
> d

## 6.4 向Eureka注册服务

> d
>
> d
>
> d

### 6.4.1 Eureka's REST API

> d
>
> d
>
> d

### 6.4.2 Eureka dashboard

> d
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch06_erueka_dashboard.jpg" width="800" /></div>
>
> d

### 6.5 使用Service Discovery发现服务

> d
>
> d
>
> d

### 6.5.1 使用Spring Discovery Client发现服务实例

> d
>
> d
>
> d

### 6.5.2 使用LoadBalancer-aware  Spring  RestTemplate调用服务

> d
>
> d
>
> d

### 6.5.3 使用Netflix Feign Client调用服务

> d
>
> d
>
> d

## 6.6 小结

> 略 