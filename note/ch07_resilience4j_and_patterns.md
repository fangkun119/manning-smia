<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [CH07 `Spring Cloud`和`Resilience4j`弹性模式](#ch07-spring-cloud%E5%92%8Cresilience4j%E5%BC%B9%E6%80%A7%E6%A8%A1%E5%BC%8F)
  - [7.01 四种弹性模式](#701-%E5%9B%9B%E7%A7%8D%E5%BC%B9%E6%80%A7%E6%A8%A1%E5%BC%8F)
    - [7.01.1 客户端负载均衡 （Client-Side Load Balancing）](#7011-%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1-client-side-load-balancing)
    - [7.01.2 断路器（Circuit Breaker）](#7012-%E6%96%AD%E8%B7%AF%E5%99%A8circuit-breaker)
    - [7.01.3 降级（Fallback Processing）](#7013-%E9%99%8D%E7%BA%A7fallback-processing)
    - [7.01.4 隔离（Bulkhead）](#7014-%E9%9A%94%E7%A6%BBbulkhead)
  - [7.02 为什么弹性模式很重要](#702-%E4%B8%BA%E4%BB%80%E4%B9%88%E5%BC%B9%E6%80%A7%E6%A8%A1%E5%BC%8F%E5%BE%88%E9%87%8D%E8%A6%81)
  - [7.03 `Resilience4j`介绍](#703-resilience4j%E4%BB%8B%E7%BB%8D)
  - [7.04 为`licensing-service`引入`Resilience4j`的依赖项](#704-%E4%B8%BAlicensing-service%E5%BC%95%E5%85%A5resilience4j%E7%9A%84%E4%BE%9D%E8%B5%96%E9%A1%B9)
  - [7.05 断路器（Circuit Breaker）](#705-%E6%96%AD%E8%B7%AF%E5%99%A8circuit-breaker)
    - [(1) Resilience4j断路器原理](#1-resilience4j%E6%96%AD%E8%B7%AF%E5%99%A8%E5%8E%9F%E7%90%86)
      - [`关闭`/`半开`/`打开`状态的切换](#%E5%85%B3%E9%97%AD%E5%8D%8A%E5%BC%80%E6%89%93%E5%BC%80%E7%8A%B6%E6%80%81%E7%9A%84%E5%88%87%E6%8D%A2)
      - [`禁用`/`强制打开`状态](#%E7%A6%81%E7%94%A8%E5%BC%BA%E5%88%B6%E6%89%93%E5%BC%80%E7%8A%B6%E6%80%81)
    - [(3) Demo项目：用Resilience4j包装对REST服务、以及对数据库的访问](#3-demo%E9%A1%B9%E7%9B%AE%E7%94%A8resilience4j%E5%8C%85%E8%A3%85%E5%AF%B9rest%E6%9C%8D%E5%8A%A1%E4%BB%A5%E5%8F%8A%E5%AF%B9%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E8%AE%BF%E9%97%AE)
    - [(4) `@CircuitBreaker`使用：包装对数据库的访问](#4-circuitbreaker%E4%BD%BF%E7%94%A8%E5%8C%85%E8%A3%85%E5%AF%B9%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E8%AE%BF%E9%97%AE)
    - [7.05.1 `@CircuitBreaker`使用：包装对REST请求的调用](#7051-circuitbreaker%E4%BD%BF%E7%94%A8%E5%8C%85%E8%A3%85%E5%AF%B9rest%E8%AF%B7%E6%B1%82%E7%9A%84%E8%B0%83%E7%94%A8)
      - [(1) 包装对REST请求的访问](#1-%E5%8C%85%E8%A3%85%E5%AF%B9rest%E8%AF%B7%E6%B1%82%E7%9A%84%E8%AE%BF%E9%97%AE)
      - [(2) 查看断路器配置的默认值](#2-%E6%9F%A5%E7%9C%8B%E6%96%AD%E8%B7%AF%E5%99%A8%E9%85%8D%E7%BD%AE%E7%9A%84%E9%BB%98%E8%AE%A4%E5%80%BC)
    - [7.05.2 自定义断路器设置](#7052-%E8%87%AA%E5%AE%9A%E4%B9%89%E6%96%AD%E8%B7%AF%E5%99%A8%E8%AE%BE%E7%BD%AE)
  - [7.06 降级处理（Fallback）](#706-%E9%99%8D%E7%BA%A7%E5%A4%84%E7%90%86fallback)
  - [7.07 隔离（Bulkhead）](#707-%E9%9A%94%E7%A6%BBbulkhead)
    - [(1) 问题及解决方法](#1-%E9%97%AE%E9%A2%98%E5%8F%8A%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95)
    - [(2) 基于信号量的资源隔离（默认实现）](#2-%E5%9F%BA%E4%BA%8E%E4%BF%A1%E5%8F%B7%E9%87%8F%E7%9A%84%E8%B5%84%E6%BA%90%E9%9A%94%E7%A6%BB%E9%BB%98%E8%AE%A4%E5%AE%9E%E7%8E%B0)
    - [(3) 基于线程池的资源隔离](#3-%E5%9F%BA%E4%BA%8E%E7%BA%BF%E7%A8%8B%E6%B1%A0%E7%9A%84%E8%B5%84%E6%BA%90%E9%9A%94%E7%A6%BB)
    - [(4) 代码编写](#4-%E4%BB%A3%E7%A0%81%E7%BC%96%E5%86%99)
  - [7.08 重试（Retry）](#708-%E9%87%8D%E8%AF%95retry)
  - [7.09 Rate Limitter](#709-rate-limitter)
    - [(1) 背景和原理](#1-%E8%83%8C%E6%99%AF%E5%92%8C%E5%8E%9F%E7%90%86)
      - [SemaphoreBasedRateLimiter](#semaphorebasedratelimiter)
      - [AtomicRateLimiter](#atomicratelimiter)
    - [(2) 配置](#2-%E9%85%8D%E7%BD%AE)
    - [(3) 代码](#3-%E4%BB%A3%E7%A0%81)
    - [(4) `bulkhead`对比`Rate Limiter`](#4-bulkhead%E5%AF%B9%E6%AF%94rate-limiter)
  - [7.10 Resilience4j与ThreadLocal的兼容性](#710-resilience4j%E4%B8%8Ethreadlocal%E7%9A%84%E5%85%BC%E5%AE%B9%E6%80%A7)
    - [(1) 例子场景](#1-%E4%BE%8B%E5%AD%90%E5%9C%BA%E6%99%AF)
    - [(2) 编写`UserContextHolder`以使用`ThreadLocal`来存储`UserContext`](#2-%E7%BC%96%E5%86%99usercontextholder%E4%BB%A5%E4%BD%BF%E7%94%A8threadlocal%E6%9D%A5%E5%AD%98%E5%82%A8usercontext)
    - [(3) 通过`ThreadLoacal<UserContext>`存储和使用数据](#3-%E9%80%9A%E8%BF%87threadloacalusercontext%E5%AD%98%E5%82%A8%E5%92%8C%E4%BD%BF%E7%94%A8%E6%95%B0%E6%8D%AE)
      - [在`UserContextFilter`中将数据存入`ThreadLocal<UserContext>`](#%E5%9C%A8usercontextfilter%E4%B8%AD%E5%B0%86%E6%95%B0%E6%8D%AE%E5%AD%98%E5%85%A5threadlocalusercontext)
      - [在LicenseController中使用数据](#%E5%9C%A8licensecontroller%E4%B8%AD%E4%BD%BF%E7%94%A8%E6%95%B0%E6%8D%AE)
      - [在LicenseService（使用@CircuitBreaker, @RateLimiter, @Retry, @Bulkhead注解）中使用数据](#%E5%9C%A8licenseservice%E4%BD%BF%E7%94%A8circuitbreaker-ratelimiter-retry-bulkhead%E6%B3%A8%E8%A7%A3%E4%B8%AD%E4%BD%BF%E7%94%A8%E6%95%B0%E6%8D%AE)
    - [(5) 测试](#5-%E6%B5%8B%E8%AF%95)
  - [7.11 小结](#711-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# CH07 `Spring Cloud`和`Resilience4j`弹性模式

> 构建弹性系统时，只考虑系统组件功能完全丧失的情况（构建冗余、集群化、负载均衡）是不够的。因为这些依赖于服务宕机被检测到，而有时服务崩溃是从性能下降开始的。性能下降不但难以检测，而且还会触发级联效应，导致多个应用程序崩溃。因此弹性模式对微服务非常关键，本章讲解4种弹性模式以及如何在Spring Cloud和Resilience4j中实现。

> 本章内容：
>
> - 断路器（circuit breaks），降级（fallbacks）和隔离模式（bulkheads）的实现
>     - 使用断路器（circuit breaks）来节省微服务客户端资源
>     - 使用Resilience4j处理远程服务失败
>     - 使用Resilience4j的隔离模式（bulkheads）隔离远程资源调用
> - Resilience4j断路器（circuit breaks）和隔离模式（bulkheads）调优
> - 定制Resilience4j的并发策略

> 原书章节在线阅读：[https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-7/v-8/](https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-7/v-8/)
>
> 本章Demo项目：[../chapter7/](../chapter7/)

## 7.01 四种弹性模式

>有4种弹性模式：1. 客户端负载均衡；2. 断路器；3. 降级；4. 隔离
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch07_resilience_patterns.jpg" width="650" /></div>
>
>这些模式的实现在逻辑上位于消耗远程资源的客户端，和资源本身之间

### 7.01.1 客户端负载均衡 （Client-Side Load Balancing）

> 客户端会缓存服务的实例列表、并从中选出一个发起调用，当检测到某个实例异常时，会从列表中剔除该实例。具体内容在上一章中介绍
>

### 7.01.2 断路器（Circuit Breaker）

> 单个远程服务处理时间过长，会被断路器中断；对于某个资源（服务），失败率过高时也会触发断路，通过`fast fail` 来阻止对该资源的访问

### 7.01.3 降级（Fallback Processing）

> 远程服务失败时，使用后备的模式（例如将用户请求放入一个队列并通知用户以后会处理；在商品推荐页面降低推荐的精度以节省计算资源等），用服务质量换取计算性能，提供降级服务来避免系统崩溃
>

### 7.01.4 隔离（Bulkhead）

> 为不同的远程资源调用，创建不同的线程池，使其相互隔离。当某个资源响应缓慢时，不会影响对其他资源的调用。
>

## 7.02 为什么弹性模式很重要

未使用`弹性模式`的系统（如下图）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch07_without_resilience_patterns.jpg" width="800" /></div>
>
> 例如NAS上的某个修改导致了特定磁盘读取换面 → 引发Inventory Service响应缓慢 → 导致Oragnization Service响应缓慢 → 导致Licensing Service处理缓慢 → 导致Licensing占用的数据库连接不能释放 → 导致数据库连接资源被耗尽 → 最终三个服务全部停止响应 

使用`弹性模式`（如下图）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch07_circuit_breaker.jpg" width="800" /></div>
>
> 演示了`orgnization-service`对`inventory-service`的调用的三种情况：
>
> `左`：没有超时，一切正常
>
> `中`：单个调用超时，调用终止；当发生超时的调用达到一定数量之后，不再对organization-service进行调用（`Fail-Fast`），防止整个应用资源耗尽
>
> `右`：调用超时达到一定数量后，后退到降级方案（`Fail Gracefully`），为大部分请求使用替代数据（少量用来检查inventory-service的状态），给inventory-service得以喘息和恢复的时间，待inventory-service状态恢复后断路器将自行重置（`Recover seamlessly`)

## 7.03 `Resilience4j`介绍

> `Resilience4j`是受`Hystrix`启发库（替代已经处于维护模式的Hystrix），提供以下功能来支持弹性模式：
>
> * 断路器
> * 重试
> * 隔离（限制对服务的并发请求数、避免过载）
> * Rate Limiter（限制服务一次接收的请求书）
> * 降级

## 7.04 为`licensing-service`引入`Resilience4j`的依赖项

> ~~~xml
> <properties>
> 	...
> 	<resilience4j.version>1.5.0</resilience4j.version>
> </properties>
> <dependencies>
> 	...
> 	<!-- 使用弹性模式的注解 -->
> 	<dependency>
> 		<groupId>io.github.resilience4j</groupId>
> 		<artifactId>resilience4j-spring-boot2</artifactId>
> 		<version>${resilience4j.version}</version>
> 	</dependency>
> 	<!-- 断路器 -->
> 	<dependency>
> 		<groupId>io.github.resilience4j</groupId>
> 		<artifactId>resilience4j-circuitbreaker</artifactId>
> 		<version>${resilience4j.version}</version>
> 	</dependency>
> 	<!-- Time Limiter -->
> 	<dependency>
> 		<groupId>io.github.resilience4j</groupId>
> 		<artifactId>resilience4j-timelimiter</artifactId>
> 		<version>${resilience4j.version}</version>
> 	</dependency>
> 	<!-- resilience4j依赖AOP -->
> 	<dependency>
> 		<groupId>org.springframework.boot</groupId>
> 		<artifactId>spring-boot-starter-aop</artifactId>
> 	</dependency>
> </dependencies>
> ~~~
>
> 完整代码：[../chapter7/licensing-service/pom.xml](../chapter7/licensing-service/pom.xml)

## 7.05 断路器（Circuit Breaker）

### (1) Resilience4j断路器原理

#### `关闭`/`半开`/`打开`状态的切换

> 服务整体故障状况，采用以下状态机（断路关闭、断路打开、断路半开）来监控：`断路关闭` → (请求失败率上升但低于阈值) → `断路半关` → (请求失败率高于阈值) → `断路打开`
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch07_resilience4j_status.jpg" width="650" /></div>
>
> 请求失败率，使用`环形位缓冲`来计算，0表示请求成功，1表示请求失败，当环已满时开始计算失败率
>
> 
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/sima2_ch07_resilience4j_buffer.jpg" width="400" /></div>
>
> `断路器打开`：在指定时间内所有请求都被拒绝、并抛出`CallNotpermittedException`
>
> `断路器半开`：使用另一个可配置的`环形位缓冲`来评估故障率，以决定是将状态变回`打开`，还是变为`关闭`

#### `禁用`/`强制打开`状态 

> 除了`打开`、`半开`、`关闭`，还可以定义另外2个状态：`已禁用`、`强制打开`。退出这两个状态需要重置断路器或者触发状态转换。
>
> 这两个状态不在本书讨论范围内 ，可参考：[https://resilience4j.readme.io/v0.17.0/docs/circuitbreaker](https://resilience4j.readme.io/v0.17.0/docs/circuitbreaker)

### (3) Demo项目：用Resilience4j包装对REST服务、以及对数据库的访问

> 包装对REST服务、以及数据库的访问，方法相同，都是使用Resilience4j注解即可
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch07_resislience4j_protection.jpg" width="650" /></div>

### (4) `@CircuitBreaker`使用：包装对数据库的访问

代码

> Spring框架会为使用`@CircuitBeaker`注解Java类方法动态生成一个代理 ，该代理将包装这个方法，并通过专用的线程池管理对该方法的调用
>
> ~~~java
> ...
> @CircuitBreaker(name = "licenseService")
> public List<License> getLicensesByOrganization(String organizationId) {
> 	return licenseRepository.findByOrganizationId(organizationId);
> }
> ...
> ~~~
>
> 完整代码：[../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)

故障模拟

> 让这个方法以一定的概率超时
>
> ~~~java
> @CircuitBreaker(name = "licenseService")   
> public List<License> getLicensesByOrganization(String organizationId) {
>     // 以1/3的概率随机sleep 5秒
> 	randomlyRunLong();
> 	return licenseRepository.findByOrganizationId(organizationId);
> }
> ~~~
>
> 多次访问`http://localhost8080/v1/organization/e6a625cc-718b-48c2-ac76-1dfdff9a531e/license/`，有一定的概率发生下面错误
>
> ~~~json
> {
> 	"timestamp": 1595178498383,
> 	"status": 500,
> 	"error": "Internal Server Error",
> 	"message": "No message available",
> 	"path": "/v1/organization/e6a625cc-718b-48c2-ac76-1dfdff9a531e/license/"
> }
> ~~~
>
> 当请求次数达到一定数量，环形位缓冲被填满，会收到下面的错误表示断路器处于`断路开启`状态
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch07_breaker_error.jpg" width="800" /></div>
>

### 7.05.1 `@CircuitBreaker`使用：包装对REST请求的调用

#### (1) 包装对REST请求的访问

> 与包装对数据库的访问相同，例如
>
> ~~~java
> @CircuitBreaker(name = "organizationService")   
> private Organization getOrganization(String organizationId) {
> 	return organizationRestClient.getOrganization(organizationId);
> }
> ~~~

#### (2) 查看断路器配置的默认值

> [`http://<host>:<service_port>/actuator/health`](http://<host>:<service_port>/actuator/health)

### 7.05.2 自定义断路器设置

> 添加如下自定义的resilience4j配置以覆盖默认配置，例如为`licensing-service`添加配置，那么可以添加在这个service在`config-server`的配置存储中
>
> ~~~yml
> ...
> 
> # 为断路器编写的配置
> resilience4j.circuitbreaker:
> 	instances:
> 		licenseService:        # 对应于@CircuitBreaker(name = "licenseService")中的name属性
> 			registerHealthIndicator: true	 # 是否将配置暴露在/actuator/health端点中
> 			ringBufferSizeInClosedState: 5   # `断路关闭`状态时`环形位缓冲`的大小，默认100
> 			ringBufferSizeInHalfOpenState: 3 # `断路半开`状态时`环形位缓冲`的大小，默认10
> 			waitDurationInOpenState: 10s     # 进入`断路打开`后下一次评估失败率之前的等待时长，默认60000ms
> 			failureRateThreshold: 50         # 失败率阈值，默认50
> 			recordExceptions:				 # 视为请求失败的异常类型，默认所有异常都视为请求失败
> 				- org.springframework.web.client.HttpServerErrorException
> 				- java.io.IOException
> 				- java.util.concurrent.TimeoutException
> 				- org.springframework.web.client.ResourceAccessException
> 		organizationService:  # 对应于@CircuitBreaker(name = "organizationService")中的name属性 
> 			registerHealthIndicator: true
> 			ringBufferSizeInClosedState: 6
> 			ringBufferSizeInHalfOpenState: 4
> 			waitDurationInOpenState: 20s
> 			failureRateThreshold: 60
> ~~~
>
> 完整的配置列表：[https://resilience4j.readme.io/docs/circuitbreaker](https://resilience4j.readme.io/docs/circuitbreaker) 
>
> 本章Demo的配置：[../chapter7/licensing-service/src/main/resources/bootstrap.yml](../chapter7/licensing-service/src/main/resources/bootstrap.yml) （其实更推配在config-server的配置存储中，而不是陪在licensing-service的本地配置文件中）

## 7.06 降级处理（Fallback）

代码

> ```java
> // 降级处理的方法：由@CircuitBreaker注解的fallbackMethod属性用来指定
> @CircuitBreaker(name = "licenseService", fallbackMethod = "buildFallbackLicenseList")
> public List<License> getLicensesByOrganization(String organizationId) throws TimeoutException {
> 	logger.debug("getLicensesByOrganization Correlation id: {}", UserContextHolder.getContext().getCorrelationId());
> 	randomlyRunLong(); // 让原始方法有1/3的概率超时
> 	return licenseRepository.findByOrganizationId(organizationId);
> }
> 
> // 该降级方法（buildFallbackLicenseList）：
> // (1) 与原始方法（getLicensesByOrganization）位于同一个类
> // (2) 包含与原始方法一致的签名（参数、返回值）以及一个额外的参数（用来表示原始方法遇到的异常参数）
> @SuppressWarnings("unused")
> private List<License> buildFallbackLicenseList(String organizationId, Throwable t){
> 	List<License> fallbackList = new ArrayList<>();
> 	License license = new License();
> 	license.setLicenseId("0000000-00-00000");
> 	license.setOrganizationId(organizationId);
> 	license.setProductName("Sorry no licensing information currently available");
> 	fallbackList.add(license);
> 	return fallbackList;
> }
> ```
>
> 完整代码：[../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)

测试

> 多次访问`http://localhost8080/v1/organization/e6a625cc-718b-48c2-ac76-1dfdff9a531e/license/`，有一定的概率因超时而降级为`buildFallbackLicenseList`的返回值 
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch07_resilience4j_breaker_fallback.jpg" width="800" /></div>
>

## 7.07 隔离（Bulkhead）

###  (1) 问题及解决方法

问题

> 微服务应用中，通常需要调用多个服务来完成一项操作，如果不使用`Bulkhead Pattern`，这些调用会使用同一个线程池。在高并发的场景下，某个下游服务故障无响应，会导致整个容器的线程池被占用，并阻塞其他请求的处理，最终服务崩溃。

Resilience4J提供了两种不同的`Bulkhead Pattern`实现

> (1) 基于信号量的资源隔离
>
> (2) 基于线程池的资源隔离

### (2) 基于信号量的资源隔离（默认实现）

方法

> 用信号量限制对服务的并发调用上限，（虽然对各个服务的调用共享同一个线程池、但是）可以控制并发量，一旦达到并发上限，就会开始拒绝请求。
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch07_resilience4j_semaphore.jpg" width="500" /></div>

优缺点

> 适用场景：访问少量远程服务，各个服务的呼叫量相对均匀
>
> 缺点：某个服务的吞吐量或者处理时间显著高于其他服务时，这个服务会占据几乎所有的线程，导致线程池被耗尽

### (3) 基于线程池的资源隔离

> 为每个服务使用一套有限队列和固定线程池，队列满时拒绝请求
>
> 可以真正做到资源隔离，并且在请求密集时也可以靠队列提供一定的缓冲
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/sima2_ch07_resilicene4j_with_threadpool.jpg" width="800" /></div>
>

###  (4) 代码编写 

配置

> ~~~yml
> # 为基于信号量的资源隔离编写的配置
> resilience4j.bulkhead:
> 	instances:
> 		# 对应于@Bulkhead(name= "bulkheadLicenseService", ...)中的bulkheadLicenseService属性
> 		bulkheadLicenseService:
> 			maxWaitDuration: 10ms	# 线程调用服务之前、最长可以被阻塞多长时间，默认值是0）
> 			maxConcurrentCalls: 20	# 最大并发量，默认值是25
> 
> # 为基于线程池的资源隔离编写的配置
> resilience4j.thread-pool-bulkhead:
> 	instances:
> 		# 对应于@Bulkhead(name= "bulkheadLicenseService", ...)中的bulkheadLicenseService属性
> 		bulkheadLicenseService: 
> 			maxThreadPoolSize: 1	# 最大线程池大小，默认值为Runtime.getRuntime().availableProcessors()
> 			coreThreadPoolSize: 1	# 核心线程池大小，默认值为Runtime.getRuntime().availableProcessors() - 1
> 			queueCapacity: 1		# 队列容量，默认值为100
> 			keepAliveDuration: 20ms	# 空闲线程在线程终止之前、等待新任务的最长时间（当线程数大于核心线程池时，会发生这种情况），默认值是20ms
> ~~~
>
> 完整代码：[../chapter7/licensing-service/src/main/resources/bootstrap.yml](../chapter7/licensing-service/src/main/resources/bootstrap.yml) （其实更推配在config-server的配置存储中，而不是陪在licensing-service的本地配置文件中）

线程池大小该如何配置

> ~~~python
> # qps_peak 		: requests per second at peak when the service is healthy 
> # latency_99pct : 99th percentile latency in seconds
> # overhead 		: small amount of extra threads for overhead
> thread_pool_size = (qps_peak * latency_99pct) + overhead
> ~~~
>
> 线程池大小是重要配置，服务部署之后，也应当关注超时情况，进行调整

创建基于信号量的资源隔离

> ~~~java
> @Bulkhead(name= "bulkheadLicenseService", fallbackMethod= "buildFallbackLicenseList")
> public List<License> getLicensesByOrganization(String organizationId) throws TimeoutException {
> 	logger.debug("getLicensesByOrganization Correlation id: {}",UserContextHolder.getContext().getCorrelationId());
> 	randomlyRunLong(); // 有1/3的概率触发调用超时
> 	return licenseRepository.findByOrganizationId(organizationId);
> }
> ~~~
>
> `@Bulkhead`注解中未指定`type`属性时、会使用默认的基于信号量的方式来实现资源隔离

创建基于线程池的资源隔离

> ~~~java
> @Bulkhead(
> 	name = "bulkheadLicenseService", 
> 	type = Bulkhead.Type.THREADPOOL, 
> 	fallbackMethod = "buildFallbackLicenseList") 
> public List<License> getLicensesByOrganization(String organizationId) throws TimeoutException {
> 	logger.debug("getLicensesByOrganization Correlation id: {}",UserContextHolder.getContext().getCorrelationId());
> 	randomlyRunLong(); // 有1/3的概率触发调用超时
> 	return licenseRepository.findByOrganizationId(organizationId);
> }
> ~~~
>
> `@Bulkhead`注解中指定`type=Bulkhead.Type.THREADPOOL`，来创建基于线程池的资源隔离

完整代码：[../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)

## 7.08 重试（Retry）

> 在服务因网络中断等暂时失败时，进行 重试

配置：

> ~~~yml
> # 为重试编写的配置
> resilience4j.retry:
> 	instances:
> 		# 对应于@Retry(name = "retryLicenseService", ...)中的name属性
> 		retryLicenseService:
> 			maxRetryAttempts: 5		# 最大重试次数
> 			waitDuration: 10000		# 重试等待时长，默认值500ms
> 			retry-exceptions:		# 可以触发重试的异常列表，默认值为空
> 				- java.util.concurrent.TimeoutException
> ~~~
>
> 代码：[../chapter7/licensing-service/src/main/resources/bootstrap.yml](../chapter7/licensing-service/src/main/resources/bootstrap.yml) （其实更推配在config-server的配置存储中，而不是陪在licensing-service的本地配置文件中）
>
> 补充：除了上面三个配置，还有更多的选项可以使用
>
> * `intervalFunction`：设置用于计算等待时长的函数 
> * `retryOnResultPredicate`：设置一个函数，该函数返回true时触发重试
> * `retryOnExceptionPredicate`：设置一个函数，该函数对一个exception进行评估，返回true时触发重试
> * `ignoreExceptions`：被忽略且不会重试的exception列表，默认值为null

代码：

> ~~~java
> @Retry(name = "retryLicenseService", fallbackMethod = "buildFallbackLicenseList")
> public List<License> getLicensesByOrganization(String organizationId) throws TimeoutException {
> 	logger.debug("getLicensesByOrganization Correlation id: {}", UserContextHolder.getContext().getCorrelationId());
> 	randomlyRunLong(); // 有1/3的概率触发调用超时
> 	return licenseRepository.findByOrganizationId(organizationId);
> }
> ~~~
>
> 完整代码：[../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)

##  7.09 Rate Limitter

### (1) 背景和原理

> 控制对下游服务的压力，也是很重要的事情。`resilience4j`提供了两种rate limiter：(1) SemaphoreBasedRateLimiter (2) AtomicRateLimiter

#### SemaphoreBasedRateLimiter

> 请求下游服务的线程需要先获取信号量（semaphore.tryAcquire）才能发起请求
>
> 同时会有另一个线程，定期 （在每一个新的limitRefreshPeriod启动时）释放信号量（semaphore.release）触发对另一线程的调用 

####  AtomicRateLimiter

> 由用户线程自己来进行请求频次控制，它以纳秒级别来把时间分隔成一些列`相等`的周期（cycle、refresh period），在每个周期开始重置请求发送许可次数（permissions）。其中涉及到如下术语：
>
> * `ActiveCycle`：上次发送请求时的周期ID
> * `ActivePermissions`：上次发送请求时还剩下的available permissions
> * `NanosToWait`：上次发送请求时等待available permission的纳秒数
>
> 当available permissions不够时，会根据current permissions来计算还要等待多长时间、才能获得足够的permissions

### (2) 配置

> ~~~yml
> resilience4j.ratelimiter:
> 	instances:
> 		licenseService:
> 			timeoutDuration: 1000ms   # 允许等待许可的时间长度、默认值为5s
> 			limitRefreshPeriod: 5000  # permission刷新的时间间隔、默认值为500ns（纳秒）
> 			limitForPeriod: 5		  # 每次刷新后设置的permission数量，默认值为50
> ~~~
>
> 代码：[../chapter7/licensing-service/src/main/resources/bootstrap.yml](../chapter7/licensing-service/src/main/resources/bootstrap.yml) （其实更推配在config-server的配置存储中，而不是陪在licensing-service的本地配置文件中）

### (3) 代码

> ~~~java
> @RateLimiter(name = "licenseService", fallbackMethod = "buildFallbackLicenseList")
> public List<License> getLicensesByOrganization(String organizationId) throws TimeoutException {
>    logger.debug("getLicensesByOrganization Correlation id: {}", UserContextHolder.getContext().getCorrelationId());
>    randomlyRunLong(); // 以1/3的概率触发调用超时
>    return licenseRepository.findByOrganizationId(organizationId);
> }
> ~~~
>
> 完整代码：[../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter7/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)

### (4) `bulkhead`对比`Rate Limiter`

> `bulkhead`限制一次并发请求的并发量；而`Rate Limiter`限制规定时间内的总请求量（例如每Y秒容许请求X次）。需要仔细检查需求来决定使用哪一种

## 7.10 Resilience4j与ThreadLocal的兼容性

### (1) 例子场景

> 1. 使用Filter类来拦截对REST服务的调用，从HTTP请求中获取`用于追踪REST挑用的correlation_id`以及`身份验证token`
> 2. 将获取的数据存储在`UserContext`中
> 3. 下游代码需要使用这些变量时会从`Tread local storage varaible`中取出`UserContext`再取出所需要的变量

### (2) 编写`UserContextHolder`以使用`ThreadLocal`来存储`UserContext`

> UserContextHolder使用ThreadLocal来存储UserContext，所有的方法都是静态方法
>
> ~~~java
> public class UserContextHolder {
> 	// 用ThreadLocal来存储UserContext
> 	private static final 
> 		ThreadLocal<UserContext> userContext = new ThreadLocal<UserContext>();
> 
> 	public static final UserContext getContext(){
> 		UserContext context = userContext.get();
> 		if (context == null) {
> 			context = createEmptyContext();
> 			userContext.set(context); 
> 		}
> 		return userContext.get();
> 	}
> 
> 	public static final void setContext(UserContext context) {
> 		userContext.set(context);
> 	}
> 
> 	public static final UserContext createEmptyContext(){
> 		return new UserContext();
> 	}
> }
> ~~~
>
> 代码：[../chapter7/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextHolder.java](../chapter7/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextHolder.java)
>
> UserContext是一个使用@Component注解用来存储数据的POJO
>
> ~~~java
> @Component
> public class UserContext {
> 	private String correlationId= new String();
> 	private String authToken= new String();
> 	private String userId = new String();
> 	private String organizationId = new String();
>  
> 	public String getCorrelationId() { 
> 		return correlationId;
> 	}
> 	public void setCorrelationId(String correlationId) {
> 		this.correlationId = correlationId;
> 	}
> 
> 	...
> }
> ~~~
>
> 代码：[../chapter7/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContext.java](../chapter7/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContext.java)

### (3) 通过`ThreadLoacal<UserContext>`存储和使用数据 

#### 在`UserContextFilter`中将数据存入`ThreadLocal<UserContext>`

> ~~~java
> @Component
> public class UserContextFilter implements Filter {
> 	...
> 	@Override
> 	public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
> 		// 获取HttpServletRequest
> 		HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
> 		// 调用自己编写的UserContextHolder封装类，存储从http请求中提取的数据，包括HTTP Header中的
> 		// * "tmx-correlation-id"
> 		// * "tmx-auth-token"
> 		// * "tmx-user-id"
> 		// * "tmx-organization-id"
> 		UserContextHolder.getContext().setCorrelationId(httpServletRequest.getHeader(UserContext.CORRELATION_ID));
> 		UserContextHolder.getContext().setUserId(httpServletRequest.getHeader(UserContext.USER_ID));
> 		UserContextHolder.getContext().setAuthToken(httpServletRequest.getHeader(UserContext.AUTH_TOKEN));
> 		UserContextHolder.getContext().setOrganizationId(httpServletRequest.getHeader(UserContext.ORGANIZATION_ID));
> 		// 打印日志
> 		logger.debug("UserContextFilter Correlation id: {}", UserContextHolder.getContext().getCorrelationId());
> 		// 让框架继续执行后续的操作
> 		filterChain.doFilter(httpServletRequest, servletResponse);
> 	}
> 	...
> }
> ~~~
>
> 完整代码：[../chapter7/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextFilter.java](../chapter7/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextFilter.java)

#### 在LicenseController中使用数据

> ~~~java
> @RequestMapping(value="/",method = RequestMethod.GET)
> public List<License> getLicenses(@PathVariable("organizationId") String organizationId) throws TimeoutException {
> 	logger.debug(
> 		"LicenseServiceController Correlation id: {}", 
>         // 从ThreadLocal<UserContext>中获取数据
> 		UserContextHolder.getContext().getCorrelationId());
> 	return licenseService.getLicensesByOrganization(organizationId);
> }
> ~~~
>
> 代码：[../chapter7/licensing-service/src/main/java/com/optimagrowth/license/controller/LicenseController.java](../chapter7/licensing-service/src/main/java/com/optimagrowth/license/controller/LicenseController.java)

#### 在LicenseService（使用@CircuitBreaker, @RateLimiter, @Retry, @Bulkhead注解）中使用数据

> ~~~java
> // 这个方法使用了resilience4j的注解
> @CircuitBreaker(name = "licenseService", fallbackMethod = "buildFallbackLicenseList")
> @RateLimiter(name = "licenseService", fallbackMethod = "buildFallbackLicenseList")
> @Retry(name = "retryLicenseService", fallbackMethod = "buildFallbackLicenseList")
> @Bulkhead(name = "bulkheadLicenseService", type= Type.THREADPOOL, fallbackMethod = "buildFallbackLicenseList")
> public List<License> getLicensesByOrganization(String organizationId) throws TimeoutException {
> 	logger.debug(
> 		"getLicensesByOrganization Correlation id: {}", 
>         // 从ThreadLocal<UserContext>中获取数据
> 		UserContextHolder.getContext().getCorrelationId());
> 	randomlyRunLong(); //以1/3的概率触发超时，
> 	return licenseRepository.findByOrganizationId(organizationId);
> }
> ~~~

### (5) 测试

> 打印DEBUG日志：[../chapter7/licensing-service/src/main/resources/bootstrap.yml](../chapter7/licensing-service/src/main/resources/bootstrap.yml)
>
> ~~~yml
> logging:
> 	level:
> 		org.springframework.web: WARN
> 		com.optimagrowth: DEBUG
> ~~~
>
> 重新编译、创建镜像和容器 
>
> ~~~bash
> mvn clean package dockerfile:build
> docker-compose -f docker/docker-compose.yml up
> ~~~
>
> 测试
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch07_correlation_id_in_http_header.jpg" width="800" /></div>
>
> 在日志中可以看到，三处日志打印
>
> ~~~bash
> licensingservice_1     | 2021-02-10 11:00:34.162  INFO 1 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
> licensingservice_1     | 2021-02-10 11:00:34.274 DEBUG 1 --- [nio-8080-exec-1] c.o.license.utils.UserContextFilter      : UserContextFilter Correlation id: TEST-CORRELATION_ID
> licensingservice_1     | 2021-02-10 11:00:34.372 DEBUG 1 --- [nio-8080-exec-1] c.o.l.controller.LicenseController       : LicenseServiceController Correlation id: TEST-CORRELATION_ID
> licensingservice_1     | 2021-02-10 11:00:34.437 DEBUG 1 --- [nio-8080-exec-1] c.o.license.service.LicenseService       : getLicensesByOrganization Correlation id: TEST-CORRELATION_ID
> ~~~
>
> 可以看到即使在调用被resilience4j注解包裹的方法，虽然这些方法在其他线程中被执行，但correlation id仍然可以被正确打印，说明resilience4j从父线程中将这些值传递了过来

## 7.11 小结

> 略