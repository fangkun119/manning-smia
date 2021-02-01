[TOC]

# Spring Cloud

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
> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch2_pattern_support.jpg)

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

## 2.2 代码实例演示

> 这个演示项目还不能够运行（需要配置很多支持服务），主要是演示将`客户端负载均衡`和`服务发现`集成到一个Spring Boot REST Application中
>
> ~~~java
> @SpringBootApplication
> @RestController
> @RequestMapping(value="hello")
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
>         // RestTemplate
>         // (1) 使用传入的Logical Server ID，以便访问Erueka获得服务实例的物理位置
>         // (2) 使用Spring Cloud Load Balancer在客户端选择不同的服务实例，不使用集中式的负载均衡Server以避免单点故障
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

##  2.3 云原生微服务最佳实践

> <img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch2_12factor.jpg" style="zoom:65%;" />

### 2.3.01 代码库

> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_codebase.jpg)

### 2.3.02 依赖

> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_proj_dependencies.jpg)

###  2.3.03 配置

> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_config.jpg)



### 2.3.04 支持服务（Backing Services）

> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_backsvc.jpg)



### 2.3.05 构建、发布、运行

> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_release_proc.jpg)

### 2.3.06 无状态

> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_stateless.jpg)



### 2.3.07 端口绑定



### 2.3.08 并发

> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/sima2_ch02_concurrency.jpg)

### 2.3.09 Disposability



### 2.3.10 Dev/Prod parity



### 2.3.11 日志

> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/sima2_ch02_log.jpg)

### 2.3.12 Admin Processes



## 2.4 确保例子相关



##  2.5 使用Spring Boot和Java构建微服务



### 2.5.1 环境设置



### 2.5.2 编写项目轮廓（Skeleton Project）

> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_spring_initializer1.jpg)
>
> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_spring_initializer2.jpg)



> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_proj_structure.jpg)
>
> ![](https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_proj_dependencies.jpg)



### 2.5.3 编写Bootsrap类用来Booting Spring Boot Application

> 

## 2.6  小结

> 略