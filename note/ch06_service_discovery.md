<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!-- **Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)* -->

- [CH06 Service Discovery](#ch06-service-discovery)
  - [6.1 传统集中式服务发现](#61-%E4%BC%A0%E7%BB%9F%E9%9B%86%E4%B8%AD%E5%BC%8F%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0)
    - [(1) 模型](#1-%E6%A8%A1%E5%9E%8B)
    - [(2) 适用场景](#2-%E9%80%82%E7%94%A8%E5%9C%BA%E6%99%AF)
    - [(3) 不适合云环境微服务的原因](#3-%E4%B8%8D%E9%80%82%E5%90%88%E4%BA%91%E7%8E%AF%E5%A2%83%E5%BE%AE%E6%9C%8D%E5%8A%A1%E7%9A%84%E5%8E%9F%E5%9B%A0)
  - [6.2 云环境中的服务发现](#62-%E4%BA%91%E7%8E%AF%E5%A2%83%E4%B8%AD%E7%9A%84%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0)
    - [6.2.1 服务发现](#621-%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0)
      - [(1) 添加/删除服务实例](#1-%E6%B7%BB%E5%8A%A0%E5%88%A0%E9%99%A4%E6%9C%8D%E5%8A%A1%E5%AE%9E%E4%BE%8B)
      - [(2) 客户端负载均衡](#2-%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1)
    - [6.2.2 本章Demo及服务间调用关系](#622-%E6%9C%AC%E7%AB%A0demo%E5%8F%8A%E6%9C%8D%E5%8A%A1%E9%97%B4%E8%B0%83%E7%94%A8%E5%85%B3%E7%B3%BB)
  - [6.3 构建Spring Eureka Service](#63-%E6%9E%84%E5%BB%BAspring-eureka-service)
      - [(1) 项目创建及依赖](#1-%E9%A1%B9%E7%9B%AE%E5%88%9B%E5%BB%BA%E5%8F%8A%E4%BE%9D%E8%B5%96)
      - [(2) Eureka本地配置](#2-eureka%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE)
      - [(3) 在config-server中的配置](#3-%E5%9C%A8config-server%E4%B8%AD%E7%9A%84%E9%85%8D%E7%BD%AE)
      - [(4) 程序引导类](#4-%E7%A8%8B%E5%BA%8F%E5%BC%95%E5%AF%BC%E7%B1%BB)
      - [(5) 程序启动](#5-%E7%A8%8B%E5%BA%8F%E5%90%AF%E5%8A%A8)
  - [6.4 向Eureka注册服务](#64-%E5%90%91eureka%E6%B3%A8%E5%86%8C%E6%9C%8D%E5%8A%A1)
      - [(1) 添加依赖](#1-%E6%B7%BB%E5%8A%A0%E4%BE%9D%E8%B5%96)
      - [(2) 服务的本地配置](#2-%E6%9C%8D%E5%8A%A1%E7%9A%84%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE)
      - [(3) 服务在config-server上的配置](#3-%E6%9C%8D%E5%8A%A1%E5%9C%A8config-server%E4%B8%8A%E7%9A%84%E9%85%8D%E7%BD%AE)
    - [6.4.1 Eureka's REST API](#641-eurekas-rest-api)
      - [(1) API](#1-api)
      - [(2) 测试](#2-%E6%B5%8B%E8%AF%95)
    - [6.4.2 Eureka dashboard](#642-eureka-dashboard)
  - [6.5 使用Service Discovery发现服务](#65-%E4%BD%BF%E7%94%A8service-discovery%E5%8F%91%E7%8E%B0%E6%9C%8D%E5%8A%A1)
    - [(1) 三个客户端](#1-%E4%B8%89%E4%B8%AA%E5%AE%A2%E6%88%B7%E7%AB%AF)
    - [(2) Controller](#2-controller)
    - [(3) Service](#3-service)
    - [6.5.1 方法1：使用Spring Discovery Client发现服务实例（不推荐）](#651-%E6%96%B9%E6%B3%951%E4%BD%BF%E7%94%A8spring-discovery-client%E5%8F%91%E7%8E%B0%E6%9C%8D%E5%8A%A1%E5%AE%9E%E4%BE%8B%E4%B8%8D%E6%8E%A8%E8%8D%90)
      - [(1) Sprint Boot引导类](#1-sprint-boot%E5%BC%95%E5%AF%BC%E7%B1%BB)
      - [(2) Discovery Client封装](#2-discovery-client%E5%B0%81%E8%A3%85)
    - [6.5.2 方法2： 使用LoadBalancer Awared RestTemplate调用服务](#652-%E6%96%B9%E6%B3%952-%E4%BD%BF%E7%94%A8loadbalancer-awared-resttemplate%E8%B0%83%E7%94%A8%E6%9C%8D%E5%8A%A1)
      - [(1) 定义LoadBalancer Awared RestTemplate](#1-%E5%AE%9A%E4%B9%89loadbalancer-awared-resttemplate)
      - [(2) 使用LoadBalancer Awared RestTempate](#2-%E4%BD%BF%E7%94%A8loadbalancer-awared-resttempate)
    - [6.5.3 使用Netflix Feign Client调用服务](#653-%E4%BD%BF%E7%94%A8netflix-feign-client%E8%B0%83%E7%94%A8%E6%9C%8D%E5%8A%A1)
      - [(1) Spring Boot引导类](#1-spring-boot%E5%BC%95%E5%AF%BC%E7%B1%BB)
      - [(2) 定义Feign Client Interface](#2-%E5%AE%9A%E4%B9%89feign-client-interface)
    - [补充：错误处理](#%E8%A1%A5%E5%85%85%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86)
  - [6.6 小结](#66-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

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

> 本章的Demo使用`Spring Netflix Eureka`作为服务发现引擎，使用`Spring Cloud Load Balancer`用于客户端负载均衡（虽然Ribbon是目前主要使用的load balancer，但是已经不再开发进入了维护模式，因此本章演示如何使用Spring Cloud Load Balancer）
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch06_eureka_with_caching.jpg" width="800" /></div>
>
> 本章Demo创建了两个Service -`licensing service`和`organization service`：当licensing service调用organization service时，instance的信息来自Eureka，并且通过client side load banlancer进行选择
>
> Demo代码位置：[../chapter6/Final/](../chapter6/Final/)

## 6.3 构建Spring Eureka Service

> 代码位置：[../chapter6/Final/eurekaserver/](..chapter6/Final/eurekaserver/)

#### (1) 项目创建及依赖

> 使用Spring Initailizer创建项目
>
> * Java 11，JAR packaging，Spring版本选择2.2.x及以上
> * 依赖的starter选择Eureka Server、Config Client、Spring Boot Actuator
>
> 依赖：使用Spring Cloud Load Balancer替代Ribbon（原因如上一小节所属）
>
> ```xml
> <dependencies>
> 	<dependency>
> 		<groupId>org.springframework.boot</groupId>
> 		<artifactId>spring-boot-starter-actuator</artifactId>
> 	</dependency>
> 	<dependency>
> 		<groupId>org.springframework.cloud</groupId>
> 		<artifactId>spring-cloud-starter-config</artifactId>
> 	</dependency>
> 	<dependency>
> 		<groupId>org.springframework.cloud</groupId>
> 		<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
> 		<exclusions>
> 			<exclusion>
> 				<groupId>org.springframework.cloud</groupId>
> 				<artifactId>spring-cloud-starter-ribbon</artifactId>
> 			</exclusion>
> 			<exclusion>
> 				<groupId>com.netflix.ribbon</groupId>
> 				<artifactId>ribbon-eureka</artifactId>
> 			</exclusion>
> 		</exclusions>
> 	</dependency>
> 	<dependency>
> 		<groupId>org.springframework.cloud</groupId>
> 		<artifactId>spring-cloud-starter-loadbalancer</artifactId>
> 	</dependency>
> 	...
> </dependencies>
> ```
>
> 完整的pom.xml：[/eurekaserver/pom.xml](../chapter6/Final/eurekaserver/pom.xml)

#### (2) Eureka本地配置

> 配置由Spring Cloud Config Server托管，因此配置了config-server的地址
>
> ```yml
> spring:
> 	application:
> 		name: eureka-server
> 	cloud:
> 		config: 
> 			uri: http://configserver:8071 # docker-compose.yml中的service name
> 		loadbalancer:
> 			ribbon:
> 				enabled: false # 禁用ribbon，独立模式运行Eureka以便整个Spring Cloud Load Balancer
> ```
>
> [/eurekaserver/src/main/resources/bootstrap.yml](../chapter6/Final/eurekaserver/src/main/resources/bootstrap.yml)

#### (3) 在config-server中的配置

> config-server可以使用 (1) 本地文件 (2) git (3) 外部存储（例如vault）来存储配置文件。本书demo使用本地文件来来存储
>
> 配置文件：[/configserver/src/main/resources/config/eureka-server.yml](../chapter6/Final/configserver/src/main/resources/config/eureka-server.yml)
>
> ~~~yml
> spring:
> 	application:
> 		name: eureka-server
> 	boot:
> 		admin:
> 			context-path: /admin
> server:
> 	port: 8070 						 # Eureka监听端口
> eureka:
> 	instance:
> 		hostname: eurekaserver		 # Eureka主机名（与docker-compose.yml的服务名、网络别名保持一致）
> 	client:
> 		registerWithEureka: false	 # 不要把自己（Eureka）注册到Eureka服务中
> 		fetchRegistry: false		 # 不要在本地缓存instance列表
> 		serviceUrl: 				 # 给各类客户端提供的Service URL
> 			defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
> 	server:
> 		# 1. 默认情况下Eureka启动后会等待5分钟再广播服务注册信息，以便给各类服务足够的时间加入
> 		# 2. 单个服务最多需要30秒（3个heartbeat ping）才能显示在Eureka服务中
> 		# 为了能够便于demo演示（加快Eureka启动时间，显示服务加入进度），将等待时间改为5毫秒
> 		waitTimeInMsWhenSyncEmpty: 5 # 服务器接收请求之前初始等待的毫秒数
> 
> management:
> 	endpoints:
> 		web:
> 			exposure:
> 				include: "*"
> ~~~

#### (4) 程序引导类

> 使用`@EnableEurekaServer`注解
>
> ```java
> @SpringBootApplication
> @EnableEurekaServer
> public class EurekaServerApplication {
> 	public static void main(String[] args) {
> 		SpringApplication.run(EurekaServerApplication.class, args);
> 	}
> }
> ```
>
> 代码：[/eurekaserver/src/main/java/com/optimagrowth/eureka/EurekaServerApplication.java](../chapter6/Final/eurekaserver/src/main/java/com/optimagrowth/eureka/EurekaServerApplication.java)

#### (5) 程序启动

> Eureka依赖Spring Cloud Config Server，因此要先启动configserver，在[`docker-compose.yml`](../chapter6/Final/docker/docker-compose.yml)中可以看到依赖配置
>
> ~~~yml
> eurekaserver:
> 	image: ostock/eurekaserver:0.0.1-SNAPSHOT
> 	ports:
> 		- "8070:8070"
> 	depends_on:
> 		database:
> 			condition: service_healthy
> 		configserver:
> 			condition: service_started  
> 	networks:
> 		backend:
> 			aliases:
> 				- "eurekaserver"
> ~~~

## 6.4 向Eureka注册服务

> 将服务注册到`Eureka`是一件Straight Forward的事情，本章有两个服务`licensing-service`和`organization-service`需要注册到Eureka中
>

#### (1) 添加依赖

> 依赖项是`spring-cloud-starter-netflix-eureka-client`
>
> ~~~xml
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
> </dependency>
> ~~~
>
> 因为要使用Spring Cloud Load Balancer替代Ribbon，因此采用如下配置
>
> ~~~xml
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
> 	<exclusions>
> 		<exclusion>
> 			<groupId>org.springframework.cloud</groupId>
> 			<artifactId>spring-cloud-starter-ribbon</artifactId>
> 		</exclusion>
> 		<exclusion>
> 			<groupId>com.netflix.ribbon</groupId>
> 			<artifactId>ribbon-eureka</artifactId>
> 		</exclusion>
> 	</exclusions>
> </dependency>
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-starter-loadbalancer</artifactId>
> </dependency>
> ~~~
>
> 代码：
>
> * [../chapter6/Final/licensing-service/pom.xml](../chapter6/Final/licensing-service/pom.xml)
>
> * [../chapter6/Final/organization-service/pom.xml](../chapter6/Final/organization-service/pom.xml)

#### (2) 服务的本地配置

> 服务名分别设置`spring.application.name`为"organization-service"和"licensing-service"
>
> Eureka中每个instance都有两个ID：Service ID和Instance ID。其中Service ID来自`spring.application.name`的值 
>
> [`../chapter6/Final/organization-service/src/main/resources/bootstrap.yml`](../chapter6/Final/organization-service/src/main/resources/bootstrap.yml)
>
> ~~~yml
> spring:
> 	application:
> 		name: organization-service
> 	profiles:
> 		active: dev
> 	cloud:
> 		config:
> 			uri: http://localhost:8071
> ~~~
>
> [`../chapter6/Final/licensing-service/src/main/resources/bootstrap.yml`](../chapter6/Final/licensing-service/src/main/resources/bootstrap.yml)
>
> ~~~yml
> spring:
> 	application:
> 		name: licensing-service
> 	profiles:
> 		active: dev
> 	cloud:
> 		config:
> 			uri: http://localhost:8071
> ~~~

#### (3) 服务在config-server上的配置

> Demo中的config-server使用了本地配置存储的方式（为了便于演示）、与Eureka相关的配置如下
>
> [../chapter6/Final/configserver/src/main/resources/config/licensing-service.properties](chapter6/Final/configserver/src/main/resources/config/licensing-service.properties)
>
> ~~~properties
> eureka.instance.preferIpAddress = true
> eureka.client.registerWithEureka = true
> eureka.client.fetchRegistry = true
> eureka.client.serviceUrl.defaultZone = http://eurekaserver:8070/eureka/
> ~~~
>
> [../chapter6/Final/configserver/src/main/resources/config/organization-service.properties](../chapter6/Final/configserver/src/main/resources/config/organization-service.properties)
>
> ~~~properties
> eureka.instance.preferIpAddress = true
> eureka.client.registerWithEureka = true
> eureka.client.fetchRegistry = true
> eureka.client.serviceUrl.defaultZone = http://eurekaserver:8070/eureka/
> ~~~
>
> `eureka.instance.preferIpAddress = true（默认为false）`：
>
> * 默认为false，客户端按照主机名来访问instance
> * 在容器部署中，会使用随机生成的主机名启动，且不会提供DNS条目，因此改为true
>
> `eureka.client.registerWithEureka = true（默认为true）`
>
> * 告诉service将自己注册在eureka中
>
> `eureka.client.fetchRegistry = true（默认为true）`
>
> * 将获取的各类service instance列表缓存在本地，而不是每次调用都访问eureka
>
> `eureka.client.serviceUrl.defaultZone`
>
> * 用逗号分隔的Eureka服务列表，用来解析定位该服务的位置
> * 只是配置多个Eureka节点还不足以实现高可用性，高可用性需要搭建Eureka集群并让它们相互通信，具体参考[https://projects.spring.io/spring-cloud/spring-cloud.html#spring-cloud-eureka-server](https://projects.spring.io/spring-cloud/spring-cloud.html#spring-cloud-eureka-server)

### 6.4.1 Eureka's REST API

####  (1) API

> 查看所有实例
>
> ~~~txt
> http://<eureka service>:8070/eureka/apps/<APPID>
> ~~~

#### (2) 测试

> 测试用例：[../chapter6/chapter6.postman_collection.json](../chapter6/chapter6.postman_collection.json)（安装Postman后导入）
>
> 编译：mvn clean package
>
> 构建镜像：mvn dockerfile:build
>
> 创建容器：
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter6/Final/docker/
> $ docker-compose up -d
> Starting docker_configserver_1 ... done
> Starting docker_database_1     ... done
> Creating docker_eurekaserver_1    	... done
> Creating docker_licensingservice_1    ... done
> Creating docker_organizationservice_1 ... done
> ~~~
>
> 访问Eureka获得organization-service的instance列表
>
> 请求：使用curl或者post访问，默认返回XML，需要在HTTP  Header中设置"Accept"字段来获取JSON
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/note/
> $ curl -H "Accept:application/json" -X GET http://localhost:8070/eureka/apps/organization-service
> ...
> ~~~
>
> 返回值：其中`app`对应Service ID，`ipAddr`/`port`是实例的地址，`"status":"UP"`表示服务状态
>
> ~~~json
> {
> 	"application": {
> 		"name": "ORGANIZATION-SERVICE",
> 		"instance": [
> 			{
> 				"instanceId": "2b0f03b1692c:organization-service:8081",
> 				"hostName": "172.18.0.6",
> 				"app": "ORGANIZATION-SERVICE",
> 				"ipAddr": "172.18.0.6",
> 				"status": "UP",
> 				"overriddenStatus": "UNKNOWN",
> 				"port": {
> 					"$": 8081,
> 					"@enabled": "true"
> 				},
> 				"securePort": {
> 					"$": 443,
> 					"@enabled": "false"
> 				},
> 				"countryId": 1,
> 				"dataCenterInfo": {
> 					"@class": "com.netflix.appinfo.InstanceInfo$DefaultDataCenterInfo",
> 					"name": "MyOwn"
> 				},
> 				"leaseInfo": {
> 					"renewalIntervalInSecs": 30,
> 					"durationInSecs": 90,
> 					"registrationTimestamp": 1612766859837,
> 					"lastRenewalTimestamp": 1612767069580,
> 					"evictionTimestamp": 0,
> 					"serviceUpTimestamp": 1612766859891
> 				},
> 				"metadata": {
> 					"management.port": "8081"
> 				},
> 				"homePageUrl": "http://172.18.0.6:8081/",
> 				"statusPageUrl": "http://172.18.0.6:8081/actuator/info",
> 				"healthCheckUrl": "http://172.18.0.6:8081/actuator/health",
> 				"vipAddress": "organization-service",
> 				"secureVipAddress": "organization-service",
> 				"isCoordinatingDiscoveryServer": "false",
> 				"lastUpdatedTimestamp": "1612766859892",
> 				"lastDirtyTimestamp": "1612766859501",
> 				"actionType": "ADDED"
> 			}
> 		]
> 	}
> }
> ~~~

### 6.4.2 Eureka dashboard

> 可以在浏览器访问Eureka Dashboard，其中的Application一栏可以看到服务的状态（通常要等服务在30秒内ping通三次才会认为该服务已经在线）
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch06_erueka_dashboard2.jpg" width="1024" /></div>

## 6.5 使用Service Discovery发现服务

### (1) 三个客户端

> 将演示3个不同的客户端，与Spring Clould Load Balancer交互，这三个客户端分别是
>
> 1. Spring Discovery Client
> 2. 启用Spring Discovery Client的RestTemplate
> 3. Netflix Feign Client

### (2) Controller

>在Controller中增加了一个API，该API新增一个clientType参数 ，用来演示不同的客户端
>
>~~~java
>@RequestMapping(value="/{licenseId}/{clientType}",method = RequestMethod.GET)
>public License getLicensesWithClient( 
>	@PathVariable("organizationId") String organizationId,
>	@PathVariable("licenseId") String licenseId,
>	// 三种取值：
>    // Discovery : 使用Spring Discovery Client加标准的RestTemplate，无负载均衡
>    // Rest      : 使用加强版的RestTemplate通过Load Balancer调用服务
>    // Feign     : 使用Netflix Feign通过Load Balancer调用服务
>	@PathVariable("clientType") String clientType) {
>		return licenseService.getLicense(licenseId, organizationId, clientType);
>}
>~~~
>
>代码：[../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/controller/LicenseController.java](../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/controller/LicenseController.java)
>
>测试用的URL：
>
>~~~txt
>http://<licensing service Hostname/IP>:<licensing service Port>/v1/ organization/<organizationID>/license/<licenseID>/<client type( feign, discovery, rest)>
>~~~

### (3) Service

> 代码：[../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)
>
> 这个Service同时支持三种Client
>
> ~~~java
> public License getLicense(String licenseId, String organizationId, String clientType){
> 	// 从数据库获得License的信息
> 	License license = licenseRepository.findByOrganizationIdAndLicenseId(organizationId, licenseId);
> 	if (null == license) {
> 		throw new IllegalArgumentException(String.format(messages.getMessage("license.search.error.message", null, null),licenseId, organizationId));
>     }
> 	// 调用一种客户端，访问organizeation-service获得Organization信息
> 	Organization organization = retrieveOrganizationInfo(organizationId, clientType);
> 	if (null != organization) {
> 		license.setOrganizationName(organization.getName());
> 		license.setContactName(organization.getContactName());
> 		license.setContactEmail(organization.getContactEmail());
> 		license.setContactPhone(organization.getContactPhone());
> 	}
> 	// 通过configuration properties配置的信息
> 	return license.withComment(config.getProperty());
> }
> ~~~
>
> 这个方法调用的`retrieveOrganizationInfo`，会根据clientType的取值来选择一种Client来调用

### 6.5.1 方法1：使用Spring Discovery Client发现服务实例（不推荐）

> 先使用Spring Discovery Client从Load Balancer中取出一个instance的URL，然后使用标准的RestTemplate访问这个端点
>

#### (1) Sprint Boot引导类

> 为引导类添加`@EnableDiscoveryClient`注解
>
> ~~~java
> @SpringBootApplication
> @RefreshScope
> //@EnableDiscoveryClient会触发Spring Cloud启用Spring Discovery Client以及Spring Cloud Load Balancer
> @EnableDiscoveryClient
> public class LicenseServiceApplication {
> 	public static void main(String[] args) {
> 		SpringApplication.run(LicenseServiceApplication.class, args);
> 	}
> }
> ~~~
>
> 代码：[../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java](../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java)

#### (2) Discovery Client封装

> ~~~java
> @Component
> public class OrganizationDiscoveryClient {
> 	@Autowired
> 	private DiscoveryClient discoveryClient;
> 
> 	public Organization getOrganization(String organizationId) {
> 		RestTemplate restTemplate = new RestTemplate();
> 
> 		// 根据Service ID获取Instance列表
> 		List<ServiceInstance> instances = discoveryClient.getInstances("organization-service");
> 		if (instances.size()==0) {
> 			return null;
> 		}
> 		// 选一个instance的url，用标准的RestTemplate进行访问，没有负载均衡
> 		String serviceUri = String.format("%s/v1/organization/%s",instances.get(0).getUri().toString(), organizationId);
> 		ResponseEntity< Organization > restExchange = restTemplate.exchange(
> 				serviceUri, HttpMethod.GET, null, Organization.class, organizationId);
>         return restExchange.getBody();
>     }
> }
> ~~~
>
> 代码：[../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationDiscoveryClient.java](../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationDiscoveryClient.java)
>
> 备注：
>
> * 这份代码仅仅是演示如何通过Discovery Client获取instance列表，有诸多不妥之处，并不建议使用这个方法
>
> * 另外关于RestTemplate，参考：[https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch08_rest_client.md](https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch08_rest_client.md)

### 6.5.2 方法2： 使用LoadBalancer Awared RestTemplate调用服务

> 使用具有LoadBalancer Awared RestTemplate调用服务，也是与Load Balancer进行交互比较常见的机制
>

#### (1) 定义LoadBalancer Awared RestTemplate

> ~~~java
> @SpringBootApplication
> @RefreshScope
> public class LicenseServiceApplication {
> 	public static void main(String[] args) {
> 		SpringApplication.run(LicenseServiceApplication.class, args);
> 	}
> 
>    	// 定义RestTemplate Bean
>    	// 使用@LoadBalanced注解、使其变为LoadBalancer Awared
> 	@LoadBalanced
> 	@Bean
> 	public RestTemplate getRestTemplate(){
> 		return new RestTemplate();
> 	}
> }
> ~~~
>
> 代码：[../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java](../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java)

####  (2) 使用LoadBalancer Awared RestTempate

> ```java
> @Component
> public class OrganizationRestTemplateClient {
> 	// 注入的RestTemplate是使用@LoadBalanced注解、支持Spring Cloud Load Balancer的bean
> 	@Autowired
> 	RestTemplate restTemplate;
> 
> 	public Organization getOrganization(String organizationId){
> 		ResponseEntity<Organization> restExchange = restTemplate.exchange(
> 			// ”organization-service“是application id
> 			"http://organization-service/v1/organization/{organizationId}",
> 			HttpMethod.GET, null, Organization.class, organizationId);
> 		return restExchange.getBody();
> 	}
> }
> ```
>
> 在url中拼接的是application id，这个id配置在[bootstrap.yml](../chapter6/Final/organization-service/src/main/resources/bootstrap.yml)的`spring.application.name`，instance启动时，会把它当做service id向Eureka注册
>
> ~~~yml
> spring:
> 	application:
> 		name: licensing-service
> 	...
> ~~~
>
> 代码：[../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationRestTemplateClient.java](../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationRestTemplateClient.java)

### 6.5.3 使用Netflix Feign Client调用服务

#### (1) Spring Boot引导类

> 在引导类添加`@EnableFeignClients`以启用该功能
>
> ~~~java
> @SpringBootApplication
> @EnableFeignClients
> public class LicenseServiceApplication {
> 	public static void main(String[] args) {
> 		SpringApplication.run(LicenseServiceApplication.class, args);
> 	}
> }
> ~~~
>
> 代码：[../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java](../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java)

#### (2) 定义Feign Client Interface

> ~~~java
> // "organization-service"是`spring.application.name`的值
> // `spring.application.name`的值在instance启动时当做service id向Eureka注册
> @FeignClient("organization-service") 
> public interface OrganizationFeignClient {
> 	// 与OrganizationController中对应方法的@RequestMapping到的端点相同
> 	@RequestMapping(
> 		method= RequestMethod.GET,
> 		value="/v1/organization/{organizationId}",
> 		consumes="application/json")
> 	Organization getOrganization(@PathVariable("organizationId") String organizationId);
> }
> ~~~
>
> * 接口`OrganizationFeignClient`被使用`@FeignClient("organization-service") `注解后，会把Spring Cloud Load Balancer调用的方法映射到Eureka-based Service
>
> * 这个位于`licensing-service`的`getOrganization`方法上的`@RequestMapping`注解所定义的REST 端点，与位于`organizations-service`的controller方法所定义的端点相同（如下面的“参考”）
>
> 参考：[../chapter6/Final/organization-service/src/main/java/com/optimagrowth/organization/controller/OrganizationController.java](../chapter6/Final/organization-service/src/main/java/com/optimagrowth/organization/controller/OrganizationController.java)
>
> ```java
> @RestController
> @RequestMapping(value="v1/organization")
> public class OrganizationController {
> 	...
> 
> 	@RequestMapping(value="/{organizationId}",method = RequestMethod.GET)
> 	public ResponseEntity<Organization> getOrganization( @PathVariable("organizationId") String organizationId) {
> 		return ResponseEntity.ok(service.findById(organizationId));
> 	}
> 
> 	...
> }
> ```
>
> 代码：[../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationFeignClient.java](../chapter6/Final/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationFeignClient.java)

### 补充：错误处理

使用Spring RestTemplate类时

> 所有服务调用的HTTP Status Code，都将通过RestponseEntity的getStatusCode()方法返回
>
> ```java
> ResponseEntity<Organization> restExchange = restTemplate.exchange(
> 		"http://organization-service/v1/organization/{organizationId}",
> 		HttpMethod.GET, null, Organization.class, organizationId);
> HttpStatus status = restExchange.getStatusCode();
> ```

使用Feign Client时

> 如果被调用的服务返回HTTP 5XX - 5XX状态，都会抛出FeighException，该异常包含一个JSON body，可以被解析成具体的error  message。Feign提供了decoder用来解析该异常，具体参考：[https://github.com/Netflix/feign/wiki/Custom-error-handling](https://github.com/Netflix/feign/wiki/Custom-error-handling)

## 6.6 小结

> 略 