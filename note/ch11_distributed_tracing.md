<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [CH11 使用Spring Cloud Sleuth和Zipkin进行分布式追踪](#ch11-%E4%BD%BF%E7%94%A8spring-cloud-sleuth%E5%92%8Czipkin%E8%BF%9B%E8%A1%8C%E5%88%86%E5%B8%83%E5%BC%8F%E8%BF%BD%E8%B8%AA)
  - [11.1 Spring Cloud Sleuth及correlation ID](#111-spring-cloud-sleuth%E5%8F%8Acorrelation-id)
    - [11.1.1 在oragnization-service添加Spring Cloud Sleuth依赖](#1111-%E5%9C%A8oragnization-service%E6%B7%BB%E5%8A%A0spring-cloud-sleuth%E4%BE%9D%E8%B5%96)
    - [11.1.2 引入Spring Cloud Sleuth后的日志格式](#1112-%E5%BC%95%E5%85%A5spring-cloud-sleuth%E5%90%8E%E7%9A%84%E6%97%A5%E5%BF%97%E6%A0%BC%E5%BC%8F)
  - [11.2 Spring Cloud Sleuth日志聚合](#112-spring-cloud-sleuth%E6%97%A5%E5%BF%97%E8%81%9A%E5%90%88)
    - [11.2.1 Spring Cloud Sleuth / ELK Stack实践](#1121-spring-cloud-sleuth--elk-stack%E5%AE%9E%E8%B7%B5)
    - [11.2.2 在服务中配置`logstash-logback`](#1122-%E5%9C%A8%E6%9C%8D%E5%8A%A1%E4%B8%AD%E9%85%8D%E7%BD%AElogstash-logback)
      - [(1) 添加`logstash-logback-encoder` Maven依赖](#1-%E6%B7%BB%E5%8A%A0logstash-logback-encoder-maven%E4%BE%9D%E8%B5%96)
      - [(2) 创建Logstash TCP Addpender以发送日志给LogStash（并转换成JSON格式）](#2-%E5%88%9B%E5%BB%BAlogstash-tcp-addpender%E4%BB%A5%E5%8F%91%E9%80%81%E6%97%A5%E5%BF%97%E7%BB%99logstash%E5%B9%B6%E8%BD%AC%E6%8D%A2%E6%88%90json%E6%A0%BC%E5%BC%8F)
        - [(a) 方法](#a-%E6%96%B9%E6%B3%95)
        - [(b) 方法1：使用`LogstashEncoder`](#b-%E6%96%B9%E6%B3%951%E4%BD%BF%E7%94%A8logstashencoder)
        - [(c) 方法2：使用`LoggingEventCompositeJsonEncoder`](#c-%E6%96%B9%E6%B3%952%E4%BD%BF%E7%94%A8loggingeventcompositejsonencoder)
    - [11.2.3 在docker中定义和运行ELK stack](#1123-%E5%9C%A8docker%E4%B8%AD%E5%AE%9A%E4%B9%89%E5%92%8C%E8%BF%90%E8%A1%8Celk-stack)
      - [(1) 创建Logstash配置文件](#1-%E5%88%9B%E5%BB%BAlogstash%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
        - [(a) Logstash配置的组成](#a-logstash%E9%85%8D%E7%BD%AE%E7%9A%84%E7%BB%84%E6%88%90)
        - [(b) Logstash配置文件编写](#b-logstash%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%BC%96%E5%86%99)
        - [(c) Logstash插件文档](#c-logstash%E6%8F%92%E4%BB%B6%E6%96%87%E6%A1%A3)
      - [(2) 在docker-compose.yml中配置ELK Stack应用程序](#2-%E5%9C%A8docker-composeyml%E4%B8%AD%E9%85%8D%E7%BD%AEelk-stack%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F)
      - [(3) 创建容器启动 服务](#3-%E5%88%9B%E5%BB%BA%E5%AE%B9%E5%99%A8%E5%90%AF%E5%8A%A8-%E6%9C%8D%E5%8A%A1)
        - [启动命令](#%E5%90%AF%E5%8A%A8%E5%91%BD%E4%BB%A4)
        - [Docker内存问题解决](#docker%E5%86%85%E5%AD%98%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3)
    - [11.2.4 配置Kibana](#1124-%E9%85%8D%E7%BD%AEkibana)
    - [11.2.5 在Kibana中搜索Spring Cloud Sleuth Trace ID](#1125-%E5%9C%A8kibana%E4%B8%AD%E6%90%9C%E7%B4%A2spring-cloud-sleuth-trace-id)
    - [11.2.6 将Trace ID添加到HTTP Response中](#1126-%E5%B0%86trace-id%E6%B7%BB%E5%8A%A0%E5%88%B0http-response%E4%B8%AD)
      - [(1) 背景：`Spring Cloud Sleuth`默认不会在Response中添加Trace ID等信息](#1-%E8%83%8C%E6%99%AFspring-cloud-sleuth%E9%BB%98%E8%AE%A4%E4%B8%8D%E4%BC%9A%E5%9C%A8response%E4%B8%AD%E6%B7%BB%E5%8A%A0trace-id%E7%AD%89%E4%BF%A1%E6%81%AF)
      - [(2) 两种方法](#2-%E4%B8%A4%E7%A7%8D%E6%96%B9%E6%B3%95)
      - [(3) 使用Spring Cloud Gateway过滤器来添加Trace ID](#3-%E4%BD%BF%E7%94%A8spring-cloud-gateway%E8%BF%87%E6%BB%A4%E5%99%A8%E6%9D%A5%E6%B7%BB%E5%8A%A0trace-id)
        - [(a) 添加Maven依赖](#a-%E6%B7%BB%E5%8A%A0maven%E4%BE%9D%E8%B5%96)
        - [(b) 编写过滤器](#b-%E7%BC%96%E5%86%99%E8%BF%87%E6%BB%A4%E5%99%A8)
        - [(c) 测试](#c-%E6%B5%8B%E8%AF%95)
  - [11.3 使用Open Zipkin进行分布式Tracing](#113-%E4%BD%BF%E7%94%A8open-zipkin%E8%BF%9B%E8%A1%8C%E5%88%86%E5%B8%83%E5%BC%8Ftracing)
    - [11.3.1 在gateway-server添加`Spring Cloud Sleuth`和`Zipkin`依赖](#1131-%E5%9C%A8gateway-server%E6%B7%BB%E5%8A%A0spring-cloud-sleuth%E5%92%8Czipkin%E4%BE%9D%E8%B5%96)
    - [11.3.2 配置各个服务使其指向Zipkin](#1132-%E9%85%8D%E7%BD%AE%E5%90%84%E4%B8%AA%E6%9C%8D%E5%8A%A1%E4%BD%BF%E5%85%B6%E6%8C%87%E5%90%91zipkin)
    - [11.3.3 配置Zipkin Server](#1133-%E9%85%8D%E7%BD%AEzipkin-server)
    - [11.3.4 采样粒度](#1134-%E9%87%87%E6%A0%B7%E7%B2%92%E5%BA%A6)
    - [11.3.5 使用Zipkin来追踪Transactinos](#1135-%E4%BD%BF%E7%94%A8zipkin%E6%9D%A5%E8%BF%BD%E8%B8%AAtransactinos)
      - [(1) 使用例子](#1-%E4%BD%BF%E7%94%A8%E4%BE%8B%E5%AD%90)
    - [11.3.6 可视化复杂的Transaction内部调用关系](#1136-%E5%8F%AF%E8%A7%86%E5%8C%96%E5%A4%8D%E6%9D%82%E7%9A%84transaction%E5%86%85%E9%83%A8%E8%B0%83%E7%94%A8%E5%85%B3%E7%B3%BB)
    - [11.3.7 捕获消息队列的Trace信息](#1137-%E6%8D%95%E8%8E%B7%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97%E7%9A%84trace%E4%BF%A1%E6%81%AF)
    - [11.3.8 添加自定义Span](#1138-%E6%B7%BB%E5%8A%A0%E8%87%AA%E5%AE%9A%E4%B9%89span)
  - [11.4 小结](#114-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

#  CH11 使用Spring Cloud Sleuth和Zipkin进行分布式追踪

本章研究以下内容：  

> - 使用correlation ID将跨多个服务的transaction（一次来自客户的调用）关联在一起
> - 将来自各种服务的日志数据聚合到一个可搜索的数据源中
> - 可视化跨多个服务的用户事务流，并了解事务性能特征的每个部分。
> - 使用Elasticsearch Logstash和Kibana（ELK）Stack实时分析，搜索和可视化日志数据

涉及的技术如下：

> - 使用Spring Cloud Sleuth将跟踪信息注入服务调用
> - 使用日志聚合查看分布式事务的日志
> - 使用ELK堆栈实时转换，搜索，分析和可视化日志数据
> - 使用OpenZipkin可视化跨多个服务的Transaction
> - 使用Spring Cloud Sleuth和Zipkin自定义跟踪信息

对这些技术的简介如下：

> * Spring Cloud Sleuth（[https://cloud.spring.io/spring-cloud-sleuth/reference/html/](https://cloud.spring.io/spring-cloud-sleuth/reference/html/)）：使用tracing id (correlation id)来检测传入的HTTP请求、通过添加过滤器来实现
> * Zipkin（[https://zipkin.io](https://zipkin.io)）：开源数据可视化工具，可以显式跨多个service的事物流，通过将transaction分隔成各个组成部分、有助于从视觉上识别可能存在的性能热点
> * ELK Stack（[https://www.elastic.co/what-is/elk-stack](https://www.elastic.co/what-is/elk-stack)）：Elasticsearch，Logstash和Kibana。用于实时分析，搜索和可视化日志。
>     * Elasticsearch是分布式分析引擎，适用于所有类型的数据（结构化，非结构化，数字，文本等）。
>     * Logstash是服务器端数据处理管道，能够同时从多个源中提取数据并进行转换
>     * Kibana是可视化和数据管理工具

原书章节：

> https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-11/v-8/

Demo代码：

> [../chapter11/](../chapter11/)

## 11.1 Spring Cloud Sleuth及correlation ID

> 第8章（[ch08_spring_cloud_gateway.md](ch08_spring_cloud_gateway.md)）中使用`Filter`/`Context`/`Interceptor`，在外部客户端调用系统（发起一个transaction）时，生成全局唯一的correlation id，并让它在服务调用之间传播。
>
> 而这些代码其实可以通过Spring Cloud Sleuth来简化，Spring Cloud Sleuth提供如下功能：
>
> * 封装correlation id的创建和传播逻辑
> * 将相关信息添加到Spring MDC（Mapped Diagnostic Context）中，供SL4J和Logback使用
> * 将服务调用跟踪尽心发布到Zipkin调用追踪平台（可选）

### 11.1.1 在oragnization-service添加Spring Cloud Sleuth依赖

> ~~~xml
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-starter-sleuth</artifactId>
> </dependency>
> ~~~
>
> 代码：[../chapter11/organization-service/pom.xml](../chapter11/organization-service/pom.xml)

引入`spring-cloud-starter-sleuth`之后，organization-service可以：

> 检查HTTP调用，确定是否存在Spring Cloud Sleuth跟踪信息，如果存在
>
> * 则捕获、添加到Spring MDC、传递给业务代码以用于日志输出
> * 并将该信息注入到每一个outbound HTTP call以及异步消息队列的Message Channel中

###  11.1.2 引入Spring Cloud Sleuth后的日志格式

日志格式

> 以`curl -X GET http://localhost:8072/organization/v1/organization/95c0dab4-0a7e-48f8-805a-0ba31c3687b8`为例，输出如下
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_sleuth_log_fields.jpg" width="800" /></div>
>
> 对应图中的序号：
>
> 1. Application Name：对应于这个服务的`spring.application.name`配置
>2. Trace ID：代表整个交易的唯一ID
> 3. Span ID： 代表交易中一个局部过程的唯一ID，参与交易的每个service都有自己的Span ID，可以参与Zipkin的可视化
>4. Send to Zipkin：表示是否发送给Zipkin。为了减少Zipkin的负载、Spring Cloud Sleuth容许指定数据采样规则以决定是否将该数据发送给Zipkin

跨服务调用

> 下面的日志包含了多个模块的日志，可以看到这个调用产生了服务之间的调用或消息传递，输出的日志都具有**相同**的Trace ID，但是不同服务输出日志中的Span ID不同
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_tracing_id_in_logs.jpg" width="800" /></div>
>

##  11.2 Spring Cloud Sleuth日志聚合

聚合方法

> 使用流式传输，将分散在各个容器中的日志，聚合到集中式聚合点；在该节点上为日志数据建立索引使其可以被搜索
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_tracing_approach.jpg" width="700" /></div>

实现日志聚合有多种技术可以选择

> ##### Table 11.1 Log Aggregation Solutions to use with Spring Boot
>
> | Product Name                          | Implementation Models                                 | Notes                                                        |
> | ------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
> | Elasticsearch, Logstash, Kibana (ELK) | Open sourceCommercialTypically implemented on premise | https://www.elastic.co/what-is/elk-stack General-purpose search engine Can do log-aggregation through the ELK-stack |
> | Graylog                               | Open sourceCommercialOn-premise                       | https://www.graylog.org/An open-source platform that’s designed to be installed on-premise |
> | Splunk                                | Commercial only On-premise and cloud-based            | https://www.splunk.com/Oldest and most comprehensive of the log management and aggregation tools Initially an on-premise solution, but have since offered a cloud offering |
> | Sumo Logic                            | FreemiumCommercialCloud-based                         | https://www.sumologic.com/Freemium/tiered pricing model Runs only as a cloud service Requires a corporate work account to signup (no Gmail or Yahoo accounts) |
> | Papertrail                            | FreemiumCommercialCloud-based                         | https://www.papertrail.com/Freemium/tiered pricing model Runs only as a cloud service |

本书Demo使用ELK、原因是

> 1. 开源、设置简单、易于使用且用户友好
> 2. 是一套完整的工具，支持搜索、分析和可视化，实时收集数据
> 3. 可以集中所有日志记录以标识服务器和应用程序问题

### 11.2.1 Spring Cloud Sleuth / ELK Stack实践

下图显示了该解决方案的最终状态

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_elk_stack.jpg" width="800" /></div>
>
> 与图中序号相对应：
>
> 1. oranization-service、licensing-service、gateway通过TCP与LogStash通信并发送日志数据
>2. LogStash过滤、转换数据，并发送给ElasticSearch作为中央数据存储
> 3. ElasticSearch对数据进行索引并以可搜索的格式存储
>4. Kibana使用索引模式从ElasticSearch检索数据，例如输入Spring Cloud Sleuth Tracing ID，可以看到该请求在各个服务上的日志

### 11.2.2 在服务中配置`logstash-logback`

> `logstash-logback`能够让服务能够发送数据给`LogStash`

#### (1) 添加`logstash-logback-encoder` Maven依赖

> ~~~xml
> <dependency>
> 	<groupId>net.logstash.logback</groupId>
> 	<artifactId>logstash-logback-encoder</artifactId>
> 	<version>6.3</version>
> </dependency>
> ~~~
>
> 代码：
>
> * [../chapter11//gatewayserver/pom.xml](../chapter11//gatewayserver/pom.xml)
> * [../chapter11//organization-service/pom.xml](../chapter11//organization-service/pom.xml)
> * [../chapter11//licensing-service/pom.xml](../chapter11//licensing-service/pom.xml)

#### (2) 创建Logstash TCP Addpender以发送日志给LogStash（并转换成JSON格式）

##### (a) 方法

Logstash Logback默认以文本方式发送日志，改为JSON格式有三种方法

> 1. 使用`net.logstash.logback.encoder.LogstashEncoder`：简单快捷（本章demo使用的方法）
> 2. 使用`net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder`：可以自定义JSON的内容和格式
> 3. 只发送默认的纯文本数据给LogStash服务器，由LogStash服务器来将其解析成JSON（通过为Logstash设置JSON filter）

##### (b) 方法1：使用`LogstashEncoder`

> ~~~xml
> <?xml version="1.0" encoding="UTF-8"?>
> <configuration>
> 	<include resource="org/springframework/boot/logging/logback/base.xml"/>
> 	<springProperty scope="context" name="application_name" source="spring.application.name"/>
> 	<!-- 指定使用LogstashTcpSocketAppender来与LogStash通信 -->
> 	<appender name="logstash" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
> 		<!-- Logstash Server的HostName和端口 -->
> 		<destination>logstash:5000</destination>
> 		<encoder class="net.logstash.logback.encoder.LogstashEncoder"/>
> 	</appender>
> 	<root level="INFO">
> 		<appender-ref ref="logstash"/>
> 		<appender-ref ref="CONSOLE"/>
> 	</root>
> 	<logger name="org.springframework" level="INFO"/>
> 	<logger name="com.optimagrowth" level="DEBUG"/>
> </configuration>
> ~~~
>
> <!--find ../chapter11 -type f | grep logback-spring.xml | grep -v target | while read p; do echo "* [$p]($p)"; done | pbcopy-->
>
> <!--find ../chapter11 -type f | grep logback-spring.xml | grep -v target | xargs md5-->
>
> 代码：
>
> * [../chapter11/gatewayserver/src/main/resources/logback-spring.xml](../chapter11/gatewayserver/src/main/resources/logback-spring.xml)
> * [../chapter11/organization-service/src/main/resources/logback-spring.xml](../chapter11/organization-service/src/main/resources/logback-spring.xml)
> * [../chapter11/licensing-service/src/main/resources/logback-spring.xml](../chapter11/licensing-service/src/main/resources/logback-spring.xml)

`LogstashTcpSocketAppender` 的输出

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_logstash_encoder.jpg" width="800" /></div>
>
> 输出内容
>
> 1. 包含存储在`Logger Mapped Diagnostic Context (MDC) `中的所有数据
> 2. 包含由`Spring Cloud Sleuth`在日志中添加的`TraceId`、`X-B3-TraceId`、`SpanId`、`X-B3-SpanId`、`spanExportable`等字段
>
> 其中`X-`前缀的字段时Spring Cloud Sleuth添加的HTTP Header，`X-B3-`代表与Zipkin有关（Zipkin的一个名为BigBrotherBird的前缀）
>
> 了解更多`MDC`（Logger Mapped Diagnostic Context）相关的内容可以参考：
>
> * SL4J Logger Document：[http://www.slf4j.org/manual.html#mdc](http://www.slf4j.org/manual.html#mdc)
> * Spring Cloud Sleuth documentation：[https://cloud.spring.io/spring-cloud-static/spring-cloud-sleuth/2.1.0.RELEASE/single/spring-cloud-sleuth.html](https://cloud.spring.io/spring-cloud-static/spring-cloud-sleuth/2.1.0.RELEASE/single/spring-cloud-sleuth.html)

##### (c) 方法2：使用`LoggingEventCompositeJsonEncoder`

> 如果想改用方法2，可将上面`logback-spring.xml`配置为LoggingEventCompositeJsonEncoder，例如：
>
> ~~~xml
> <encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">
> 	<providers>
> 		<mdc>
> 			<excludeMdcKeyName>X-B3-TraceId</excludeMdcKeyName>
> 			<excludeMdcKeyName>X-B3-SpanId</excludeMdcKeyName>
> 			<excludeMdcKeyName>X-B3-ParentSpanId</excludeMdcKeyName>
> 		</mdc>
> 		<context/><version/><logLevel/><loggerName/>
> 		<pattern>
> 			<pattern>
> 				<omitEmptyFields>true</omitEmptyFields>
> 				{
> 					"application": {
> 						version: "1.0"
> 					},
> 					"trace": {
> 						"trace_id": "%mdc{traceId}",
> 						"span_id": "%mdc{spanId}",
> 						"parent_span_id": "%mdc{X-B3-ParentSpanId}",
> 						"exportable": "%mdc{spanExportable}"
> 					}
> 				}
> 			</pattern>
> 		</pattern>
> 		<threadName/>
> 		<message/>
> 		<logstashMarkers/>
> 		<arguments/>
> 		<stackTrace/>
> 	</providers>
> </encoder>
> ~~~

### 11.2.3 在docker中定义和运行ELK stack

> 分两部分：(1) 创建Logstash配置文件（2）在docker-compose.yml中配置ELK Stack应用程序

####  (1) 创建Logstash配置文件

##### (a) Logstash配置的组成

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_logstash_elem.jpg" width="800" /></div>
>

如上图，分为三部分

`Input`：Logstash读取数据的事件源

> 支持插件例如：GitHub、Http、TCP、Kafka等

`Filter`（可选）：负责对事件进行中间处理 

> 例如：翻译、添加新信息、解析日期、截断字段等

`Output`：数据发送的目的地

> 支持插件例如：CSV、Elasticsearch、email、file、MongoDB、Redis、stdout、…… 

##### (b) Logstash配置文件编写

demo使用`Logback TCP Appender`作为输入插件、使用`ElasticSearch`作为输出目的地，配置如下

> ~~~javascript
> input {
> 	tcp {
> 		port => 5000
> 		codec => json_lines
> 	}
> }
> 
> filter {
> 	mutate {
> 		add_tag => [ "manningPublications" ]
> 	}
> }
>  
> output {
> 	elasticsearch {
> 		hosts => "elasticsearch:9200"
> 	}
> }
> ~~~
>
> 代码：[../chapter11/docker/config/logstash.conf](../chapter11/docker/config/logstash.conf)
>
> `tcp {port => 5000 codec => json_lines}`：TCP Appender插件、使用5000端口（docker-compose.yml中的LogStash的端口 ）、数据格式为JSON
>
> `mutate {add_tag => [ "manningPublications" ]}`：mutate过滤器、将manningPublications标记添加到事件中
>
> `elasticsearch {hosts => "elasticsearch:9200"}`：输出到ElasticSearch的9200端口

##### (c) Logstash插件文档

> * [https://www.elastic.co/guide/zh/logstash/current/input-plugins.html](https://www.elastic.co/guide/zh/logstash/current/input-plugins.html)
> * [https://www.elastic.co/guide/en/logstash/current/output-plugins.html](https://www.elastic.co/guide/en/logstash/current/output-plugins.html)
> * [https://www.elastic.co/guide/en/logstash/current/filter-plugins.html](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html)

#### (2) 在docker-compose.yml中配置ELK Stack应用程序

> ~~~yml
> ...
> elasticsearch:
> 	image: docker.elastic.co/elasticsearch/elasticsearch:7.7.0
> 	container_name: elasticsearch
> 	environment:
> 		- node.name=elasticsearch
> 		- discovery.type=single-node
> 		- cluster.name=docker-cluster
> 		- bootstrap.memory_lock=true
> 		- "ES_JAVA_OPTS=-Xms512m -Xmx512m"
> 	ulimits:
> 		memlock:
> 			soft: -1
> 			hard: -1
> 	volumes:
> 		- esdata1:/usr/share/elasticsearch/data
> 	ports:
> 		- 9300:9300
> 		- 9200:9200
> 	networks:
> 		backend:
> 			aliases:
> 				- "elasticsearch"
> kibana:
> 	image: docker.elastic.co/kibana/kibana:7.7.0
> 	container_name: kibana
> 	environment:
> 		ELASTICSEARCH_URL: "http://elasticsearch:9300"
> 	ports:
> 		- 5601:5601
> 	networks:
> 		backend:
> 			aliases:
> 				- "kibana"
> logstash:
> 	image: docker.elastic.co/logstash/logstash:7.7.0
> 	container_name: logstash
> 	# 让logstash启动时以与下面目录挂载一致的方式加载配置文件
> 	command: logstash -f /etc/logstash/conf.d/logstash.conf
> 	volumes:
> 		# 通过目录挂载、来让容器中的logstash能够读到上一小节编写的Logstash配置文件
> 		- ./config:/etc/logstash/conf.d
> 	ports:
> 		- "5000:5000"
> 	networks:
> 		backend:
> 			aliases:
> 				- "logstash"
> ...
> ~~~
>
> 完整代码：[../chapter11/docker/docker-compose.yml](../chapter11/docker/docker-compose.yml)

#### (3) 创建容器启动 服务

##### 启动命令

> ~~~bash
> mvn clean package dockerfile:build
> docker-compose -f docker/docker-compose.yml up
> ~~~

##### Docker内存问题解决

> 有容器以137错误码退出时，可参考以下练几天增加Docker内存
>
> [https://www.petefreitag.com/item/848.cfm](https://www.petefreitag.com/item/848.cfm)

### 11.2.4 配置Kibana

打开Kibina页面（http://localhost:5601，端口与docker-compose.yml的配置一致）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_kibana_welc_page.jpg" width="800" /></div>
>

点击“Explore on my own”、进入Kibana设置页面，标题为“Add Data to Kibana”

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_kibana_setting_page.jpg" width="800" /></div>
>

点击左上角的“Discover”图标，进入

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_kibana_index_pattern.jpg" width="800" /></div>
>

点击左边栏的“Index Pattern”，进入”Index Pattern“创建页面。"Index Pattern"用来告诉Kibana需要检索Elasticsearch的哪些索引。上图中填入`logstash-*`表示 让Kibana检索所有Logstash的数据。接着点击“Next step”

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_kibana_ts_filter.jpg" width="800" /></div>
>

选择`@timestamp`指定时间过滤器，点击“Create Index Pattern”，配置完成之后，可以看到如下的实时日志

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_elk_log_page.jpg" width="800" /></div>
>

### 11.2.5 在Kibana中搜索Spring Cloud Sleuth Trace ID

下图演示了在Kibana中使用Trace  ID来搜索日志 （Trace ID需要是一个真实的ID值）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_seach_with_tracing_id.jpg" width="800" /></div>
>

上图中每个日志时间都以展开成详细的信息列表

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_log_fields_in_elk.jpg" width="800" /></div>
>

### 11.2.6 将Trace ID添加到HTTP Response中

#### (1) 背景：`Spring Cloud Sleuth`默认不会在Response中添加Trace ID等信息

> 因为Spring Cloud Sleuth认为在Response中添加跟踪数据是潜在的安全问题，希望由用户来决定是否添加。对于微服务来说，在HTTP Response中添加Trace ID对于debug和问题分析非常重要。需要手动添加他们

#### (2) 两种方法

方法1：让Spring Cloud Sleuth开启添加这些字段的功能

> * 需要编写三个类、并注入两个自定义Spring Bean
> * 参考： [https://cloud.spring.io/spring-cloud-static/spring-cloud-sleuth/1.0.12.RELEASE/](https://cloud.spring.io/spring-cloud-static/spring-cloud-sleuth/1.0.12.RELEASE/)

方法2：使用Spring Cloud Gateway过滤器来添加这两个字段

> * 与[`ch08_spring_cloud_gateway.md`](./ch08_spring_cloud_gateway.md) 中添加correlation id的方法相同

本书Demo使用方法(b)

#### (3) 使用Spring Cloud Gateway过滤器来添加Trace ID

##### (a) 添加Maven依赖

> ~~~xml
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-starter-sleuth</artifactId>
> </dependency>
> ~~~
>
> 代码：[../chapter11/gatewayserver/pom.xml](../chapter11/gatewayserver/pom.xml)

##### (b) 编写过滤器

> ~~~java
> ...
> import brave.Tracer;
> import reactor.core.publisher.Mono;
> 
> @Configuration
> public class ResponseFilter {
> 	final Logger logger =LoggerFactory.getLogger(ResponseFilter.class);
> 
> 	// 注入sleuth的Tracer
> 	@Autowired
> 	Tracer tracer;
> 
> 	// 自己编写的用于提取HTTP Header的辅助类
> 	@Autowired
> 	FilterUtils filterUtils;
> 
> 	@Bean
> 	public GlobalFilter postGlobalFilter() {
> 		return (exchange, chain) -> {
> 			return chain.filter(exchange).then(Mono.fromRunnable(() -> {
>                 // 从上下文中提取Trace ID
> 				String traceId = tracer.currentSpan().context().traceIdString();
> 				logger.debug("Adding the correlation id to the outbound headers. {}", traceId);
>                 // 前几章是用存储在ThreadLocal或携带在HTTP Header中的UUID作为correlation id的值
> 				// 这章开始改用Sleuth的Trace ID作为correlation id的值
> 				exchange.getResponse().getHeaders().add(FilterUtils.CORRELATION_ID, traceId);
> 				logger.debug("Completing outgoing request for {}.", exchange.getRequest().getURI());
> 			}));
> 		};
> 	}
> }
> ~~~

##### (c) 测试

> URL：`http://localhost:8072/license/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c7/license/4af05c3b-a0f3-411d-b5ff-892c62710e14`
>
> 在HTTP Response中correlation id的值已经换成了Sleuth的Trace ID
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_log_query_in_kibana.jpg" width="800" /></div>
>

## 11.3 使用Open Zipkin进行分布式Tracing

> Zipkin（[http://zipkin.io/](http://zipkin.io/)）用于可视化跨不同微服务的事务流并识别其中的性能问题
>
> 设置Spring Cloud Sleuth和Zipkin的步骤如下

### 11.3.1 在gateway-server添加`Spring Cloud Sleuth`和`Zipkin`依赖

> ```xml
> <dependency> 
> 	<groupId> org.springframework.cloud </groupId> 
> 	<artifactId>spring-cloud-starter-sleuth</artifactId> 
> </dependency>
> <dependency>
> 	<groupId>org.springframework.cloud</groupId>
> 	<artifactId>spring-cloud-sleuth-zipkin</artifactId>
> </dependency>
> ```
>
> 代码：[../chapter11/gatewayserver/pom.xml](../chapter11/gatewayserver/pom.xml)
>

### 11.3.2 配置各个服务使其指向Zipkin

> 在各个服务的配置文件中，设置zipkin的url，其中`zipkin:9411`中的`zipkin`是docker-compose.yml中为zipkin设置的网络名
>
> ~~~properties
> spring.zipkin.baseUrl: zipkin:9411
> ~~~
>
> [../chapter11/configserver/src/main/resources/config/organization-service.properties](../chapter11/configserver/src/main/resources/config/organization-service.properties)
>
> [../chapter11/configserver/src/main/resources/config/licensing-service.properties](../chapter11/configserver/src/main/resources/config/licensing-service.properties)
>
> ~~~yml
> spring:
> 	zipkin:
> 		baseUrl: http://zipkin:9411
> 	sleuth:
> 		sampler:
> 			percentage: 1
> ~~~
>
> [../chapter11/configserver/src/main/resources/config/gateway-server.yml](../chapter11/configserver/src/main/resources/config/gateway-server.yml)
>
> 说明：其实也可以通过RabbitMQ或者Kafka将数据发送到Zipkin服务器、这样可以借用异步发送，来让Zipkin与服务解耦

### 11.3.3 配置Zipkin Server

> 通过在Dockerfile传入环境变量的方法来配置。Zipkin所需的配置很少，唯一必须指定的是后端数据存储，Zipkin支持四种存储，分别是：
>
> * 内存数据库
> * MuySQL
> * Cassandra
> * Elasticsearch
>
> ~~~yml
> zipkin:
> 	image: openzipkin/zipkin
> 	container_name: zipkin
> 	depends_on:
> 		- elasticsearch
> 	environment:
> 		- STORAGE_TYPE=elasticsearch
> 		- "ES_HOSTS=elasticsearch:9300"
> 	ports:
> 		- "9411:9411"
> 	networks:
> 		backend:
> 			aliases:
> 				- "zipkin"
> ~~~
>
> 代码：[../chapter11/docker/docker-compose.yml](../chapter11/docker/docker-compose.yml)

### 11.3.4 采样粒度

默认情况下仅有10%的数据会写入Zipkin，来防止数据量不会过大从而淹没Zipkin服务器

如果需要自定义该配置，可更改如下位置：

> ~~~properties
> spring.zipkin.baseUrl:http://zipkin:9411
> # 为了便于Demo观察，采样比例调高到100%
> spring.sleuth.sampler.percentage: 1 
> ~~~
>
> [../chapter11/configserver/src/main/resources/config/organization-service.properties](../chapter11/configserver/src/main/resources/config/organization-service.properties)
>
> [../chapter11/configserver/src/main/resources/config/licensing-service.properties](../chapter11/configserver/src/main/resources/config/licensing-service.properties)
>
> ~~~yml
> spring:
> 	zipkin:
> 		baseUrl: http://zipkin:9411
> 	sleuth:
> 		sampler:
> 			# 为了便于Demo观察，采样比例调高到100%
> 			percentage: 1
> ~~~
>
> [../chapter11/configserver/src/main/resources/config/gateway-server.yml](../chapter11/configserver/src/main/resources/config/gateway-server.yml)
>
> 其中`spring.sleuth.sampler.percentage`取值在0至1之间，0表示不发送，1表示全部发送

另外负责采样的类是AlwaysSampler，它以一个Spring Bean的形式注入到容器中，也可以采用替换这个默认Bean的方式来修改配置

### 11.3.5 使用Zipkin来追踪Transactinos

#### (1) 使用例子

场景：系统响应慢、需要定位知道慢在哪个服务

客户端请求：`http//localhost:8072/organization/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c6`

Zipkin界面

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_zipkin_query_filter.jpg" width="800" /></div>
>
> 可以看到Zipkin捕获了2个Transaction，两个transaction中有一个异常、花费了1.151秒
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_zipkin_transaction_time_span.jpg" width="800" /></div>
>
> 展开后可以看到这个Transaction的细节，共捕获了5个Span
>
> * gate-server2个；oragnization-service2个；licensing-service1个
> * gate-server有2个Span的原因是：要处理原始的Request；要发送新的Request到service并接收响应
>
> 单击单个Span可以查看该Span的具体HTTP请求响应相关的新消息
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_transaction_detail.jpg" width="800" /></div>

### 11.3.6 可视化复杂的Transaction内部调用关系

了解服务调用之间的依赖关系

> 以`http://localhost:8072/license/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c8/license/4af05c3b-a0f3-411d-b5ff-892c62710e15`为例，可以看到这个客户端请求共经过了哪些服务、涉及了哪些HTTP调用
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_invoking_chains_in_trx.jpg" width="800" /></div>
>

### 11.3.7 捕获消息队列的Trace信息

用途：

> 消息队列（例如Kafka等）有时会遇到网络延迟、队列拥塞、下游消费者处理性能不足等情况，通过使用Spring Cloud Sleuth和Zipkin，可以追踪消息队列上的数据收发状况

例如通过PostMan向oragnization-service发送`DELETE http://localhost:8072/organization/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c7`，分析过程如下

1 . 获取Trace  ID

> 在`11.2.6`小节中Trace ID被添加在了名为`tmx-correlation-id`的HTTP Response Header中，因此可以从Response中提取出Trace ID，值为054accff01c9ba6b

2 . 在Zipkin上使用Trace  ID查询所属的transaction

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_using_correlation_id_to_find_transaction.jpg" width="800" /></div>
>

3 . 展开transaction，可以看到属于oragnization-service的第2个Span，是向output Channel发送消息的操作

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_tracing_messages.jpg" width="800" /></div>
>

4 . 查看属于licensing-service的Span，是从inboundOrgChannel Channel接收消息的操作

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_msg_received_by_consumers.jpg" width="800" /></div>
>

### 11.3.8 添加自定义Span

用途：追踪第三方服务

例子：希望为oragnization-server添加一个自定义Span来追踪对Redis的访问

> 代码如下：
>
> ```java
> @Component
> public class OrganizationRestTemplateClient {
> 	@Autowired
> 	RestTemplate restTemplate;
> 
> 	// 注入sleuth的Tracer
> 	@Autowired
> 	Tracer tracer;
> 
> 	@Autowired
> 	OrganizationRedisRepository redisRepository;
> 
> 	private static final Logger logger = LoggerFactory.getLogger(OrganizationRestTemplateClient.class);
>     
>     ...
> 
> 	private Organization checkRedisCache(String organizationId) {
>         // Span开始 
> 		ScopedSpan newSpan = tracer.startScopedSpan("readLicensingDataFromRedis");
> 		try {
> 			return redisRepository.findById(organizationId).orElse(null);
> 		} catch (Exception ex){
> 			logger.error("Error encountered while trying to retrieve organization {} check Redis Cache.  Exception {}", organizationId, ex);
> 			return null;
> 		} finally {
>             // Span结束
> 			newSpan.tag("peer.service", "redis");
> 			newSpan.annotate("Client received");
> 			newSpan.finish();
> 		}
> 	}
> }   
> ```
>
> 代码：[../chapter11/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationRestTemplateClient.java](../chapter11/licensing-service/src/main/java/com/optimagrowth/license/service/client/OrganizationRestTemplateClient.java)

测试

> HTTP GET `http//localhost:8072/license/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c9/license/4af05c3b-a0f3-411d-b5ff-892c62710e16`
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch11_custom_spans_in_trx_tracing.jpg" width="800" /></div>
>
> 可以看到添加的自定义Span
>

## 11.4 小结

> 略