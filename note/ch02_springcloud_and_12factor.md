<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [Spring Cloud及Cloud Native应用最佳实践](#spring-cloud%E5%8F%8Acloud-native%E5%BA%94%E7%94%A8%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5)
  - [2.1 Spring Cloud技术体系](#21-spring-cloud%E6%8A%80%E6%9C%AF%E4%BD%93%E7%B3%BB)
    - [2.1.1 Spring Cloud Config](#211-spring-cloud-config)
    - [2.1.2 Spring Cloud Service Discovery](#212-spring-cloud-service-discovery)
    - [2.1.3 Spring Cloud Load Balancer以及Resilience4j](#213-spring-cloud-load-balancer%E4%BB%A5%E5%8F%8Aresilience4j)
    - [2.1.4 Spring Cloud API Gateway](#214-spring-cloud-api-gateway)
    - [2.1.5 Spring Cloud Stream](#215-spring-cloud-stream)
    - [2.1.6 Spring Cloud Sleuth](#216-spring-cloud-sleuth)
    - [2.1.7 Spring Cloud Security](#217-spring-cloud-security)
  - [2.2 代码演示](#22-%E4%BB%A3%E7%A0%81%E6%BC%94%E7%A4%BA)
  - [2.3 云原生微服务最佳实践（Twelve Factor App）](#23-%E4%BA%91%E5%8E%9F%E7%94%9F%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5twelve-factor-app)
    - [2.3.01 代码库](#2301-%E4%BB%A3%E7%A0%81%E5%BA%93)
    - [2.3.02 依赖](#2302-%E4%BE%9D%E8%B5%96)
    - [2.3.03 配置](#2303-%E9%85%8D%E7%BD%AE)
    - [2.3.04 支持服务（Backing Services）](#2304-%E6%94%AF%E6%8C%81%E6%9C%8D%E5%8A%A1backing-services)
    - [2.3.05 构建、发布、运行](#2305-%E6%9E%84%E5%BB%BA%E5%8F%91%E5%B8%83%E8%BF%90%E8%A1%8C)
    - [2.3.06 无状态](#2306-%E6%97%A0%E7%8A%B6%E6%80%81)
    - [2.3.07 端口绑定](#2307-%E7%AB%AF%E5%8F%A3%E7%BB%91%E5%AE%9A)
    - [2.3.08 并发](#2308-%E5%B9%B6%E5%8F%91)
    - [2.3.09 一次性（Disposability）](#2309-%E4%B8%80%E6%AC%A1%E6%80%A7disposability)
    - [2.3.10 环境等价性（Dev/Prod Parity）](#2310-%E7%8E%AF%E5%A2%83%E7%AD%89%E4%BB%B7%E6%80%A7devprod-parity)
    - [2.3.11 日志](#2311-%E6%97%A5%E5%BF%97)
    - [2.3.12 维护操作（Admin Processes）](#2312-%E7%BB%B4%E6%8A%A4%E6%93%8D%E4%BD%9Cadmin-processes)
  - [2.4 本书的项目例子](#24-%E6%9C%AC%E4%B9%A6%E7%9A%84%E9%A1%B9%E7%9B%AE%E4%BE%8B%E5%AD%90)
  - [2.5 使用Spring Boot和Java构建微服务](#25-%E4%BD%BF%E7%94%A8spring-boot%E5%92%8Cjava%E6%9E%84%E5%BB%BA%E5%BE%AE%E6%9C%8D%E5%8A%A1)
    - [2.5.1 环境设置](#251-%E7%8E%AF%E5%A2%83%E8%AE%BE%E7%BD%AE)
    - [2.5.2 编写项目轮廓（Skeleton Project）](#252-%E7%BC%96%E5%86%99%E9%A1%B9%E7%9B%AE%E8%BD%AE%E5%BB%93skeleton-project)
    - [2.5.3 编写Bootsrap类以引导应用程序启动](#253-%E7%BC%96%E5%86%99bootsrap%E7%B1%BB%E4%BB%A5%E5%BC%95%E5%AF%BC%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E5%90%AF%E5%8A%A8)
  - [2.6  小结](#26--%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Spring Cloud及Cloud Native应用最佳实践

> 系统越分散发生故障的位置越多，微服务的正确管理非常重要，也需要应用最佳实践保持架构尽可能高效和可扩展，这些需要用到Spring Cloud提供的各种功能（例如服务注册发现、断路器、监视等）
>
> 本章将从以下方面开始介绍：
>
> 1. Spring Cloud技术体系
>
> 2. 云原生应用开发原则
>
> 3. [12-Factor](https://12factor.net/zh_cn/)方法论的最佳实践
>
> 4. 使用Spring Cloud构建微服务

## 2.1 Spring Cloud技术体系

> [Spring Cloud](http://projects.spring.io/spring-cloud/)集成了大量经过实践检验的开源项目，提供（上一章中介绍的）最常见模式的解决方案，并简化了设置和配置。
>
> 下图是第一章中的模式，映射到的Spring Cloud项目
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch2_pattern_support.jpg" width="800" /></div>

### 2.1.1 Spring Cloud Config

> 用来保证程序的配置，与部署的微服务完全分开，不论启动多少微服务，他们使用拥有相同的配置。除了`Spring Cloud Config自己的属性管理存储库`，它还与以下开源项目集成：
>
> * Git：可以从Git读取应用程序的配置
> * Consul（[https://www.consul.io/](https://www.consul.io/)）：服务发现工具、拥有key-value数据库可以存储应用程序配置
> * Eureka（https://github.com/Netflix/erueke）：服务发现工具，同样拥有key-value数据库可以存储配置

### 2.1.2 Spring Cloud Service Discovery

> 功能：
>
> * 客户端可以使用服务的`逻辑名`从`服务发现`从中查询到服务器部署所在的`物理位置`（IP或服务器名称）
> * `服务发现`还可以在服务实例启动和关闭时对它们进行注册和注销
>
> 服务发现可以使用`Consul`、`Eureka`和`Zookeeper`，本章使用Eureka，在附录C和D中介绍Consul和Zookeeper

### 2.1.3 Spring Cloud Load Balancer以及Resilience4j

> 对于微服务客户端的`弹性模式`，Spring Cloud包装了Resilience4J（[https://github.com/resilience4j/resilience4j](https://github.com/resilience4j/resilience4j) ）和Spring Cloud Load balancer
>
> * `Resilience4J`：可以快速实现服务客户端的弹性模式，例如断路器（breaker）、重试（retry）、隔离（bulkhead）等
> * `Spring Cloud Load Balancer`：简化了与服务发现代理（例如Eureka）的集成，为service consumers提供了客户端负载均衡，在服务发现代理暂时不可用时也仍然可以支持客户端进行服务调用

### 2.1.4 Spring Cloud API Gateway

> API网关为系统提供给路由功能，代理服务请求，将服务调用集中化，以便试试标准的服务策略（如安全授权、身份验证、内容过滤、路由规则等）。可以使用：
>
> * `Zuul`（[https://github.com/Netflix/zuul](https://github.com/Netflix/zuul)）
> * `Spring Cloud Gateway`（[https://spring.io/projects/spring-cloud-gateway](https://spring.io/projects/spring-cloud-gateway)）
>
>  来实现。本书将使用Spring Cloud Gateway（由`Spring 5`、`Reactor`（与`Spring Web Flux`集成）和`Spring Boot 2`构建）。

### 2.1.5 Spring Cloud Stream

`Spring Cloud Strem`（[https://cloud.spring.io/spring-cloud-stream](https://cloud.spring.io/spring-cloud-stream)）

> 将消息队列集（例如RabbitMQ、Kafka）成到微服务中，处理异步事件

### 2.1.6 Spring Cloud Sleuth

`Spring Cloud Sleuth`：[https://cloud.spring.io/spring-cloud-sleuth/](https://cloud.spring.io/spring-cloud-sleuth/)

> 生成唯一的`跟踪ID`并集成到`HTTP调用`和`消息队列`中，打印在所有的日志记录中，以便跟踪事务在应用程序不同服务之间的流动。

与日志聚合、跟踪工具整合

> 将Spring Cloud Sleuth与日志聚合技术工具（例如ELK，https://www.elastic.co/what-is/elk-stack）和跟踪工具（例如Zipkin，[http://zipkin.io](http://zipkin.io/)）。可以可视化单个事务的服务调用流程

`ELK`

> * Elasticsearch（https://www.elastic.co/），搜索和分析引擎
> * Logstash（https://www.elastic.co/products/logstash），务器数据的端数据处理管道，对其进行转换以将其发送到“存储”
> * Kibana（https://www.elastic.co/products/kibana），客户端UI，允许用户查询和可视化整个堆栈的数据。

### 2.1.7 Spring Cloud Security

Spring Cloud Security（https://cloud.spring.io/spring-cloud-security/）

> 身份验证和授权框架，基于令牌
>
> 包括JSON Web令牌（https://jwt.io），标准化了OAuth2令牌创建方式的格式，并提供了对生成的令牌进行数字签名的标准

## 2.2 代码演示

> 这个演示项目还不能够运行（需要配置很多支持服务），主要是演示将`客户端负载均衡`和`服务发现`集成到一个Spring Boot REST Application中
>
> ~~~java
> @SpringBootApplication
> @RestController
> @RequestMapping(value="hello")
> // @EnableEurekaClient
> //（1）将这个Spring Boot Application注册到Eureka服务发现代理中
> //（2）引入spring-cloud-starter-netflix-eureka-client依赖时该注解为optional
> @EnableEurekaClient 
> public class Application {
> 	public static void main(String[] args) {
> 		SpringApplication.run(ContactServerAppApplication.class, args);
> 	}
> 
> 	public String helloRemoteServiceCall(String firstName,String lastName){
> 		RestTemplate restTemplate = new RestTemplate();
> 		// RestTemplate
> 		// (1) 使用传入的Logical Server ID，以便访问Erueka获得服务实例的物理位置
> 		// (2) 使用Spring Cloud Load Balancer在客户端选择不同的服务实例，不使用集中式的负载均衡Server以避免单点故障
> 		ResponseEntity<String> restExchange = restTemplate.exchange(
> 				"http://logical-service-id/name/{firstName}/{lastName}",
> 				HttpMethod.GET, null, 
> 				String.class, firstName, lastName);
> 		return restExchange.getBody();
> 	}
>  
> 	@RequestMapping(value="/{firstName}/{lastName}",method = RequestMethod.GET)
> 	public String hello(
> 		@PathVariable("firstName") String firstName, @PathVariable("lastName") String lastName) {
> 		return helloRemoteServiceCall(firstName, lastName);
> 	}
> }
> ~~~

##  2.3 云原生微服务最佳实践（Twelve Factor App）

两种云应用程序：(1) `Cloud-Ready`（2）`Cloud-Native`

> * `Cloud-Read`应用程序：已经能够可以直接在云环境中部署、例如他们的配置都已经外部化，而不再是仅能部署在特定的服务器上 
> * `Cloud-Native`应用程序：专门为云计算设计的应用程序，由可扩展组件（例如容器）构建而成，部署为微服务，通过持续交付工作流的DevOPs Pipeline在虚拟基础架构环境中进行管理

`Cloud Native`开发的四个原则

> * `DevOPs`：软件交付流程和基础架构变更的自动化
> * `微服务`：将大型代码库分解为细小，定义明确的片段，解决复杂性问题
> * `持续交付`：交付过程自动化，以支持短期交付
> * `容器`：将服务作为容器部署在云中

Heroku的最佳实践指南（称为"[Twelve Factor App](https://12factor.net/)"）

> 创建`Cloud Native`应用程序准寻的指南，12个factor分别对应12个主题、以及与之相关的最佳实践
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch2_12factor.jpg" width="500" /></di

### 2.3.01 代码库

> 每个微服务都应具有单个代码库（不仅是代码，独立存储的配置也应处于版本控制中），代码库可以具有多个部署环境实例（开发，测试，Staging，生产等），但不与任何其他微服务共享
>

### 2.3.02 依赖

> 使用构建工具（Maven、Gradle等）声明依赖项时，第三方依赖关系应使用版本号声明，确保始终以相同的版本库来构建微服务

###  2.3.03 配置

> `配置存储`独立于代码，避免在代码中添加嵌入式配置，而是将配置与可部署的微服务完全分开。微服务可以在启动时加载外部配置、可以在运行时重新加载而不必重启
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_config.jpg" width="800"/></div>

### 2.3.04 支持服务（Backing Services）

> 支持服务是指与微服务通信的数据库、API RESTful服务、消息队列等。部署的应用（Application Deploy）能够切换这些支持服务（例如从本地数据库切换到AWS RDS上的数据库时），而不需要代码修改。在第12章中将具体介绍。
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_backsvc.jpg" width="550" /></div>

### 2.3.05 构建、发布、运行

> 包含以下原则：
>
> * 构建、发布、Staging、运行阶段完全分开，代码更改需要经历整个过程
> * 发布阶段：将构建的服务、与针对各个环境的配置项结合
>
> 这样每一个在生产环境上部署过的版本，都可以在存储库中追溯
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_release_proc.jpg" width="650" /></div>

### 2.3.06 无状态

> 微服务应该设计为无状态的，即仅包含执行transaction所需要的信息，以做到任何时候kill或更换微服务节点，都不会担心数据丢失。
>
> 对状态存储的需求，硬通过内存缓存（例如Redis或后备数据库）来完成
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_stateless.jpg" width="350" /></div>

### 2.3.07 端口绑定

> * 仅通过特定端口发布服务，微服务完全独立于运行时引擎
> * 无需单独的Web或应用服务器，该服务可以在命令行上自行启动，并可以通过公开的HTTP端口立即访问

### 2.3.08 并发

> 应对并发量上升带来的性能问题时：应当使用横向扩展（添加更多实例，Scale Out）加负载均衡；而不是依赖纵向扩展（提升instance的性能，Scale Up）
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/sima2_ch02_concurrency.jpg" width="800" /></div>

### 2.3.09 一次性（Disposability）

> 微服务可以按需启动和停止，以促进弹性扩容并快速部署。
>
> 可以在不影响其他服务的情况下，用新实例替换故障实例。

### 2.3.10 环境等价性（Dev/Prod Parity）

> 应具有尽量相似的不同环境（例如Dev，Staging，Prod）以便控制部署和执行应用程序时可能遇到的各种情况
>
> * 这些环境始终包含已部署代码的相似版本，以及基础服务
> * 可以通过连续部署来完成，并做到自动化
> * 一旦代码完成，就因对其测试，要避免环境之间的代码版本差异过大

### 2.3.11 日志

> 日志应当作为事件流：
>
> * 应使用LogStash（[https://www.elastic.co/products/logstash](https://www.elastic.co/products/logstash)）或Fluentd（[[http://fluentd.org](http://fluentd.org/)]([http://fluentd.org](http://fluentd.org/))）等进行管理
> * 应当被收集并集中存储
> * 微服务不需要关心该过程、只需将日志输出
>
> 在第11章演示如何提供自动配置以将日志发送到ELK（Elasticsearch、Logstash和Kibana）
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/sima2_ch02_log.jpg" width="800" /></div>

### 2.3.12 维护操作（Admin Processes）

> 对于日常的维护操作（例如数据迁移、转换等），应当有操作脚本并且在代码库中保存，这些脚本应当可以重复使用并且在各个环境间通用。在第8章将会有具体演示。

## 2.4 本书的项目例子

> 本书提供确保与日常工作相关的Demo项目，它以一个`软件资产管理系统`为背景，将应用程序分解为更小的微服务设计，从手头的问题入手构建特定的微服务，然后使用各种Spring Cloud（及一些非Spring Cloud）技术创建支持这些服务的基础架构

##  2.5 使用Spring Boot和Java构建微服务

> 下一章的`软件许可证服务`的微服务代码，其构建过程在这一小节说明
>
> 项目代码：[../chapter3/licensing-service/](../chapter3/licensing-service/)

### 2.5.1 环境设置

> [Java 11](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html)，[Maven 3.5.4](https://maven.apache.org/download.cgi)或更高版本，IDE（[STS 4](https://spring.io/tools)、[Eclipse](https://www.eclipse.org/downloads)或[IntelliJ IDEA](https://www.jetbrains.com/idea/download/)）

### 2.5.2 编写项目轮廓（Skeleton Project）

> 此节的内容包括：
>
> * 使用Spring Initializer（[https://start.spring.io/](https://start.spring.io/)）或IDE中提供的类似的Initializer创建
>* 依赖的starter选择：Spring Web，Spring Boot Actuator
> * 项目目录结构、从starter引入的依赖（例如嵌入式tomcat，spring 5等）
>* pom.xml中各个配置的说明

<!-- <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_spring_initializer1.jpg" width="600" /></div> -->
<!-- <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_spring_initializer2.jpg" width="600" /></div> -->
<!-- <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_proj_structure.jpg" width="800" /></div> -->
<!-- <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_proj_dependencies.jpg" width="800" /></div> -->


### 2.5.3 编写Bootsrap类以引导应用程序启动

> 此小节内容为项目bootstrap类[LicenseServiceApplication.java](../chapter3/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java)的代码说明

## 2.6  小结

> 略