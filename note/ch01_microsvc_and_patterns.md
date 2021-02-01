<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [CH01 微服务介绍](#ch01-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E4%BB%8B%E7%BB%8D)
  - [1.01 微服务架构](#101-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84)
  - [1.02 Spring和Spring Boot带来的简化](#102-spring%E5%92%8Cspring-boot%E5%B8%A6%E6%9D%A5%E7%9A%84%E7%AE%80%E5%8C%96)
  - [1.03 微服务全景（本书构建的项目概览）](#103-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%85%A8%E6%99%AF%E6%9C%AC%E4%B9%A6%E6%9E%84%E5%BB%BA%E7%9A%84%E9%A1%B9%E7%9B%AE%E6%A6%82%E8%A7%88)
  - [1.04 本书的内容](#104-%E6%9C%AC%E4%B9%A6%E7%9A%84%E5%86%85%E5%AE%B9)
    - [1.04.1 将要从书中学到的](#1041-%E5%B0%86%E8%A6%81%E4%BB%8E%E4%B9%A6%E4%B8%AD%E5%AD%A6%E5%88%B0%E7%9A%84)
    - [1.04.2 谁适合阅读本书](#1042-%E8%B0%81%E9%80%82%E5%90%88%E9%98%85%E8%AF%BB%E6%9C%AC%E4%B9%A6)
  - [1.05 云与微服务应用](#105-%E4%BA%91%E4%B8%8E%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%BA%94%E7%94%A8)
    - [1.05.1 使用Spring Boot构建Core Services](#1051-%E4%BD%BF%E7%94%A8spring-boot%E6%9E%84%E5%BB%BAcore-services)
    - [1.05.2 云计算](#1052-%E4%BA%91%E8%AE%A1%E7%AE%97)
    - [1.05.3 为什么使用云环境部署微服务](#1053-%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BD%BF%E7%94%A8%E4%BA%91%E7%8E%AF%E5%A2%83%E9%83%A8%E7%BD%B2%E5%BE%AE%E6%9C%8D%E5%8A%A1)
  - [1.06 构建微服务所需考虑的因素](#106-%E6%9E%84%E5%BB%BA%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%89%80%E9%9C%80%E8%80%83%E8%99%91%E7%9A%84%E5%9B%A0%E7%B4%A0)
  - [1.07 核心服务的开发模式（Core Microservice Development Pattern）](#107-%E6%A0%B8%E5%BF%83%E6%9C%8D%E5%8A%A1%E7%9A%84%E5%BC%80%E5%8F%91%E6%A8%A1%E5%BC%8Fcore-microservice-development-pattern)
  - [1.08 路由模式（Routing Patterns）](#108-%E8%B7%AF%E7%94%B1%E6%A8%A1%E5%BC%8Frouting-patterns)
  - [1.09 客户端容错模式（Client Resiliency Pattern）](#109-%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%AE%B9%E9%94%99%E6%A8%A1%E5%BC%8Fclient-resiliency-pattern)
  - [1.10 安全模式（Security Pattern）](#110-%E5%AE%89%E5%85%A8%E6%A8%A1%E5%BC%8Fsecurity-pattern)
  - [1.11 微服务日志与跟踪模式（Logging and Tracing Pattern）](#111-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%97%A5%E5%BF%97%E4%B8%8E%E8%B7%9F%E8%B8%AA%E6%A8%A1%E5%BC%8Flogging-and-tracing-pattern)
  - [1.12 性能指标度量模式（Application Metrics Patterns）](#112-%E6%80%A7%E8%83%BD%E6%8C%87%E6%A0%87%E5%BA%A6%E9%87%8F%E6%A8%A1%E5%BC%8Fapplication-metrics-patterns)
  - [1.13 开发和部署模式（Build and Deployment Patterns）](#113-%E5%BC%80%E5%8F%91%E5%92%8C%E9%83%A8%E7%BD%B2%E6%A8%A1%E5%BC%8Fbuild-and-deployment-patterns)
  - [1.14 小结](#114-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# CH01 微服务介绍

> 本章包含四个方面的内容
>
> * 传统的monolithic式的架构与微服务架构的差别 （1.1）
> * 理解微服务、公司为何使用微服务 （1.1）
> * 使用Spring、Spring Boot、Spring Cloud构建微服务（1.2 - 1.4)
> * 理解cloud concept以及cloud-based computing models（1.5 - 1.13）
>
> 原书在线阅读：[https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-1/v-8/](https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-1/v-8/)

## 1.01 微服务架构

<!--分布式松耦合带来的挑战：scalability，service discovery，monitoring，distributed tracing，security，management，……-->

<!--技术：Spring Cloud，Spring boot，Swagger，Docker，Kubernetes，ELK（Elasticsearch，Logstash，Kibana），Stack，Grafana，Prometheus，……，client-load balancing，dynamic scaling，distributed tracing， ……-->

传统`Monolithic式架构`与`微服务架构`的对比

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_arch_cmp.jpg" width="600"/></div>

传统`Monolithic式架构`要求多个部门手动同步他们各自维护的系统 ，因为这些系统将会构建成一个整体

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_monolithic_arch.jpg" width="600" /></div>

`微服务架构`用来响应系统、技术、组织增大后带来的挑战。整个系统由小型、松耦合、分布式的服务所组成

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_microsvc_arch.jpg" width="600" /></div>

微服务架构的设计目标

> * 系统被划分为合适粒度、职责明确、边界清晰的组件
> * 组件之间通过轻量级方式通用的方式进行通讯（例如JSON）作为服务之间的交互接口，底层的实现方式可以更换
> * 每个组件在部署方面完全独立
> * 小型化和松耦合能够降低各个小团队工作的复杂度

转向微服务的原因

> * 支持业务扩展带来的复杂度
> * 支持快速迭代
> * 性能、可扩展性、可用性、容错的保障

## 1.02 Spring和Spring Boot带来的简化

> Spring：通过Dependency Injection简化对象关系管理，进而降低大型Java项目的复杂度
>
> Spring Boot：
>
> * 嵌入式web server简化服务器配置：tomcat（默认），jetty，undertow
> * 使用starter依赖快速创建项目
> * 自动配置
> * 大量辅助功能涉及metrics、security、status verification、externalized configuration
> * 与Spring生态系统集成

## 1.03 微服务全景（本书构建的项目概览）

> 本书演示如何使用Spring Boot、Spring Cloud以及相关技术构建微服务架构，下图是相关Service和technology integrations的概览（原书的图目前也不清楚）
>
> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_tech_overview.jpg")
>
> 从用户发起请求开始，经历如下环节
>
> * 向KeyCloak请求授权获得access token
> * 向Spring Cloud API Gateway（整个应用的Entry Point）发起请求
> * API Gateway与Service Discovery通信，并找到与Request相对应的Service（Orgnization Service，Licensing Services）并发起请求
> * Orgnization Service访问KeyCloak验证access token有效并且用户有权限，计算并返回Response
> * 如果涉及到update操作，Orgnization  Service还会向Kakfa发送一条消息，以便消费该Kafka topic的Licensing Service能够监听到该变化 
> * Licensing Service收到消息后 ，更新Redis
>
> 整个过程中，微服务架构将
>
> * 使用Zipkin，ElasticSearch，Logstash进行分布式追踪
> * 使用Zipkin管理和显示日志
> * 使用Spring Boot Actuator，Prometheus，Grafana来显示应用状态（Application Metrics）
>
> 在本书中将涉及到Spring Boot，Spring Cloud，Elasticsearch，Logstash，Kibana，Prometheus，Grafana，Kafka等……，展示如何将它们组合起来实现各自的功能

## 1.04 本书的内容

### 1.04.1 将要从书中学到的

> 使用多种Spring技术构建可以部署在本地、私有云、公有云上的微服务，涉及如下主题：
>
> * 微服务、最佳实践、设计考量、不适合使用微服务的场景
> * SpringBoot，Docker在微服务中的使用
> * 微服务的运维模式、特别是在云环境中，Spring Cloud在其中的应用
> * 如何创建application metrics并且在监控工具中可视化
> * 如何构建能够在本地、私有云、公有云上使用的deployment pipeline

### 1.04.2 谁适合阅读本书

> 略

## 1.05 云与微服务应用

### 1.05.1 使用Spring Boot构建Core Services

> 演示使用Spring Boot构建一个hellow world REST service，并使用PostMan进行测试
>
> 代码：[../chapter1/simple-application/](../chapter1/simple-application/)

### 1.05.2 云计算

> 云计算为提供计算和虚拟化服务，如下图，不同的`Cloud Computing Models`为用户提供不同程度的服务托管。常见的`Cloud Computing Models`包括：
>
> * `Infrastructure as a Service (IaaS)`：提供服务器/存储/网络等、需要用户来管理虚拟机，例如`AWS EC2`、`Azure Virtual Machines`、`Google Compute Engine`
> * `Container as a Service (CaaS)`：提供容器管理服务给用户，例如`Google Container Engine (GKE)`，` Amazon’s Elastic Container Service (ECS)`
> * `Platform as a Service (PaaS)`：提供程序运行环境给用户，例如`Google App Engine`，`CloudFoundry`，`Heroku`，`AWS (Beanstalk)`
> * `Function as a Service (FaaS)`：用户专注在实现和上传定制化的Function而不需要管理基础设施，例如`AWS (Lambda)`，`Google Cloud Function`，`Azure functions`
> * `Software as a Service (SaaS)`：用户通过界面来定制和管理所需要的功能，例如`Salesforce`、`SAP`、`Google Business`
>
> 这些Model提供的服务如下图
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_cloud_computing_models.jpg" width="600" /></div>

### 1.05.3 为什么使用云环境部署微服务

> 本书的应用构建在CaaS之上，使用Docker容器，其特点是：
>
> * 简化了基础实施管理
> * 水平扩展能力
> * 跨数据中心和地域的服务部署

## 1.06 构建微服务所需考虑的因素

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_microsvc_guide.jpg" width="600" /></div>
>
> 如上图，构建一个健壮的微服务，需要考虑的因素包括
>
> * `Right Sized`：每个服务的内容量适度，功能开发/变更快速，同时又不会给整体架构带来风险
> * `Location Transaprent`：如何管理服务之间的调用，可以快速启动和关闭多个service instances
> * `Resilient`：容错性，`fail fast`/`系统整体健壮`/`routing around failing services`等
> * `Repeatable`：新的instance上的程序运行环境及代码与其他instance保持一致
> * `Scalable`：最小化服务间的直接依赖，优雅扩容
>
> 本书使用`pattern based`方法来实现上面的特性，将通用的模式应用在不同的技术实现上，这些模式包括 
>
> * Core development pattern
> * Routing patterns
> * Client resiliency patterns
> * Security patterns
> * Logging and tracing patterns
> * Application metrics patterns
> * Build and deployment pattern

## 1.07 核心服务的开发模式（Core Microservice Development Pattern）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_svc_comm.jpg" width="600" /></div>
>
> 设计一个微服务时，要考虑该服务如何被消费和通信，包括：
>
> * `Service Gradularity`：如何把一个业务领域问题拆解为大小适度的微服务
>     * 拆解粒度太粗：会导致不同domain的业务重叠在一起，使得该服务难以维护和修改
>     * 拆解粒度太细：增加整体架构的复杂度，而该服务不能承担足够的计算逻辑（更像是一个数据存取层）
>
> * `Communication Protocals`：同步通信还是异步通信 
>     * 同步通信：使用XML、还是JSON、还是Thrift、Protobuffer这样的二进制通信
>     * 异步通信：AMQP、RabbitMQ、Kafka、还是SQS
> * `Interface Design`：第2章介绍相关的best  practice
> * `Configuration Management`：如何管理配置以便能够让微服务在不同的环境中迁移，第5章介绍于之相关的externalized configuration和profiles
> * `Event Processing between Services`：如何使用event来让服务解耦，第10章介绍与之相关的
>     Spring Cloud Stream

## 1.08 路由模式（Routing Patterns）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_routing_pattern.jpg" width="600" /></div>
>
> 微服务路由模式解决客户端需要调用某个`内部服务`时，如何能够发现并"路由"到该服务，包括：
>
> * `Service Discovery`：解决服务注册和服务发现、让客户端在不知道内部服务IP地址的前提下 ，访问 该服务。
>     * 本书使用Eureka，其他还包括etcd、Consul、Apache Zookeeper等，在第6章介绍
> * `Service Routing`：为所有内部服务提供统一的`Entry Point`、以便安全策略及路由规则可以统一部署
>     * 本书使用Spring Cloud API Gateway，在第8章介绍

## 1.09 客户端弹性模式（Client Resiliency Pattern）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_client_resiliency_pattern.jpg" width="600" /></div>
>
> `容错模式`用来防止单个内部服务（或单个节点）故障升级成系统故障并影响到用户，包括如下环节：
>
> * `Client Side Load Balancing`：涉及到如何缓存内部服务节点的位置
> * `Circuit Breakers Pattern`：有异常客户端过度频繁请求服务，或者某个内部服务性能不足时，使用fail fast保护系统并阻止频繁请求的客户端
> * `Fallback Pattern`：当服务调用失败时，提供一个机制让客户端通过其他方式完成工作
> * `Bulkhead Pattern`：内部服务之间的故障隔离
>
> 这些内容在第7章介绍

## 1.10 安全模式（Security Pattern）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_microsvc_security_pattern.jpg" width="600" /></div>
>
> `安全模式`用来保证只有被授权的请求才可以访问服务，包括：
>
> * `Authentication`：客户端身份认证 
> * `Authorization`：客户端权限检查
> * `Credentical Management and Propagation`：本书中采用token-based安全机制（例如 OAuth2或JSON Web  Tokens(JWT)）来获取token，并在service调用链上传递

## 1.11 微服务日志与跟踪模式（Logging and Tracing Pattern）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_logntrace_pattern.jpg" width="600" /></div>
>
> 微服务的一个难点是debug、tracing以及monitor非常困难（一个内部服务可以触发多个指向其他内部服务的调用），后续的章节会讲解如何使用`Spring Cloud Sleuth`、`Zipkin`以及`ELK`来实现分布式tracing，具体的pattern包括如下三个：
>
> * `Log correlation` ：用于把分散在众多内部服务中、但是属于同一user traction的用户请求的日志绑定在一起
> * `Log Aggregation`：用于日志收集聚合、统一存储查询
> * `Microservice Tracing`：可视化client  transaction flow在众多内部服务之间的运行状况

## 1.12 性能指标度量模式（Application Metrics Patterns）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_metrics_pattern.jpg" width="600" /></div>
>
> 用于防止潜在的性能问题，涉及性能指标数据收集/存储/查询/报警等，该模式会使用pull和push两种方式，由以下3个组件构成 ：
>
> * 组件1：`Metrics`、关于如何度量系统的性能
> * 组件2：`Metrics Service`、用于性能指标的存储和查询 
> * 组件3：`Metrics visualization suite`、可视化

## 1.13 开发和部署模式（Build and Deployment Patterns）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia_ch01_deploy_pattern.jpg" width="600" /></div>
>
> 关于如何构建deployment pipeline，在第12章介绍，包含如下内容：
>
> * `Build and Deployment Pipeline`：创建可重复使用的构建和部署流程、一键部署
>
> * `Infrastructure as code`：当微服务被打包时，自动构建镜像，并且这些镜像也像代码一样被保存起来
>
> * `Immutable Servers`：镜像一旦被创建、就不再会被改变
>
> * `Phoenix Servers`：如何处理ad hoc changes的情况。以及定期使用镜像重新部署、防止部署后容器发生改变并累积
>
>     > Phoenix servers. The longer a server is running, the more opportunity for configuration drift. How do you ensure that servers that run individual containers get torn down on a regular basis and recreated off an immutable image? A configuration drift could occur when ad hoc changes to a system configuration are unrecorded.
>     >
>     > Our goal with these patterns and topics is to ruthlessly expose and stamp out configuration drift as quickly as possible before it can hit your upper environments, such as stage or production.

## 1.14 小结

> 略

