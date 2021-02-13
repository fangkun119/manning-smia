<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [CH08 Spring Cloud Gateway](#ch08-spring-cloud-gateway)
  - [8.1 Gateway](#81-gateway)
    - [(1) 服务调用方式：不使用网关 v.s 使用网关](#1-%E6%9C%8D%E5%8A%A1%E8%B0%83%E7%94%A8%E6%96%B9%E5%BC%8F%E4%B8%8D%E4%BD%BF%E7%94%A8%E7%BD%91%E5%85%B3-vs-%E4%BD%BF%E7%94%A8%E7%BD%91%E5%85%B3)
    - [(2) 中央策略执行点（PEP：Policy Enforcement Point）功能](#2-%E4%B8%AD%E5%A4%AE%E7%AD%96%E7%95%A5%E6%89%A7%E8%A1%8C%E7%82%B9peppolicy-enforcement-point%E5%8A%9F%E8%83%BD)
    - [(3) 防止Gateway单点故障](#3-%E9%98%B2%E6%AD%A2gateway%E5%8D%95%E7%82%B9%E6%95%85%E9%9A%9C)
  - [8.2 Spring Cloud Gateway](#82-spring-cloud-gateway)
    - [8.2.1 创建Spring Cloud Gateway模块](#821-%E5%88%9B%E5%BB%BAspring-cloud-gateway%E6%A8%A1%E5%9D%97)
      - [(1) 创建gateway-server](#1-%E5%88%9B%E5%BB%BAgateway-server)
      - [(2) 本地配置](#2-%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE)
    - [8.2.2 配置gateway-server与Eureka通信](#822-%E9%85%8D%E7%BD%AEgateway-server%E4%B8%8Eeureka%E9%80%9A%E4%BF%A1)
      - [(1) 在`config-server`配置gateway-server让它将自己注册到Eureka中](#1-%E5%9C%A8config-server%E9%85%8D%E7%BD%AEgateway-server%E8%AE%A9%E5%AE%83%E5%B0%86%E8%87%AA%E5%B7%B1%E6%B3%A8%E5%86%8C%E5%88%B0eureka%E4%B8%AD)
      - [(2) Spring Boot引导类](#2-spring-boot%E5%BC%95%E5%AF%BC%E7%B1%BB)
      - [(3) 改用`Spring Cloud Load Balancer`来实现负载均衡](#3-%E6%94%B9%E7%94%A8spring-cloud-load-balancer%E6%9D%A5%E5%AE%9E%E7%8E%B0%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1)
  - [8.3 在Spring Cloud Gateway中配置路由](#83-%E5%9C%A8spring-cloud-gateway%E4%B8%AD%E9%85%8D%E7%BD%AE%E8%B7%AF%E7%94%B1)
    - [8.3.1 通过服务发现自动映射路由](#831-%E9%80%9A%E8%BF%87%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0%E8%87%AA%E5%8A%A8%E6%98%A0%E5%B0%84%E8%B7%AF%E7%94%B1)
      - [(1) 开启自动映射](#1-%E5%BC%80%E5%90%AF%E8%87%AA%E5%8A%A8%E6%98%A0%E5%B0%84)
      - [(2) 路由映射规则](#2-%E8%B7%AF%E7%94%B1%E6%98%A0%E5%B0%84%E8%A7%84%E5%88%99)
      - [(3) 自动支持Instance增加和删除、自动支持新Service注册](#3-%E8%87%AA%E5%8A%A8%E6%94%AF%E6%8C%81instance%E5%A2%9E%E5%8A%A0%E5%92%8C%E5%88%A0%E9%99%A4%E8%87%AA%E5%8A%A8%E6%94%AF%E6%8C%81%E6%96%B0service%E6%B3%A8%E5%86%8C)
      - [(4) 查看Gateway管理的路由](#4-%E6%9F%A5%E7%9C%8Bgateway%E7%AE%A1%E7%90%86%E7%9A%84%E8%B7%AF%E7%94%B1)
        - [(a) 服务启动及状态观察](#a-%E6%9C%8D%E5%8A%A1%E5%90%AF%E5%8A%A8%E5%8F%8A%E7%8A%B6%E6%80%81%E8%A7%82%E5%AF%9F)
        - [(b) 查看路由](#b-%E6%9F%A5%E7%9C%8B%E8%B7%AF%E7%94%B1)
    - [8.3.2 使用服务发现手动映射路由](#832-%E4%BD%BF%E7%94%A8%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0%E6%89%8B%E5%8A%A8%E6%98%A0%E5%B0%84%E8%B7%AF%E7%94%B1)
      - [(1) 用途](#1-%E7%94%A8%E9%80%94)
      - [(2) 添加手动路由配置](#2-%E6%B7%BB%E5%8A%A0%E6%89%8B%E5%8A%A8%E8%B7%AF%E7%94%B1%E9%85%8D%E7%BD%AE)
      - [(3) 查看Gateway管理的路由](#3-%E6%9F%A5%E7%9C%8Bgateway%E7%AE%A1%E7%90%86%E7%9A%84%E8%B7%AF%E7%94%B1)
        - [(a) 服务启动及状态观察](#a-%E6%9C%8D%E5%8A%A1%E5%90%AF%E5%8A%A8%E5%8F%8A%E7%8A%B6%E6%80%81%E8%A7%82%E5%AF%9F-1)
        - [(b) 查看路由](#b-%E6%9F%A5%E7%9C%8B%E8%B7%AF%E7%94%B1-1)
      - [(4) 单独使用手动路由配置](#4-%E5%8D%95%E7%8B%AC%E4%BD%BF%E7%94%A8%E6%89%8B%E5%8A%A8%E8%B7%AF%E7%94%B1%E9%85%8D%E7%BD%AE)
    - [8.3.3 动态更新路由配置](#833-%E5%8A%A8%E6%80%81%E6%9B%B4%E6%96%B0%E8%B7%AF%E7%94%B1%E9%85%8D%E7%BD%AE)
  - [8.4 Predicates和Filter工厂](#84-predicates%E5%92%8Cfilter%E5%B7%A5%E5%8E%82)
    - [(1) 用途](#1-%E7%94%A8%E9%80%94-1)
    - [(2) 实现原理](#2-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
    - [8.4.1 内置Predicate工厂](#841-%E5%86%85%E7%BD%AEpredicate%E5%B7%A5%E5%8E%82)
    - [8.4.2 内置Filter工厂](#842-%E5%86%85%E7%BD%AEfilter%E5%B7%A5%E5%8E%82)
    - [8.4.3 自定义过滤器](#843-%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%87%E6%BB%A4%E5%99%A8)
      - [(1) Pre-filters和Post-filters](#1-pre-filters%E5%92%8Cpost-filters)
      - [(2) Demo内容：微服务调用跟踪](#2-demo%E5%86%85%E5%AE%B9%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%B0%83%E7%94%A8%E8%B7%9F%E8%B8%AA)
  - [8.5 使用Pre-filter](#85-%E4%BD%BF%E7%94%A8pre-filter)
    - [8.5.1 在服务间的Request中添加correlation id](#851-%E5%9C%A8%E6%9C%8D%E5%8A%A1%E9%97%B4%E7%9A%84request%E4%B8%AD%E6%B7%BB%E5%8A%A0correlation-id)
      - [(1) 方法](#1-%E6%96%B9%E6%B3%95)
      - [(2) UserContextFilter：拦截传入的HTTP请求](#2-usercontextfilter%E6%8B%A6%E6%88%AA%E4%BC%A0%E5%85%A5%E7%9A%84http%E8%AF%B7%E6%B1%82)
      - [(3) UserContextIntecptor和自定义RestTemplate：确保correlation向下游传播](#3-usercontextintecptor%E5%92%8C%E8%87%AA%E5%AE%9A%E4%B9%89resttemplate%E7%A1%AE%E4%BF%9Dcorrelation%E5%90%91%E4%B8%8B%E6%B8%B8%E4%BC%A0%E6%92%AD)
        - [(a) UserContextInteceptor](#a-usercontextinteceptor)
        - [(b) 自定义RestTemplate](#b-%E8%87%AA%E5%AE%9A%E4%B9%89resttemplate)
  - [8.6 使用Post-filter](#86-%E4%BD%BF%E7%94%A8post-filter)
  - [8.7 小节](#87-%E5%B0%8F%E8%8A%82)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# CH08 Spring Cloud Gateway

用途：统一实施安全性、日志记录、调用跟踪等行为（而不用在各个服务中重复实现）

> 相比使用公共类库来统一实施，使用Gateway有如下好处
>
> 1. 避免多处使用类库时、使用不当带来的不一致行为
> 2. 一些功能（例如安全）需要专门领域知识、适合专门的团队来实现
> 3. 部署新功能只需升级Gateway，不需要生成散落在各处的业务系统

本章内容：

> * 在Spring Cloud项目中使用网关，实现微服务路由映射
> * 构建过滤器以使用correlation id来跟踪微服务调用

## 8.1 Gateway

### (1) 服务调用方式：不使用网关 v.s 使用网关

如果没有Gateway

> 要么通过Web客户端直接调用服务（使用instance的url或IP地址）
>
> 要么通过服务发现引擎（例如Eureka）以编程的方式（客户端缓存instance列表）调用这些服务
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch08_without_gateway.jpg" width="650" /></div>
>

使用Gateway

> Gateway充当服务调用中介，客户端仅与Gateway管理的单个URL对话，而Gateway根据客户端请求的路径确定它调用的服务
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch08_with_gateway.jpg" width="650" /></div>
>

### (2) 中央策略执行点（PEP：Policy Enforcement Point）功能

> Gateway可以用作中央策略执行点，可以实现跟多跨多个功能单元的功能，例如：
>
> * 静态路由：开发人员不需要知道其他部门的服务端点，它们现在统一置于Gateway之后
>* 动态路由：Gateway根据服务请求的数据来决定调用哪个服务端点，实现智能路由（例如参与某个计划的客户请求会路由到一个特定的服务集群）
> * 认证和授权：所有服务调用都会经过Gateway，由Gateway来进行检验用户身份和权限会非常方便
>* 指标收集：例如服务调用和响应时间等许多基本指标，从Gateway可以统一收集

### (3) 防止Gateway单点故障

> * 使用Load Balancer来确保Gateway可扩展
> * Gateway以stateless的方式实现，不再内存中保存任何信息

## 8.2 Spring Cloud Gateway

> 本书使用Spring Cloud Gateway来作为微服务网关的实现，它构建在Spring 5/Project Reactor/Spring Boot 2之上。使用非阻塞方式来实现。
>
> Spring Cloud Gateway提供如下功能：
>
> * 将所有服务映射到单个（或多个URL）、可以设置精细化的路由映射
> * 可以设置过滤器，检查通过网关的请求和响应
> * 使用`Route Predicate工厂`来设置测试器，检查请求是否满足一个/一组特定条件 

### 8.2.1 创建Spring Cloud Gateway模块 

#### (1) 创建gateway-server

> 在已有的微服务项目中新增一个名为`gatewayserver`的module，同样使用IDE或者官网提供的Spring Inilitializer，项目类型为Maven，packaging方式为Jar，Java版本选择11，依赖的starter如下：
>
> * Eureka Discovery Client（SPRING CLOUD DISCOVERY）
> * Config Client（SPRING CLOUD CONFIG）
> * Spring Boot Actuator（OPS）
> * Gateway （SPRING CLOUD ROUTING）
>
> 完整的pom.xml：[../chapter9/gatewayserver/pom.xml](../chapter9/gatewayserver/pom.xml)

#### (2) 本地配置

> ~~~yml
> spring:
> 	application:
> 		# server id
> 		name: gateway-server
> 	cloud:
> 		config:
> 			# config server的地址，业务配置存储在config server
> 			uri: http://localhost:8071
> ~~~
>
> 完整代码：[../chapter9/gatewayserver/src/main/resources/bootstrap.yml](../chapter9/gatewayserver/src/main/resources/bootstrap.yml)

### 8.2.2 配置gateway-server与Eureka通信

#### (1) 在`config-server`配置gateway-server让它将自己注册到Eureka中

> 添加用于让Geteway向Eureka注册的配置，其他配置后续小节介绍。
>
> 配置内容与其他server访问Eureka的配置相同，可参考：[CH06 6.4向Eureka注册服务](https://github.com/fangkun119/manning-smia/blob/master/note/ch06_service_discovery.md#3-%E6%9C%8D%E5%8A%A1%E5%9C%A8config-server%E4%B8%8A%E7%9A%84%E9%85%8D%E7%BD%AE)
>
> ~~~yml
> server:
> 	port: 8072
> eureka:
> 	instance:
> 		# instance不使用主机名，而是使用IP来向Eureka注册
> 		preferIpAddress: true 
> 	client:
> 		# 告诉instance将自己注册在Erueka中
> 		registerWithEureka: true
> 		# 将从Erueka获取的各类service instance列表缓存在本地，而不是每次调用服务都访问Eureka
> 		fetchRegistry: true
> 		# Erueka服务器列表
> 		# 生产环境下通常不是配置服务列表，而是配置Erueka集群，具体参考 https://projects.spring.io/spring-cloud/spring-cloud.html#spring-cloud-eureka-server
> 		serviceUrl:
> 			defaultZone: http://eurekaserver:8070/eureka/
> ~~~
> 
>Spring Cloud Configuration Server支持多种存储后端：文件、Git、Valut、……，本书的例子使用文件来存储。完整配置文件：[../chapter9/configserver/src/main/resources/config/gateway-server.yml](../chapter9/configserver/src/main/resources/config/gateway-server.yml)

#### (2) Spring Boot引导类

> 使用`@EnbaleEurekaClient`来让`gateway-server`可以访问Eureka
>
> ~~~java
> @SpringBootApplication
> @EnableEurekaClient
> public class ApiGatewayServerApplication { 
> 	public static void main(String[] args) {
> 		SpringApplication.run(ApiGatewayServerApplication.class, args);
> 	}
> }
> ~~~
>
> 代码：[../chapter9/gatewayserver/src/main/java/com/optimagrowth/gateway/ApiGatewayServerApplication.java](../chapter9/gatewayserver/src/main/java/com/optimagrowth/gateway/ApiGatewayServerApplication.java)

#### (3) 改用`Spring Cloud Load Balancer`来实现负载均衡

> 因为Demo中的Eureka在构建时将Load Balancer由默认的`Ribbon`改为了`Spring Cloud Loda Balancer`（具体参考6.3 构建[Spring Erueka Service](./ch06_service_discovery.md#63-构建spring-eureka-service)）。

为了能够正常与Eureka通信，需要把Gateway的Load Balancer也改成`Spring Cloud Load Balancer`

> 修改方法参考：[6.4 向Eureka注册服务](./ch06_service_discovery.md#64-向eureka注册服务)
>
> 修改内容：
>
> * 参考[git diff](https://github.com/fangkun119/manning-smia/commit/608eb44cc5b345139e6d0441bc5c58bb94557f75) 
> * 除了将gateway-server的Load Balancer改成了Spring Cloud Load Balancer，还修正了organize-service的编译依赖

## 8.3 在Spring Cloud Gateway中配置路由

> Gateway的核心是反向代理：捕获客户端的请求，然后代表客户端调用远程资源。Gateway需要知道如何将客户端请求映射到远端的服务上，有两种方式：
>
> * 通过Service Discovery自动映射路由
>* 通过Service Discovery手动映射路由

### 8.3.1 通过服务发现自动映射路由

#### (1) 开启自动映射

> 在config-server中的配置存储中添加配置（以使用文件作为存储为例）
>
> ~~~yml
> spring:
> 	cloud:
> 		gateway:
> 			# gateway下面只保留下面三行自动路由配置
> 			discovery.locator:
> 				enabled: true				# 开启
> 				lowerCaseServiceId: true	# service_id使用小写字母
> ~~~
>
> 代码：[../chapter9/configserver/src/main/resources/config/gateway-server.yml](../chapter9/configserver/src/main/resources/config/gateway-server.yml)

#### (2) 路由映射规则

> 映射规则：
>
> ~~~txt
> http://${gateway_server_host}:${gateway_port}/${service_id}/${rest_end_point}
> ~~~
>
> 例如 ：
>
> ~~~txt
> http://localhost:8072/organization-service/v1/organization/958aa1bf-18dc-405c-b84a-b69f04d98d4f
> ~~~
>
> 其中的`service_id`就是每个service本地配置中的`spring.application.name`。Instance启动时会把该配置项的值当做`service_id`向`Eureca`注册。以[licensing-service](../chapter9/licensing-service/src/main/resources/bootstrap.yml)为例它的service_id配置如下：
>
> ~~~yml
> spring:
> 	application:
> 		name: licensing-service 
> 	profiles:
> 		active: dev
> 	cloud:
> 		config: 
> 			uri: http://configserver:8071
> ~~~
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch08_mapping_orgsvc.jpg" width="650" /></div

#### (3) 自动支持Instance增加和删除、自动支持新Service注册

> 上述配置中Gateway与Eureka组合使用，好处是：
>
> * 可以借助Eureka添加和删除Instance而不需要修改Gateway
> * 新的服务向Eureka注册时，Gateway会自动生成指向该服务的映射

#### (4) 查看Gateway管理的路由

##### (a) 服务启动及状态观察

> 在[git commit 949793](https://github.com/fangkun119/manning-smia/commit/949793174a73870744e57d115c171cd738f8adf9)中更新了了各个容器创建的依赖关系，构建镜像（在关闭VPN的情况下运行`mvn clean package dockerfile:build`)之后：
>
> 1. 使用`docker-compose up -d`编排和创建容器
>
> 2. 等待eurekaserver容器启动完成（使用` docker-compose logs -f eurekaserver`查看）
>
> 3. 在浏览器中访问`http://localhost:8070/`确认可以查看Eureka Dashboard
>
>     通常情况下要等待5分钟才开始广播注册信息，demo为了便于观察设置了`waitTimeInMsWhenSyncEmpty: 5`，等待5毫秒之后就会开始广播
>
> 4. 等待gatewayserver容器启动完成（使用`docker-compose logs -f gatewayserver`查看）
>
> 5. 在Eureka Dashboard页面观察`Instances currently registered with Eureka` 一栏等待`GATEWAY-SERVER`，`LICENSING-SERVICE`、`ORGANIZATION-SERVICE`的状态变为UP
>
> 此时所有服务都已经启动完成

##### (b) 查看路由

> 可以看到，`自动映射路由`将service-id（如`licensing-service`、`gateway-server`、`organization-service`）用作了REST端点的第一层路径
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter8/
> $ curl -X GET http://localhost:8072/actuator/gateway/routes
> [
> 	{
> 		"predicate": "Paths: [/licensing-service/**], match trailing slash: true",
> 		"metadata": {
> 			"management.port": "8080"
> 		},
> 		"route_id": "ReactiveCompositeDiscoveryClient_LICENSING-SERVICE",
> 		"filters": [
> 			"[[RewritePath /licensing-service/(?<remaining>.*) = '/${remaining}'], order = 1]"
> 		],
> 		"uri": "lb://LICENSING-SERVICE",
> 		"order": 0
> 	}, {
> 		"predicate": "Paths: [/gateway-server/**], match trailing slash: true",
> 		"metadata": {
> 			"management.port": "8072"
> 		},
> 		"route_id": "ReactiveCompositeDiscoveryClient_GATEWAY-SERVER",
> 		"filters": [
> 				"[[RewritePath /gateway-server/(?<remaining>.*) = '/${remaining}'], order = 1]"
> 		],
> 		"uri": "lb://GATEWAY-SERVER",
> 		"order": 0
> 	}, {
> 		"predicate": "Paths: [/organization-service/**], match trailing slash: true",
> 		"metadata": {
> 			"management.port": "8081"
> 		},
> 		"route_id": "ReactiveCompositeDiscoveryClient_ORGANIZATION-SERVICE",
> 		"filters": [
> 			"[[RewritePath /organization-service/(?<remaining>.*) = '/${remaining}'], order = 1]"
> 		],
> 		"uri": "lb://ORGANIZATION-SERVICE",
> 		"order": 0
> 	}
> ]
> ~~~
>

### 8.3.2 使用服务发现手动映射路由

#### (1) 用途

> 提供更精细化的配置，下面的demo通过手动映射路由（例如将orgainzation-service缩短为orianization）来提供更短的REST URI、并且删除HTTP Request Header中与cookie相关的字段

#### (2) 添加手动路由配置

> 配置内容如下，在之前`自动映射`生成的路由基础上，增添了两条手动配置的路由
>
> ~~~yml
> spring:
> 	cloud:
> 		gateway:
> 			discovery.locator:
> 				enabled: true
>           			lowerCaseServiceId: true
>         		routes:
> 				- id: organization-service
> 					uri: lb://organization-service
> 					predicates:
> 					- Path=/organization/**
> 					filters:
> 					- RewritePath=/organization/(?<path>.*), /$\{path}
> 					- RemoveRequestHeader= Cookie,Set-Cookie
> 				- id: licensing-service
> 					uri: lb://licensing-service
> 					predicates:
> 					- Path=/license/**
> 					filters:
> 					- RewritePath=/license/(?<path>.*), /$\{path}
> 					- RemoveRequestHeader= Cookie,Set-Cookie
> ~~~
>
> 代码：[../chapter9/configserver/src/main/resources/config/gateway-server.yml](../chapter9/configserver/src/main/resources/config/gateway-server.yml)
>
> 以organization-service为例，当遇到由`predicates`所表示的匹配`/organization/**`的请求路径时，会执行`filters`中所定义的filters（将请求路径中的"/organization/"去掉、删除Cookie相关的Http Header），然后通过load balancer访问`uri`中所定义的organization-service

#### (3) 查看Gateway管理的路由

##### (a) 服务启动及状态观察

> 与上一小节相同

##### (b) 查看路由

> 可以看到最末尾的两条路由（peridcate为`/organization/**`和`/licensing/**`的）就是手动配置添加的路由
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter8/
> $ curl -X GET http://localhost:8072/actuator/gateway/routes
> [
> 	{
> 		"predicate": "Paths: [/licensing-service/**], match trailing slash: true",
> 		"metadata": {
> 			"management.port": "8080"
> 		},
> 		"route_id": "ReactiveCompositeDiscoveryClient_LICENSING-SERVICE",
> 		"filters": [
> 			"[[RewritePath /licensing-service/(?<remaining>.*) = '/${remaining}'], order = 1]"
> 		],
> 		"uri": "lb://LICENSING-SERVICE",
> 		"order": 0
>  	}, {
> 		"predicate": "Paths: [/gateway-server/**], match trailing slash: true",
> 		"metadata": {
> 			"management.port": "8072"
> 		},
> 		"route_id": "ReactiveCompositeDiscoveryClient_GATEWAY-SERVER",
> 		"filters": [
> 			"[[RewritePath /gateway-server/(?<remaining>.*) = '/${remaining}'], order = 1]"
> 		],
> 		"uri": "lb://GATEWAY-SERVER",
> 		"order": 0
>  	}, {
> 		"predicate": "Paths: [/organization-service/**], match trailing slash: true",
> 		"metadata": {
> 			"management.port": "8081"
> 		},
> 		"route_id": "ReactiveCompositeDiscoveryClient_ORGANIZATION-SERVICE",
> 		"filters": [
> 			"[[RewritePath /organization-service/(?<remaining>.*) = '/${remaining}'], order = 1]"
> 		],
> 		"uri": "lb://ORGANIZATION-SERVICE",
> 		"order": 0
>  	}, {
> 		"predicate": "Paths: [/organization/**], match trailing slash: true",
> 		"route_id": "organization-service",
> 		"filters": [
> 			"[[RewritePath /organization/(?<path>.*) = '/${path}'], order = 1]",
> 			"[[RemoveRequestHeader name = 'Cookie'], order = 2]"
> 		],
> 		"uri": "lb://organization-service",
> 		"order": 0
>  	}, {
> 		"predicate": "Paths: [/license/**], match trailing slash: true",
> 		"route_id": "licensing-service",
> 		"filters": [
> 			"[[RewritePath /license/(?<path>.*) = '/${path}'], order = 1]",
> 			"[[RemoveRequestHeader name = 'Cookie'], order = 2]"
> 		],
> 		"uri": "lb://licensing-service",
> 		"order": 0
> 	}
> ]
> ~~~

#### (4) 单独使用手动路由配置

> 自动配置和手动配置可以一起使用，也可以单独使用。
>
> 如果将自动配置删除，只保留主动配置如下
>
> ~~~yml
> spring:
> 	cloud:
> 		gateway:
>      		routes:
> 				- id: organization-service
> 					uri: lb://organization-service
> 					predicates:
> 					- Path=/organization/**
> 					filters:
> 					- RewritePath=/organization/(?<path>.*), /$\{path}
> 					- RemoveRequestHeader= Cookie,Set-Cookie
> 				- id: licensing-service
> 					uri: lb://licensing-service
> 					predicates:
> 					- Path=/license/**
> 					filters:
> 					- RewritePath=/license/(?<path>.*), /$\{path}
> 					- RemoveRequestHeader= Cookie,Set-Cookie
> ~~~
>
> 重复之前的实验，可以看到5条路由中的3条自动路由已经被删除，只剩下两条手动的路由
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter8/
> $ curl -X GET http://localhost:8072/actuator/gateway/routes
> [
> 	{
> 		"predicate": "Paths: [/organization/**], match trailing slash: true",
> 		"route_id": "organization-service",
> 		"filters": [
> 			"[[RewritePath /organization/(?<path>.*) = '/${path}'], order = 1]",
> 			"[[RemoveRequestHeader name = 'Cookie'], order = 2]"
> 		],
> 		"uri": "lb://organization-service",
> 		"order": 0
> 	}, {
> 		"predicate": "Paths: [/license/**], match trailing slash: true",
> 		"route_id": "licensing-service",
> 		"filters": [
> 			"[[RewritePath /license/(?<path>.*) = '/${path}'], order = 1]",
> 			"[[RemoveRequestHeader name = 'Cookie'], order = 2]"
> 		],
> 		"uri": "lb://licensing-service",
> 		"order": 0
> 	}
> ]
> ~~~
>

### 8.3.3 动态更新路由配置

在不重启gateway-server以及内部服务的情况下，动态更新路由配置，方法如下：

(1) 让config-server使用github来存储配置（本地存储需要重新创建镜像）

> 参考：[./ch05_spring_clound_configuration.md#536-整合git用git来存储配置](ch05_spring_clound_configuration.md#536-整合git用git来存储配置)

(2) 更新存储在git上的路由配置

(3) 向gateway-server的/actuator/gateway/refresh端点发送POST请求，将触发gateway的配置刷新

> 将触发gateway的配置刷新，gateway-server返回 200代表路由重载成功

(4) 向gateway-server的/actuator/gateway/routes端点发送GET请求（与上一小节相同）

> 可以看到新的路由配置

## 8.4 Predicates和Filter工厂

### (1) 用途

> 编写自定义逻辑（例如安全、日志、服务调用追踪等），将它们统一用在所有的服务调用上
>
> 而无需修改每一个服务

### (2) 实现原理

> `Spring Cloud Gateway`的`判断器`（`predicates`）和`过滤器`（`filters`）工厂，使用Spring Aspect在gateway-server上拦截各种服务访问，匹配并修饰或更改这些服务调用的行为，而无需原始编码者知道。下图显示了具体的过程：
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch08_gateway_arch_for_pf.jpg" width="650" /></div>
>
> 对应图中的序号：
>
> 1. 客户端向gateway-server发送请求，进入网关处理程序
>
> 2. gateway-server检查请求路径，根据predicates与路由规则向匹配
>
>     ~~~yml
>     # 例如上一小节中的配置
>     "predicate": "Paths: [/organization/**], match trailing slash: true",
>     ~~~
>
> 3. 对于匹配的请求，根据路由规则所配置的filter进行处理
>
>     ~~~yml
>     # 例如上一小节中的配置
>     "filters": [
>     	"[[RewritePath /license/(?<path>.*) = '/${path}'], order = 1]",
>     	"[[RemoveRequestHeader name = 'Cookie'], order = 2]"
>     ],
>     ~~~
>
> 4. 当通过所有filter处理之后，将请求转发给对应的服务（例如orgnization-service）

### 8.4.1 内置Predicate工厂

> Spring Cloud Gateway提供的内置Predicates工厂见表8.1，可以以列表的方式配置多个predicate，例如
>
> ~~~yml
> predicates:
> 	- Path=/organization/**
> ~~~
>
> ##### Table 8.1 Built-in predicate factories in the Spring Cloud Gateway
>
> | Predicate        | Description                                                  | Example                               |
> | ---------------- | ------------------------------------------------------------ | ------------------------------------- |
> | Before Route     | 匹配某个时间点之前的请求                                     | Before=2020-03-11T…                   |
> | After Route      | 匹配某个时间点之后的请求                                     | After=2020-03-11T…                    |
> | Between Route    | 匹配发生在在时间区间`[1st_parameter, 2nd_paramter)`内请求    | Between=2020-03-11T...,2020-04-11T... |
> | Header Route     | 用参数2的正则表达式、与参数1指定的HTTP Header进行匹配        | Header=X-Request-Id, \d+              |
> | Host Route       | 用Host Header与配置中的Host Pattern（Ant-style pattern separated with "." ）进行匹配 | Host=**.example.com                   |
> | Method Route     | 用HTTP Method进行匹配                                        | Method=GET                            |
> | Path Route       | 使用Spring PathMatcher进行匹配                               | Path=/organization/{id}               |
> | Query Route      | 用参数2指定的正则表达式，与参数1指定的url parameter取值进行匹配 | Query=id, 1                           |
> | Cookie Route     | 用参数2指定的正则表达式，与参数1指定的cookie字段进行匹配     | Cookie=SessionID, abc.                |
> | RemoteAddr Route | 用参数中的IP列表，与发起请求的远端IP地址进行匹配             | RemoteAddr=192.168.3.5/24             |

### 8.4.2 内置Filter工厂

> Spring Cloud Gateway提供的内置Filter工厂见表8.2，可以以列表的方式配置多个filter并指定他们执行的顺序，例如
>
> ~~~yml
> "filters": [
> 	"[[RewritePath /license/(?<path>.*) = '/${path}'], order = 1]",
> 	"[[RemoveRequestHeader name = 'Cookie'], order = 2]"
> ],
> ~~~
>
> ##### Table 8.2 Built-in filters factories in the Spring Cloud Gateway
>
> | Predicate             | Description                                                  | Example                                          |
> | --------------------- | ------------------------------------------------------------ | ------------------------------------------------ |
> | AddRequestHeader      | 根据配置参数添加HTTP Request Header                          | AddRequestHeader=X-Organization-ID,F39s2         |
> | AddResponseHeader     | 添加HTTP Response Header                                     | AddResponseHeader=X- Organization-ID,F39s2       |
> | AddRequestParameter   | 根据配置参数添加HTTP Request Parameters                      | AddRequestParameter=Organizationid, F39s2        |
> | PrefixPath            | 为HTTP Request Path添加前缀                                  | PrefixPath=/api                                  |
> | RequestRateLimiter    | 配置一个Rate Limiter：参数1代表每秒请求数；参数2代表bursting capacity；参数3用来设置一个KeyResolver接口的bean | RequestRateLimiter=10, 20,#{@userKeyResolver}    |
> | RedirectTo            | 根据配置参数中的HTTP Status（300系列的重定向代码）和URL来进行重定向 | RedirectTo=302,http://localhost:8072             |
> | RemoveNonProxyHeaders | 删除例如`Keep-Alive`, `Proxy-Authenticate`, or `Proxy-Authorization`之类的HTTP Header | -                                                |
> | RemoveRequestHeader   | 根据配置参数指定的名称来删除HTTP Reqeust Header              | RemoveRequestHeader=X-Request-Foo                |
> | RemoveResponseHeader  | 根据配置参数指定的名称来删除HTTP Response Header             | RemoveResponseHeader=X-Organization-ID           |
> | RewritePath           | 根据参数1的正则表达式，和参数2的替换值重写HTTP请求路径       | RewritePath=/organization/(?<path>.*), /$\{path} |
> | SecureHeaders         | 将与Security有关的HTTP Header添加到Response中                | -                                                |
> | SetPath               | 根据路径模板参数修改Request Path                             | SetPath=/{organization}                          |
> | SetStatus             | 设置HTTP Response的状态码                                    | SetStatus=500                                    |
> | SetResponseHeader     | 根据参数1指定的name以及参数2指定的value，设置HTTP Response Header | SetResponseHeader=X-Response-ID,123              |

### 8.4.3 自定义过滤器

#### (1) Pre-filters和Post-filters

> 使用自定义过滤器可以定义一组业务逻辑，每个服务请求都会被该逻辑处理。Spring Cloud Gateway支持两种filters：
>
> * `Pre-filters`：实际请求发送给目标服务之前 ，使用Pre-filter进行处理。例如确保所有的HTTP请求头添加了特定的Header、确保所有请求都经过了用户身份验证等
> * `Post-filters`：调用目标服务之后 ，Post-filter被调用，并将响应返回给客户端。例如将rsponse写入日志、处理错误、检查以确保response中没有敏感信息等
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch08_filter_pipeline.jpg" width="650" /></div>
>
> 下图是整个处理过程：请求首先经过`Pre-filters`处理，然后才决定路由到哪个service，最后service返回的Response交给`Post-filters`处理

#### (2) Demo内容：微服务调用跟踪

> 根据上图的流程 ，将使用两种过滤器、与gateway路由搭配，微服务调用跟踪功能：
>
> 1. `Pre-filters`：确保每一个经过gateway的请求，都有一个`correlation id`，踏实执行客户端请求时跨所有微服务携带的唯一ID，用来将一些列微服务调用串在一起
> 2. `Services`：即到目前为止Demo中所创建的organization-service和licensing-service
> 3. `Post-filters`：响Response中添加与此次REST调用关联的`correlation id`，这样当收到Response的程序可以提取`correlation id`并将其添加到它所发起的其他REST请求中

## 8.5 使用Pre-filter

创建Pre-filter的步骤如下：

步骤1：编写TrackingFilter类

> ~~~java
> @Order(1)
> @Component
> public class TrackingFilter implements GlobalFilter {
> 	...
> 
> 	@Autowired
> 	FilterUtils filterUtils;
> 
> 	@Override
> 	public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
> 		// 从ServerWebExchange对象获取HTTP Header
> 		HttpHeaders requestHeaders = exchange.getRequest().getHeaders();
> 		// 检查HTTP Header中是否包含correlation id
> 		if (isCorrelationIdPresent(requestHeaders)) {
> 			logger.debug("tmx-correlation-id found in tracking filter: {}. ", filterUtils.getCorrelationId(requestHeaders));
> 		} else {
> 			// 如果HTTP Header中没有correlation id
> 			// 使用java.util.UUID.randomUUID().toString()生成全局唯一的correlation id
> 			String correlationID = generateCorrelationId();
> 			// 将correlation id设置到HTTP Header中
> 			exchange = filterUtils.setCorrelationId(exchange, correlationID);
> 			// 会输出如下类似的日志：
> 			// gatewayserver_1        | 2020-04-14 22:31:23.835 DEBUG 1 --- [or-http-epoll-3] c.o.gateway.filters.TrackingFilter       : tmx-correlation-id generated in tracking filter: 735d8a31-b4d1-4c13-816d-c31db20afb6a.
> 			// 
> 			// 开启debug日志的配置：
> 			// logging:
> 			//     level:
> 			//		   com.netflix: WARN
> 			//		   org.springframework.web: WARN
> 			//		   com.optimagrowth: DEBUG
> 			logger.debug("tmx-correlation-id generated in tracking filter: {}.", correlationID);
> 		}
> 		return chain.filter(exchange);
> 	}
> 
> 	private boolean isCorrelationIdPresent(HttpHeaders requestHeaders) {
>      return null != filterUtils.getCorrelationId(requestHeaders) ? true : false;
> 	}
> 
> 	...
> }
> ~~~
>
> 代码：[/gatewayserver/src/main/java/com/optimagrowth/gateway/filters/TrackingFilter.java](../chapter9/gatewayserver/src/main/java/com/optimagrowth/gateway/filters/TrackingFilter.java)
>
> 读写HTTP Header中`tmx-correlation-id`字段的`getCorrelationId`和`setCorrelationId`方法如下
>
> ~~~java
> @Component
> public class FilterUtils {
> 	public static final String CORRELATION_ID = "tmx-correlation-id";
> 	public static final String AUTH_TOKEN     = "Authorization";
> 
> 	public String getCorrelationId(HttpHeaders requestHeaders){
> 		if (requestHeaders.get(CORRELATION_ID) !=null) {
> 			List<String> header = requestHeaders.get(CORRELATION_ID);
> 			return header.stream().findFirst().get();
> 		} else {
> 			return null;
> 		}
> 	}
> 
> 	...
> 
> 	public ServerWebExchange setRequestHeader(ServerWebExchange exchange, String name, String value) {
> 		// mutate()方法返回一个builder以用来更改exchange object的属性
> 		return exchange.mutate() 
> 					// 修改exchange的request属性
> 					// 该修改后的属性，作为装饰器的装饰部分，覆盖build中原属性的值
> 					.request(
> 						exchange.getRequest().mutate()
> 							.header(name, value) 
> 							.build()
> 					// 让builder生成一个新的ServerWebExchange
> 					).build();	
> 	}
> 
> 	public ServerWebExchange setCorrelationId(ServerWebExchange exchange, String correlationId) {
> 		return this.setRequestHeader(exchange, CORRELATION_ID, correlationId);
> 	}
> }
> ~~~
>
> 代码：[/gatewayserver/src/main/java/com/optimagrowth/gateway/filters/FilterUtils.java](../chapter9/gatewayserver/src/main/java/com/optimagrowth/gateway/filters/FilterUtils.java)

### 8.5.1 在服务间的Request中添加correlation id

#### (1) 方法 

本节要添加的功能如下图：

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch08_utilclass_for_correlation_id.jpg" width="500" /></div>
>

如图中的序号：

> 1. 上一小节使用的 pre-filter保证每个请求都添加了correlation id
> 2. 为下游的每一个服务引入UserContextFilter类，从请求中提取correlation id
> 3. 让下游服务的业务逻辑能够通过UserContext获取correlation id
> 4. 使用拦截器UserContextInterceptor，每一个outbound REST调用都能添加correlation id，是其可以在服务之间传播

#### (2) UserContextFilter：拦截传入的HTTP请求

以Liscensing Service为例

> ~~~java
> // 实现javax.servlet.Filter接口并声明成一个@Component，是的容器可以注入并使用过滤器
> @Component
> public class UserContextFilter implements Filter {
> 	...
> 
> 	@Override
> 	public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
> 		HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
> 		// 从HTTP请求中获取correlation id
> 		UserContextHolder.getContext().setCorrelationId(httpServletRequest.getHeader(UserContext.CORRELATION_ID));
> 		logger.debug("UserContextFilter Correlation id: {}",UserContextHolder.getContext().getCorrelationId());
> 		// 从HTTP请求中获取其他数据
> 		UserContextHolder.getContext().setUserId(httpServletRequest.getHeader(UserContext.USER_ID));
> 		UserContextHolder.getContext().setAuthToken(httpServletRequest.getHeader(UserContext.AUTH_TOKEN));
> 		UserContextHolder.getContext().setOrganizationId(httpServletRequest.getHeader(UserContext.ORGANIZATION_ID));
>  		// 继续执行其他的Filter
>         filterChain.doFilter(httpServletRequest, servletResponse);
>     }
> }
> ~~~

代码

> UserContextFilter：[/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextFilter.java](../chapter9/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextFilter.java)
>
> UserContextHolder：[/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextHolder.java](../chapter9/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextHolder.java)
>
> UserContext：[/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContext.java](../chapter9/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContext.java)

关于这三个类的实现原理，在上一章[ch07_resilience4j_and_patterns.md#710-resilience4j与threadlocal的兼容性](./ch07_resilience4j_and_patterns.md#710-resilience4j与threadlocal的兼容性)有详细介绍

#### (3) UserContextIntecptor和自定义RestTemplate：确保correlation向下游传播

##### (a) UserContextInteceptor

> ~~~java
> public class UserContextInterceptor implements ClientHttpRequestInterceptor {
> 	private static final Logger logger = LoggerFactory.getLogger(UserContextInterceptor.class);
> 	
> 	@Override
> 	public ClientHttpResponse intercept(HttpRequest request, byte[] body, ClientHttpRequestExecution execution) throws IOException {
> 		HttpHeaders headers = request.getHeaders();
> 		// 在HTTP Header中添加从上游收到的correlation_id，
> 		// 这样当gateway-server收到请求时，会使用这个correlation_id的值，而不是生成新的UUID
> 		headers.add(UserContext.CORRELATION_ID, UserContextHolder.getContext().getCorrelationId());
> 		headers.add(UserContext.AUTH_TOKEN, UserContextHolder.getContext().getAuthToken());
> 		return execution.execute(request, body);
> 	}
> }
> ~~~
>
> 代码：[/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextInterceptor.java](../chapter9/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContextInterceptor.java)

##### (b) 自定义RestTemplate

> ```java
> @SuppressWarnings("unchecked")
> @LoadBalanced // 使用负载均衡，默认使用Ribbon，本例使用Spring Cloud Load Balancer
> @Bean
> public RestTemplate getRestTemplate() {
> 	RestTemplate template = new RestTemplate();
> 	List interceptors = template.getInterceptors();
> 	// 将RestTemplate添加拦截器
> 	if (interceptors==null){
> 		template.setInterceptors(Collections.singletonList(new UserContextInterceptor()));
> 	} else{
> 		interceptors.add(new UserContextInterceptor());
> 		template.setInterceptors(interceptors);
> 	}
> 	return template;
> }
> ```
>
> 代码：[/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java](../chapter9/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java)

## 8.6 使用Post-filter

> `gateway-server`代表各个服务接收HTTP请求，因此有机会在服务返回的Response中添加附加信息，进而实现收集metrics、将同一个transaction中的调用关联在一起等功能。代码如下：
>
> ~~~java
> @Configuration
> public class ResponseFilter { 
> 	final Logger logger =LoggerFactory.getLogger(ResponseFilter.class);
> 	@Autowired
> 	FilterUtils filterUtils;
> 
> 	@Bean
> 	public GlobalFilter postGlobalFilter() {
> 		return (exchange, chain) -> {
> 			return chain.filter(exchange).then(
>                 Mono.fromRunnable(() -> {
> 					// 原始HTTP Request的headers
> 					HttpHeaders requestHeaders = exchange.getRequest().getHeaders();
> 					// 从headers中提取correlation id
> 					String correlationId = filterUtils.getCorrelationId(requestHeaders);
> 					// 打印日志，并将correlation id添加到response的HTTP Header中
> 					logger.debug("Adding the correlation id to the outbound headers. {}", correlationId);
> 					exchange.getResponse().getHeaders().add(FilterUtils.CORRELATION_ID,  correlationId);
> 					logger.debug("Completing outgoing request for {}.", exchange.getRequest().getURI());
> 				}
> 			));
> 		};
> 	}
> }
> ~~~
>
> 代码修改之后，可以看到Response HTTP Header中增加了tmx-correlation-id字段
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch08_test_response_filter_payload.jpg" width="650" /></div>
>
> 在日志中，同一个transaction下，不同service的日志可以通过correlation id来关联
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch08_test_response_filter_log.jpg" width="650" /></div>
>

## 8.7 小节

> 略