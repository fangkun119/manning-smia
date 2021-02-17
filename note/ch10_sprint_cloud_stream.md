<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [CH10 Spring Cloud Srream事件驱动架构](#ch10-spring-cloud-srream%E4%BA%8B%E4%BB%B6%E9%A9%B1%E5%8A%A8%E6%9E%B6%E6%9E%84)
  - [10.1 EDA（事件驱动框架）与微服务](#101-eda%E4%BA%8B%E4%BB%B6%E9%A9%B1%E5%8A%A8%E6%A1%86%E6%9E%B6%E4%B8%8E%E5%BE%AE%E6%9C%8D%E5%8A%A1)
    - [10.1.1 方案1：使用同步请求-响应](#1011-%E6%96%B9%E6%A1%881%E4%BD%BF%E7%94%A8%E5%90%8C%E6%AD%A5%E8%AF%B7%E6%B1%82-%E5%93%8D%E5%BA%94)
      - [(1) 模块交互关系](#1-%E6%A8%A1%E5%9D%97%E4%BA%A4%E4%BA%92%E5%85%B3%E7%B3%BB)
      - [(2) 缺点](#2-%E7%BC%BA%E7%82%B9)
    - [10.1.2 方案2：使用消息服务](#1012-%E6%96%B9%E6%A1%882%E4%BD%BF%E7%94%A8%E6%B6%88%E6%81%AF%E6%9C%8D%E5%8A%A1)
      - [(1) 模块交互关系](#1-%E6%A8%A1%E5%9D%97%E4%BA%A4%E4%BA%92%E5%85%B3%E7%B3%BB-1)
      - [(2) 优点](#2-%E4%BC%98%E7%82%B9)
    - [10.1.3 使用消息服务是需要考虑的问题](#1013-%E4%BD%BF%E7%94%A8%E6%B6%88%E6%81%AF%E6%9C%8D%E5%8A%A1%E6%98%AF%E9%9C%80%E8%A6%81%E8%80%83%E8%99%91%E7%9A%84%E9%97%AE%E9%A2%98)
      - [(1) 消息处理语义（Message handling semantics）](#1-%E6%B6%88%E6%81%AF%E5%A4%84%E7%90%86%E8%AF%AD%E4%B9%89message-handling-semantics)
      - [(2) 消息可见性（Message Visibility）](#2-%E6%B6%88%E6%81%AF%E5%8F%AF%E8%A7%81%E6%80%A7message-visibility)
      - [(3) 消息编排（Message Choreography）](#3-%E6%B6%88%E6%81%AF%E7%BC%96%E6%8E%92message-choreography)
  - [10.2 Spring Cloud Stream介绍](#102-spring-cloud-stream%E4%BB%8B%E7%BB%8D)
  - [10.3 使用Spring Cloud Stream默认Channel传递消息](#103-%E4%BD%BF%E7%94%A8spring-cloud-stream%E9%BB%98%E8%AE%A4channel%E4%BC%A0%E9%80%92%E6%B6%88%E6%81%AF)
    - [(1) demo功能](#1-demo%E5%8A%9F%E8%83%BD)
    - [(2) 模块健康检查依赖](#2-%E6%A8%A1%E5%9D%97%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5%E4%BE%9D%E8%B5%96)
    - [10.3.1 在`docker-compose.yml`中添加Kafka和Redis](#1031-%E5%9C%A8docker-composeyml%E4%B8%AD%E6%B7%BB%E5%8A%A0kafka%E5%92%8Credis)
    - [10.3.2 为`organization-service`编写Message Producer](#1032-%E4%B8%BAorganization-service%E7%BC%96%E5%86%99message-producer)
      - [(1) 添加Maven依赖：`spring-cloud-stream`和`spring-cloud-starter-stream-kafka`](#1-%E6%B7%BB%E5%8A%A0maven%E4%BE%9D%E8%B5%96spring-cloud-stream%E5%92%8Cspring-cloud-starter-stream-kafka)
      - [(2) 绑定Spring Cloud Stream消息代理](#2-%E7%BB%91%E5%AE%9Aspring-cloud-stream%E6%B6%88%E6%81%AF%E4%BB%A3%E7%90%86)
      - [(3) 在UserContext的ThreadLocal存储中，添加organization ID](#3-%E5%9C%A8usercontext%E7%9A%84threadlocal%E5%AD%98%E5%82%A8%E4%B8%AD%E6%B7%BB%E5%8A%A0organization-id)
      - [(4) 添加消息发布代码（调用Source接口）](#4-%E6%B7%BB%E5%8A%A0%E6%B6%88%E6%81%AF%E5%8F%91%E5%B8%83%E4%BB%A3%E7%A0%81%E8%B0%83%E7%94%A8source%E6%8E%A5%E5%8F%A3)
      - [(5) 添加配置将Source绑定到Kafka](#5-%E6%B7%BB%E5%8A%A0%E9%85%8D%E7%BD%AE%E5%B0%86source%E7%BB%91%E5%AE%9A%E5%88%B0kafka)
    - [10.3.3 为`licensing-service`编写Message Consumer](#1033-%E4%B8%BAlicensing-service%E7%BC%96%E5%86%99message-consumer)
      - [(1) 消息传递过程](#1-%E6%B6%88%E6%81%AF%E4%BC%A0%E9%80%92%E8%BF%87%E7%A8%8B)
      - [(2) 添加Maven依赖：`spring-cloud-stream`和`spring-cloud-starter-stream-kafka`](#2-%E6%B7%BB%E5%8A%A0maven%E4%BE%9D%E8%B5%96spring-cloud-stream%E5%92%8Cspring-cloud-starter-stream-kafka)
      - [(3) 让`licensing-service`绑定消息代理，添加处理消息的方法](#3-%E8%AE%A9licensing-service%E7%BB%91%E5%AE%9A%E6%B6%88%E6%81%AF%E4%BB%A3%E7%90%86%E6%B7%BB%E5%8A%A0%E5%A4%84%E7%90%86%E6%B6%88%E6%81%AF%E7%9A%84%E6%96%B9%E6%B3%95)
      - [(4) 添加配置将Source绑定到Kafka](#4-%E6%B7%BB%E5%8A%A0%E9%85%8D%E7%BD%AE%E5%B0%86source%E7%BB%91%E5%AE%9A%E5%88%B0kafka)
    - [10.3.4 Seeing Message Service in action](#1034-seeing-message-service-in-action)
  - [10.4 使用Redis缓存](#104-%E4%BD%BF%E7%94%A8redis%E7%BC%93%E5%AD%98)
    - [10.4.1 使用Redis缓存查询](#1041-%E4%BD%BF%E7%94%A8redis%E7%BC%93%E5%AD%98%E6%9F%A5%E8%AF%A2)
      - [(1) 引入Spring Data Redis依赖项目](#1-%E5%BC%95%E5%85%A5spring-data-redis%E4%BE%9D%E8%B5%96%E9%A1%B9%E7%9B%AE)
      - [(2) 创建与Redis的连接](#2-%E5%88%9B%E5%BB%BA%E4%B8%8Eredis%E7%9A%84%E8%BF%9E%E6%8E%A5)
      - [(3) 定义`Spring Data Redis Repository`与Redis交互](#3-%E5%AE%9A%E4%B9%89spring-data-redis-repository%E4%B8%8Eredis%E4%BA%A4%E4%BA%92)
      - [(4) 查询逻辑封装](#4-%E6%9F%A5%E8%AF%A2%E9%80%BB%E8%BE%91%E5%B0%81%E8%A3%85)
      - [(5) 测试](#5-%E6%B5%8B%E8%AF%95)
    - [10.4.2 定义Spring Cloud自定义Channel传递消息](#1042-%E5%AE%9A%E4%B9%89spring-cloud%E8%87%AA%E5%AE%9A%E4%B9%89channel%E4%BC%A0%E9%80%92%E6%B6%88%E6%81%AF)
      - [(1) 创建并配置自定义Input Channel](#1-%E5%88%9B%E5%BB%BA%E5%B9%B6%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89input-channel)
      - [(2) 测试](#2-%E6%B5%8B%E8%AF%95)
      - [(3) 备注：创建并配置自定义Output Channel](#3-%E5%A4%87%E6%B3%A8%E5%88%9B%E5%BB%BA%E5%B9%B6%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89output-channel)
  - [10.5 小结](#105-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# CH10 Spring Cloud Srream事件驱动架构

EDA（事件驱动框架）

> 也叫做MDA（消息驱动框架），使用异步消息穿戴来表示事物额变化，从而构建高度解耦的系统

Spring Cloud Stream

> 用来构建继续消息的解耦方案，简化消息发布和消费的代码开发，同时使系统免受底层消息平台实现细节的影响

本章内容

> * 事件驱动架构与微服务的关系
> * 使用Spring Cloud Stream和Kafka发送和接受消息
> * 使用Spring Cloud Stream，Kafka，Redis实现分布式缓存

## 10.1 EDA（事件驱动框架）与微服务

缓存需求（Demo项目背景）

> organization-service调用花费时间长数据库负担重；但是organization相关的数据很少更高，并且查询主要都是使用主键查询来完成：因此适合使用缓存
>
> 该场景缓存解决方案有三个核心要求
>
> 1. 需要许可服务在所有实例之间保持一致
> 2. 不能使用内存缓存（这个场景的数据量大）
> 3. 当organization数据发生更新/删除时，希望数据使用方licensing-service能够意识到服务中的状态已经发生更改，进而让缓存失效

organization数据发生变化/删除时的操作

方案1（同步请求-响应）

> 调用organization-service的REST端点时，oragnization-service用同步阻塞的方式调用licensing-service

方案2（使用消息服务）

> 方法2：调用organization-service的REST端点时，oragnization-service向Message Queue发布关于这次变更的消息，licensing-service侦听这个消息事件并从缓存中删除相关数据

### 10.1.1 方案1：使用同步请求-响应

数据缓存：使用[Redis](https://redis.io/)

#### (1) 模块交互关系

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch10_sync_req_resp_model.jpg" width="650" /></div>
>
> 对应图中的序号：
>
> 1. 用户请求licensing-service
> 2. licensing-service在处理这个请求时会用到organization信息、因此查询redis缓存
> 3. Redis缓存miss，licensing-service需要请求organization-service来获取该信息
> 4. 某个时间，某个客户端调用organization-service更新了organization信息，需要通知licensing-service让缓存失效

#### (2) 缺点

> 1. 两个service紧密耦合
>     * 需要让licensing-service为organization-service提供一个端点
>     * 或者让oragnization-service直接访问为licensing-service架设的redis，这是一个很大的禁忌
> 2. 耦合引入了脆弱性
>     * 如果licensing-service发生了更改，则organization-service必须也更改
>     * 如果licensing-service运行缓慢，organization-service也收到影响
>     * 假如让organization-service访问redis，则redis故障时会连同organization-service一同影响
> 3. 不灵活，organization-service添加新的使用者变的困难（需要添加新的依赖）

### 10.1.2 方案2：使用消息服务

数据缓存：使用[Redis](https://redis.io/)

#### (1) 模块交互关系

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch10_msg_queue_model.jpg" width="650" /></div>
>
> 安装一个消息队列，并配置一个消息主题（TOPIC）：
>
> * organization-service更新/删除oragnization数据时发布消息到该TOPIC上；
> * licensing-service订阅该TOPIC，收到消息时清除对应的Redis缓存

#### (2) 优点

> 1. 松耦合（Loose coupling）：减少了不必要的端点和服务调用，让服务解耦降低依赖
> 2. 耐用性（Durability）：licensing-service故障时organization-service仍然可以正常工作；而organization-service故障时可以用缓存做方案降级
> 3. 可扩展（Scalability）：可以水平扩展通过增加节点的方式来处理队列消费速度不足的问题
> 4. 灵活性（Flexibility）：通过增加队列消费者来为organization-service灵活地增加新的使用者

### 10.1.3 使用消息服务是需要考虑的问题

> 是否使用Messaging Architecture也需要权衡，该方案也尤其复杂的一面，需要关注以下方面

#### (1) 消息处理语义（Message handling semantics）

> * 消息是否需要排序？如果被无序处理会发生什么？
> * 当消息处理抛出异常、或者error被乱序处理、或者前一条消息处理错误时收到了后续消息，这些状况该如何处理？

#### (2) 消息可见性（Message Visibility）

> * 同步服务调用时，相关的异步消息还没有到达
> * 诸如correlation id提取等功能该如何实现

#### (3) 消息编排（Message Choreography）

> * 异步消息的分散性和乱序性，使得在debug时，根据日志来推断业务调用过程变得复杂

## 10.2 Spring Cloud Stream介绍

> [Spring Cloud Stream](https://spring.io/projects/spring-cloud-stream)用来简化Spring微服务项目中的消息集成，支持Kafka、RabbitMQ等多种消息队列，为其提供抽象化的代码封装，使得业务代码不依赖特定的消息队列实现。

理解Spring Cloud Stream对消息队列的抽象化

> Spring Cloud Stream包含四个组件：(1) Source (2) Channel (3) Binder (4) Sink
>
> * `Source`：提供给生产者、用来发布的消息的接口
>
> * `Channel`：消息队列的抽象，同时底层队列的配置信息都封装在Channel中、不会暴露给业务逻辑
>
> * `Binder`：封装并隐藏底层消息队列的API
>
> * `Sink`：监听消息队列并反序列化收到的消息，返回POJO给消费者的业务代码
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch10_spring_cloud_stream_intro.jpg" width="650" /></div>

## 10.3 使用Spring Cloud Stream默认Channel传递消息

### (1) demo功能

> 编写一个简单场景：oragnization-service通过kafka发送消息给licensing-service
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch10_orgsvc_example_intro.jpg" width="650" /></div>

### (2) 模块健康检查依赖

> 为了让所有模块正确启动，需要确保在config-server先完整启动并且开始工作之后，再启动其他模块。这需要能够对config-server进行健康检查.

添加健康检查

> 相关修改：[https://github.com/fangkun119/manning-smia/commit/13341b20f3739a2923fd30fde38729b904dca65b](https://github.com/fangkun119/manning-smia/commit/13341b20f3739a2923fd30fde38729b904dca65b)
>
> 这个commit除了添加`健康检查依赖`，还让所有模块都统一使用Spring Cloud Load Balancer来替代Ribbon

### 10.3.1 在`docker-compose.yml`中添加Kafka和Redis

> ~~~yml
> zookeeper:
> 	image: wurstmeister/zookeeper:latest
> 	ports:
> 		- 2181:2181
> 	networks:
> 		backend:
> 			aliases:
> 				- "zookeeper"
> kafkaserver:
> 	# 镜像使用说明：https://hub.docker.com/r/wurstmeister/kafka
> 	image: wurstmeister/kafka:latest
> 	ports:
> 		- 9092:9092
> 	environment:
> 		- KAFKA_ADVERTISED_HOST_NAME=kafka
> 		- KAFKA_ADVERTISED_PORT=9092
> 		- KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181
> 		# 主题1名称:分区数:副本数,主题2名称:分区数:副本数
> 		- KAFKA_CREATE_TOPICS=dresses:1:1,ratings:1:1,orgChangeTopic:1:1
> 	volumes:
> 		- "/var/run/docker.sock:/var/run/docker.sock"
> 	depends_on:
> 		- zookeeper
> 	networks:
> 		backend:
> 			aliases:
> 				- "kafka"
> redisserver:
> 	image: redis:alpine
> 	ports:
> 		- 6379:6379
> 	networks:
> 		backend:
> 			aliases:
> 				- "redis"
> ~~~
>
> 代码：[../chapter10/docker/docker-compose.yml](../chapter10/docker/docker-compose.yml)
>

### 10.3.2 为`organization-service`编写Message Producer

> organization-service在每次添加/更新/删除organization数据时，向Kafka主题发布消息，指示发生更改事件。消息内容包括：(1) 更改事件关联的oranization id；(2) 操作的类型（添加、更新、删除）

#### (1) 添加Maven依赖：`spring-cloud-stream`和`spring-cloud-starter-stream-kafka`

> ~~~xml
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-stream</artifactId>
> </dependency>
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-starter-stream-kafka</artifactId>
> </dependency>
> ~~~
>
> 代码：[../chapter10/organization-service/pom.xml](../chapter10/organization-service/pom.xml)

#### (2) 绑定Spring Cloud Stream消息代理

> ```java
> ...
> import org.springframework.cloud.stream.messaging.Source;
> 
> @SpringBootApplication
> @RefreshScope // 来自Spring Boot Actuator，会提供一个/refresh端点用于配置更新
> @EnableBinding(Source.class) //将这个服务绑定到消息代理
> public class OrganizationServiceApplication {
> 	public static void main(String[] args) {
> 		SpringApplication.run(OrganizationServiceApplication.class, args);
> 	}
> }
> ```
>
> 代码：[../chapter10/organization-service/src/main/java/com/optimagrowth/organization/OrganizationServiceApplication.java](../chapter10/organization-service/src/main/java/com/optimagrowth/organization/OrganizationServiceApplication.java)

#### (3) 在UserContext的ThreadLocal存储中，添加organization ID

> ```java
> @Component
> public class UserContext {
> 	...
> 
> 	// 添加organization id
> 	public static final String ORG_ID = "tmx-org-id";
> 	private static final ThreadLocal<String> orgId = new ThreadLocal<String>();
> 	public static String getOrgId() { return orgId.get(); }
> 	public static void setOrgId(String aOrg) {orgId.set(aOrg);}
>     
> 	public static HttpHeaders getHttpHeaders(){
> 		HttpHeaders httpHeaders = new HttpHeaders();
> 		httpHeaders.set(CORRELATION_ID, getCorrelationId());
> 		return httpHeaders;
> 	}
> }
> ```
>
> 代码：[../chapter10/organization-service/src/main/java/com/optimagrowth/organization/utils/UserContext.java](../chapter10/organization-service/src/main/java/com/optimagrowth/organization/utils/UserContext.java)

#### (4) 添加消息发布代码（调用Source接口）

> ```java
> @Component
> public class SimpleSourceBean {
> 	private static final Logger logger = LoggerFactory.getLogger(SimpleSourceBean.class);
> 
> 	// Source接口的实现类对象从外部注入
> 	private Source source;
> 	@Autowired
> 	public SimpleSourceBean(Source source){
> 		this.source = source;
> 	}
> 
> 	// 向Source发布消息
> 	public void publishOrganizationChange(ActionEnum action, String organizationId){
> 		logger.debug("Sending Kafka message {} for Organization Id: {}", action, organizationId);
>         // 普通的POJO、封装下面的四个属性
> 		OrganizationChangeModel change =  new OrganizationChangeModel(
> 				OrganizationChangeModel.class.getTypeName(), // "OrganizationChangeModel"
> 				action.toString(), 							 // 四种取值：GET, CREATED, UPDATED, DELETED
> 				organizationId,								 // 阻止ID
> 				UserContext.getCorrelationId()				 // 用于分布式调用追踪的ID，将属于同一transaction的数据串联起来
>         );
> 
>         // Source只暴露出一个output方法
>         // * source.output()方法返回MessageChannel，用来发送消息
>         // * 单一channel场景下使用source发送消息非常简洁
>         // * 后面会演示多channel场景
> 		// MessageBuilder将POJO转换成Message对象以供发送
> 		source.output().send(MessageBuilder.withPayload(change).build());
> 	}
> }
> ```
>
> 完整代码：
>
> * SimpleSourceBean：[/organization-service/src/main/java/com/optimagrowth/organization/events/source/SimpleSourceBean.java](../chapter10/organization-service/src/main/java/com/optimagrowth/organization/events/source/SimpleSourceBean.java)
>
> * OrganizationChangeModel：[/organization-service/src/main/java/com/optimagrowth/organization/events/model/OrganizationChangeModel.java](../chapter10/organization-service/src/main/java/com/optimagrowth/organization/events/model/OrganizationChangeModel.java)

#### (5) 添加配置将Source绑定到Kafka

> ~~~properties
> ...
> spring.cloud.stream.bindings.output.destination=orgChangeTopic
> spring.cloud.stream.bindings.output.content-type=application/json
> spring.cloud.stream.kafka.binder.zkNodes=zookeeper
> spring.cloud.stream.kafka.binder.brokers=kafka
> ~~~
>
> 代码：[../chapter10/configserver/src/main/resources/config/organization-service.properties](../chapter10/configserver/src/main/resources/config/organization-service.properties)
>
> 其中
>
> * `"kafka"`和`"zookeeper"`是[docker-compose.yml](../chapter10/docker/docker-compose.yml)中kafkaserver和zookeeper的网络名
> * `orgChangeTopic`是用来传输消息的kafka topic
> * `application/json`是数据序列化格式
>
> 消息内容：只通知消息订阅方某个organization id的数据发生了变化，但不会告知变化的内容，而是让他们触发缓存失效，到`organization-service`来取数据的最新版本。

### 10.3.3 为`licensing-service`编写Message Consumer

#### (1) 消息传递过程

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch10_licensing_svc_example_intro.jpg" width="600" /></div>

#### (2) 添加Maven依赖：`spring-cloud-stream`和`spring-cloud-starter-stream-kafka`

> ~~~xml
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-stream</artifactId>
> </dependency>
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-starter-stream-kafka</artifactId>
> </dependency>
> ~~~
>
> 代码：[../chapter10/licensing-service/pom.xml](../chapter10/licensing-service/pom.xml)

#### (3) 让`licensing-service`绑定消息代理，添加处理消息的方法

> ~~~java
> @SpringBootApplication
> ...
> @EnableBinding(Source.class) // 使用默认的Source接口、绑定这个服务到消息代理上
> public class LicenseServiceApplication {
> 	public static void main(String[] args) {
> 		SpringApplication.run(LicenseServiceApplication.class, args);
> 	}
> 
> 	...
> 
> 	// 用来监听消息的方法
> 	@StreamListener(Sink.INPUT)
> 	public void loggerSink(OrganizationChangeModel orgChange) {
> 		logger.debug("Received {} event for the organization id {}", orgChange.getAction(), orgChange.getOrganizationId());
> 	}
> }
> ~~~
>
> 代码：[../chapter10/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java](../chapter10/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java)

#### (4) 添加配置将Source绑定到Kafka

> ~~~properties
> # spring.cloud.stream.bindings.inboundOrgChanges.destination=orgChangeTopic
> # spring.cloud.stream.bindings.inboundOrgChanges.content-type=application/json
> # spring.cloud.stream.bindings.inboundOrgChanges.group=licensingGroup
> spring.cloud.stream.bindings.input.destination=orgChangeTopic
> spring.cloud.stream.bindings.input.content-type=application/json
> spring.cloud.stream.bindings.input.group=licensingGroup
> # zookeeper和kafka是docker-compose.yml中的两个容器的网络名，在容器环境中可以当做主机名来访问
> spring.cloud.stream.kafka.binder.zkNodes=zookeeper
> spring.cloud.stream.kafka.binder.brokers=kafka
> ~~~
>
> `licensingGroup`是Kafka Consumer的消费者组概念，用于让多个消费者实例分摊接收到的数据（每条消息只会被组内的一个消费者取走），具体说明如下图
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch10_consumer_group.jpg" width="600" /></div>

### 10.3.4 Seeing Message Service in action

> 测试，用PostMan将如下数据POST到organization-service上
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch10_create_orgsvc_rec.jpg" width="500" /></div>
>
> organization-service将数据放入Kafka，被licensing service消费 ，并打印如下日志
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch10_create_orgsvc_rec_log.jpg" width="600" /></div>
>
> POST请求的payload：
>
> ~~~json
> {
> 	"name":"Ostock",
> 	"contactName":"Illary Huaylupo",
> 	"contactEmail":"illaryhs@gmail.com",
> 	"contactPhone":"888888888"
> }
> ~~~
>
> 另外发送请求需要登录授权并携带Bearer令牌，具体方法见上一章

## 10.4 使用Redis缓存

### 10.4.1 使用Redis缓存查询

> Demo将为licensing-service引入Redis缓存，缓存命中时使用缓存中的内容，缓存没有命中时调用organization-service。具体需要做四件事：
>
> 1. 引入Spring Data Redis依赖项目
> 2. 创建与Redis的连接
> 3. 定义Spring Data Redis Repositories与Redis交互
> 4. 使用Redis和Licensing Service来存储和读取organization相关的数据

#### (1) 引入Spring Data Redis依赖项目

> ~~~xml
> <dependency>
> 	<groupId>org.springframework.data</groupId>
> 	<artifactId>spring-data-redis</artifactId>
> </dependency>
> <dependency>
> 	<groupId>redis.clients</groupId>
> 	<artifactId>jedis</artifactId>
> 	<type>jar</type>
> </dependency>
> ~~~
>
> 完整代码：[../chapter10/licensing-service/pom.xml](../chapter10/licensing-service/pom.xml)
>
> [Jedis]()是用来与Redis Server通信的开源项目

#### (2) 创建与Redis的连接

装配两个Bean：`JedisConnectionFactory`和`RedisTemplate<String, Object>`

> ~~~java
> @SpringBootApplication
> ...
> @EnableBinding(Sink.class)
> public class LicenseServiceApplication {
> 	// 注入ServiceConfig Bean
> 	@Autowired
> 	private ServiceConfig serviceConfig;
> 
> 	@Bean
> 	JedisConnectionFactory jedisConnectionFactory() {
> 		String hostname = serviceConfig.getRedisServer();
> 		int port = Integer.parseInt(serviceConfig.getRedisPort());
> 		RedisStandaloneConfiguration redisStandaloneConfiguration = new RedisStandaloneConfiguration(hostname, port);
> 		return new JedisConnectionFactory(redisStandaloneConfiguration);
> 	}
> 
> 	@Bean
> 	public RedisTemplate<String, Object> redisTemplate() {
> 		RedisTemplate<String, Object> template = new RedisTemplate<>();
> 		template.setConnectionFactory(jedisConnectionFactory());
> 		return template;
> 	}
> 
>     ...
> }
> ~~~
>
> 完整代码：[../chapter10/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java](../chapter10/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java)

其中ServiceConfig的内容如下

> ~~~java
> @Component @Getter
> public class ServiceConfig{
> 	...
> 
> 	@Value("${redis.server}")
> 	private String redisServer="";
> 
> 	@Value("${redis.port}")
> 	private String redisPort="";
> }
> ~~~
>
> 完整代码：[../chapter10/licensing-service/src/main/java/com/optimagrowth/license/config/ServiceConfig.java](../chapter10/licensing-service/src/main/java/com/optimagrowth/license/config/ServiceConfig.java)

载入到ServiceConfig中的配置

> ~~~properties
> ...
> redis.server = redis
> redis.port = 6379
> ~~~
>
> 完整代码：[../chapter10/configserver/src/main/resources/config/licensing-service.properties](../chapter10/configserver/src/main/resources/config/licensing-service.properties)

#### (3) 定义`Spring Data Redis Repository`与Redis交互

定义`OrganizationRedisRepository`、其基类提供用于增删查改的各种方法

> ~~~java
> @Repository
> public interface OrganizationRedisRepository extends CrudRepository<Organization,String>  {
> }
> ~~~
>
> 代码：[../chapter10/licensing-service/src/main/java/com/optimagrowth/license/repository/OrganizationRedisRepository.java](../chapter10/licensing-service/src/main/java/com/optimagrowth/license/repository/OrganizationRedisRepository.java)

封装数据的Model类

> ~~~java
> @Getter @Setter @ToString
> @RedisHash("organization")
> public class Organization extends RepresentationModel<Organization> {
> 	@Id
> 	String id;
> 	String name;
> 	String contactName;
> 	String contactEmail;
> 	String contactPhone;
> }
> ~~~
>
> 代码：[../chapter10/licensing-service/src/main/java/com/optimagrowth/license/model/Organization.java](../chapter10/licensing-service/src/main/java/com/optimagrowth/license/model/Organization.java)

#### (4) 查询逻辑封装

> 修改licensing-service，在访问oragnization-service之前先检查Redis缓存，缓存中没有数据时再访问oragnization-service
>
> ~~~java
> @Component
> public class OrganizationRestTemplateClient {
> 	private static final Logger logger = LoggerFactory.getLogger(OrganizationRestTemplateClient.class);
> 
>     @Autowired
> 	RestTemplate restTemplate;
> 
> 	@Autowired
> 	OrganizationRedisRepository redisRepository;
> 
> 	public Organization getOrganization(String organizationId){
> 		// 检查Redis缓存
> 		logger.debug("In Licensing Service.getOrganization: {}", UserContext.getCorrelationId());
> 		Organization organization = checkRedisCache(organizationId);
> 		if (organization != null){
> 			logger.debug("I have successfully retrieved an organization {} from the redis cache: {}", organizationId, organization);
> 			return organization;
> 		}
> 		// 缓存没有命中时，访问organization-service
> 		logger.debug("Unable to locate organization from the redis cache: {}.", organizationId);
> 		ResponseEntity<Organization> restExchange = restTemplate.exchange(
> 			"http://gateway:8072/organization/v1/organization/{organizationId}",
> 			HttpMethod.GET, null, Organization.class, organizationId);
> 		// 将从oragnization-service查到的数据存入缓存
> 		organization = restExchange.getBody();
> 		if (organization != null) {
> 			cacheOrganizationObject(organization);
> 		}
> 		return restExchange.getBody();
> 	}
> 
> 	private Organization checkRedisCache(String organizationId) {
> 		try {
> 			return redisRepository.findById(organizationId).orElse(null);
> 		}catch (Exception ex){
> 			logger.error("Error encountered while trying to retrieve organization {} check Redis Cache.  Exception {}", organizationId, ex);
> 			return null;
> 		}
> 	}
> 	
> 	private void cacheOrganizationObject(Organization organization) {
>         try {
>         	redisRepository.save(organization);
>         }catch (Exception ex){
>             logger.error("Unable to cache organization {} in Redis. Exception {}", organization.getId(), ex);
>         }
>     }
> }
> ~~~
>
> 代码： [../chapter10/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationRestTemplateClient.java](../chapter10/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationRestTemplateClient.java)

#### (5) 测试

> 访问`http://localhost:8072/license/v1/organization/e839ee96-28de-4f67-bb79-870ca89743a0/license/279709ff-e6d5-4a54-8b55-a5c37542025b`
>
> 日志如下
>
> ~~~txt
> licensingservice_1       | DEBUG 1 --- [nio-8080-exec-4] c.o.l.s.c.OrganizationRestTemplateClient : Unable to locate organization from the redis cache: e839ee96-28de-4f67-bb79-870ca89743a0. 
> licensingservice_1       | DEBUG 1 --- [nio-8080-exec-7] c.o.l.s.c.OrganizationRestTemplateClient : I have successfully retrieved an organization e839ee96-28de-4f67-bb79-870ca89743a0 from the redis cache: Organization(id=e839ee96-28de-4f67-bb79-870ca89743a0, name=Ostock, contactName=Illary Huaylupo, contactEmail=illaryhs@gmail.com, contactPhone=888888888)
> ~~~

### 10.4.2 定义Spring Cloud自定义Channel传递消息

> 之前的例子中都是使用Source和Sink接口覆盖的默认Channel来发送和接收消息

这个小节演示使用自定义Channel来传递消息，用这个方法可以定义多个自定义Channel映射到多个Kafka Topic，进而使用多个不同的队列收发消息。步骤如下：

#### (1) 创建并配置自定义Input Channel

对于消费者来说 ，方法如下

> ~~~java
> public interface CustomChannels { 
> 	// licensing-service将监听这个channel
> 	// channel在配置文件中映射到的配置项为”spring.cloud.stream.bindings.inboundOrgChanges.*“
>     // 以这样的方式，可以在配置文件中配置多个channel，在代码中创建多个类似的接口
> 	@Input("inboundOrgChanges")
> 	SubscribableChannel orgs();
> }
> ~~~
>
> 代码：[../chapter10/licensing-service/src/main/java/com/optimagrowth/license/events/CustomChannels.java](../chapter10/licensing-service/src/main/java/com/optimagrowth/license/events/CustomChannels.java)
>
> 对应的配置修改如下
>
> ~~~properties
> # 用于默认Channel的配置
> # spring.cloud.stream.bindings.input.destination=orgChangeTopic 
> # spring.cloud.stream.bindings.input.content-type=application/json
> # spring.cloud.stream.bindings.input.group=licensingGroup
> # 用于名为inboundOrgChanges的Channel的配置
> spring.cloud.stream.bindings.inboundOrgChanges.destination=orgChangeTopic
> spring.cloud.stream.bindings.inboundOrgChanges.content-type=application/json
> spring.cloud.stream.bindings.inboundOrgChanges.group=licensingGroup
> spring.cloud.stream.kafka.binder.zkNodes=zookeeper
> spring.cloud.stream.kafka.binder.brokers=kafka
> ~~~
>
> 代码：[../chapter10/configserver/src/main/resources/config/licensing-service.properties](../chapter10/configserver/src/main/resources/config/licensing-service.properties)

#### (2) 测试

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch10_customer_channel_log.jpg" width="800" /></div>
>

#### (3) 备注：创建并配置自定义Output Channel

对于生产者来说，方法类似，只是要换成@OutputChannel注解

> ~~~java
> @OutputChannel(“orgChangeTopic”)
> MessageChannel outboundOrg();
> ~~~
>
> 配置也做类似的修改
>
> ~~~properties
> # spring.cloud.stream.bindings.output.destination=orgChangeTopic
> # spring.cloud.stream.bindings.output.content-type=application/json
> spring.cloud.stream.bindings.orgChangeTopic.destination=orgChangeTopic
> spring.cloud.stream.bindings.orgChangeTopic.content-type=application/json
> spring.cloud.stream.kafka.binder.zkNodes=zookeeper
> spring.cloud.stream.kafka.binder.brokers=kafka
> ~~~

## 10.5 小结

> 略



