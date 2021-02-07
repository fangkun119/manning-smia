<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [CH05 使用Spring Cloud Configuration Server](#ch05-%E4%BD%BF%E7%94%A8spring-cloud-configuration-server)
  - [5.1 管理配置（和复杂性）](#51-%E7%AE%A1%E7%90%86%E9%85%8D%E7%BD%AE%E5%92%8C%E5%A4%8D%E6%9D%82%E6%80%A7)
    - [5.1.1 配置管理架构](#511-%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E6%9E%B6%E6%9E%84)
    - [5.1.2 技术选择](#512-%E6%8A%80%E6%9C%AF%E9%80%89%E6%8B%A9)
  - [5.2  构建Spring Cloud Configuration Server](#52--%E6%9E%84%E5%BB%BAspring-cloud-configuration-server)
    - [(1) 项目依赖](#1-%E9%A1%B9%E7%9B%AE%E4%BE%9D%E8%B5%96)
    - [(2) 配置](#2-%E9%85%8D%E7%BD%AE)
    - [5.2.1 启动类](#521-%E5%90%AF%E5%8A%A8%E7%B1%BB)
    - [5.2.2 本地文件作为配置存储](#522-%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6%E4%BD%9C%E4%B8%BA%E9%85%8D%E7%BD%AE%E5%AD%98%E5%82%A8)
    - [5.2.3 本地存储配置的编写和使用](#523-%E6%9C%AC%E5%9C%B0%E5%AD%98%E5%82%A8%E9%85%8D%E7%BD%AE%E7%9A%84%E7%BC%96%E5%86%99%E5%92%8C%E4%BD%BF%E7%94%A8)
  - [5.3 与Spring Boot客户端集成](#53-%E4%B8%8Espring-boot%E5%AE%A2%E6%88%B7%E7%AB%AF%E9%9B%86%E6%88%90)
    - [(1) `licensing-service`与`config-server`的交互](#1-licensing-service%E4%B8%8Econfig-server%E7%9A%84%E4%BA%A4%E4%BA%92)
    - [5.3.1 依赖](#531-%E4%BE%9D%E8%B5%96)
    - [5.3.2 服务配置](#532-%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE)
      - [(1) 配置编写](#1-%E9%85%8D%E7%BD%AE%E7%BC%96%E5%86%99)
      - [(2) 构建镜像创建容器](#2-%E6%9E%84%E5%BB%BA%E9%95%9C%E5%83%8F%E5%88%9B%E5%BB%BA%E5%AE%B9%E5%99%A8)
    - [5.3.3 Data Source配置](#533-data-source%E9%85%8D%E7%BD%AE)
    - [5.3.4 使用`@ConfigurationProperties`读取properties](#534-%E4%BD%BF%E7%94%A8configurationproperties%E8%AF%BB%E5%8F%96properties)
      - [(1) 配置存储](#1-%E9%85%8D%E7%BD%AE%E5%AD%98%E5%82%A8)
      - [(2) 配置加载](#2-%E9%85%8D%E7%BD%AE%E5%8A%A0%E8%BD%BD)
      - [(3) ConfigurationProperties处理器](#3-configurationproperties%E5%A4%84%E7%90%86%E5%99%A8)
      - [(4) 使用ConfigurationProperties](#4-%E4%BD%BF%E7%94%A8configurationproperties)
    - [5.3.5 配置刷新](#535-%E9%85%8D%E7%BD%AE%E5%88%B7%E6%96%B0)
      - [(1) 单个instance配置更新](#1-%E5%8D%95%E4%B8%AAinstance%E9%85%8D%E7%BD%AE%E6%9B%B4%E6%96%B0)
      - [(2) 多个instance批量更新](#2-%E5%A4%9A%E4%B8%AAinstance%E6%89%B9%E9%87%8F%E6%9B%B4%E6%96%B0)
    - [5.3.6 整合Git：用Git来存储配置](#536-%E6%95%B4%E5%90%88git%E7%94%A8git%E6%9D%A5%E5%AD%98%E5%82%A8%E9%85%8D%E7%BD%AE)
      - [(1) 配置编写](#1-%E9%85%8D%E7%BD%AE%E7%BC%96%E5%86%99-1)
      - [(2) 测试](#2-%E6%B5%8B%E8%AF%95)
    - [5.3.7 整合Vault](#537-%E6%95%B4%E5%90%88vault)
      - [(1) 创建Vault容器](#1-%E5%88%9B%E5%BB%BAvault%E5%AE%B9%E5%99%A8)
    - [5.3.8 使用Vault UI](#538-%E4%BD%BF%E7%94%A8vault-ui)
      - [(1) 登录Vault UI](#1-%E7%99%BB%E5%BD%95vault-ui)
      - [(2) 配置加密引擎](#2-%E9%85%8D%E7%BD%AE%E5%8A%A0%E5%AF%86%E5%BC%95%E6%93%8E)
      - [(3) 添加加密配置](#3-%E6%B7%BB%E5%8A%A0%E5%8A%A0%E5%AF%86%E9%85%8D%E7%BD%AE)
      - [(4) 配置`config-server`，将Vault作为configserver的后端存储](#4-%E9%85%8D%E7%BD%AEconfig-server%E5%B0%86vault%E4%BD%9C%E4%B8%BAconfigserver%E7%9A%84%E5%90%8E%E7%AB%AF%E5%AD%98%E5%82%A8)
      - [(5) 访问config-server进行测试](#5-%E8%AE%BF%E9%97%AEconfig-server%E8%BF%9B%E8%A1%8C%E6%B5%8B%E8%AF%95)
  - [5.4 保护敏感配置信息](#54-%E4%BF%9D%E6%8A%A4%E6%95%8F%E6%84%9F%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF)
    - [5.4.1 启用对称加密](#541-%E5%90%AF%E7%94%A8%E5%AF%B9%E7%A7%B0%E5%8A%A0%E5%AF%86)
    - [5.4.2  property加密解密](#542--property%E5%8A%A0%E5%AF%86%E8%A7%A3%E5%AF%86)
  - [5.5 思想总结](#55-%E6%80%9D%E6%83%B3%E6%80%BB%E7%BB%93)
  - [5.6 小结](#56-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# CH05 使用Spring Cloud Configuration Server

> 使用YAML、JSON、XML配置的方法适用于少量应用程序，但是在处理数百个微服务的基于云的应用程序时，会迅速瓦解。为避免这种状况，最佳实践应该是
>
> 1. 将配置与代码分离 
> 2. 构建不可变镜像
> 3. 服务启动时通过环境变量或者读取配置存储来注入配置信息
>
> 本章节内容
>
> 1. 如何将服务配置与代码分离
> 2. 部署Spring Cloud配置服务器并与Spring Boot集成
> 3. 加密敏感属性
> 4. 将Spring Cloud配置服务器与Hashicorp Vault集成
>
> 原书章节在线阅读：[https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-5/v-8/](https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-5/v-8/)

## 5.1 管理配置（和复杂性）

微服务配置管理的的四个原则

> 1. `分离（Segregate）`：配置信息与服务在物理部署上完全分开
> 2. `抽象化（Abstract）`：服务不直接访问配置存储、而是使用REST来检索配置数据
> 3. `集中（Centralize） `：最小化保存配置信息的存储库数量，将配置集中在尽可能小的存储库中，以避免配置分散在数百个服务中难以管理
> 4. `健壮（Harden）`：配置服务需要高可用并且有冗余备份

服务与配置的分开也带来了一个新的外部依赖，需要对其进行管理和版本控制

### 5.1.1 配置管理架构

>微服务的启动会经理如下生命周期，在服务引导（Bootstrapping）阶段会读取应用程序配置
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch05_cfg_in_lifecycle.jpg" width="800" /></div>
>
>下图是服务引导时配置加载的过程
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch05_cfg_arch.jpg" width="800" /></div>
>
>如图中的标注，4个环节分别是：
>
>1. 实例启动时，访问`Configuration Management Service`服务端点的连接信息（连接凭据、服务端点等）会传给该实例，实例通过访问服务端点来读取属于它的配置
>2. 实际配置保存在存储库中，实现可以是：(1) 具有版本控制的文件（2）关系型数据库（3）key-value数据库
>3. 配置变更通过`build and deployment pipeline`来推送到配置存储中，配置信息带有版本标记，并支持不同的运行环境（development、staging、production、……)
>4. 配置变更需要通知到运行中的Service，让他们重新加载配置

### 5.1.2 技术选择

有很多经过实践检验的开源技术，下表列举其中一部分

>| Project name                      | Description                                                  | Characteristics                                              |
>| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
>| Etcd                              | 用GO编写，用于服务发现和key-value管理，使用[raft](https://raft.github.io/)协议作为分布式计算模型 | 计算速度快，可扩展，命令行驱动，易于使用和配置               |
>| Eureka                            | Netflix的技术，久经考验，支持服务发现和key-value管理         | 分布式key-value存储，灵活但需要一定的配置，开箱即用的客户端动态刷新功能 |
>| Consul                            | Hashicorp的技术，功能类似Etcd和Eureka，但分布式计算模型使用不同的算法 | 快速，提供"native service discovery"，可选直接与DNS集成，不直接提供客户端动态刷新功能（需要借助Spring Cloud之类的其他技术） |
>| ZooKeeper                         | 分布式锁，Apache项目，通常用作key-value配置管理的解决方案    | 最古老、最久经考验的解决方案；使用方法最复杂；是否使用取决于项目特点，通常是跟随整个大项目的整体方案统一使用 |
>| Spring Cloud configuration server | 提供不同后端常规配置管理的解决方案                           | 非分布式key-value存储；与Spring及Non-Spring服务紧密集成；可以使用多个后端存储配置数据；包括共享文件系统（Eureka, Consul, ，Git） |

本节使用`Spring Cloud Configuration  Server`，将其与Spring Boot集成，演示三种配置管理机制：(1) 使用文件系统；(2) 使用Git存储；(3) 使用Hashicorp Vault

## 5.2  构建Spring Cloud Configuration Server

### (1) 项目依赖

>Spring Clould Configuration Server是基于Spring Boot的REST应用程序，同样使用[Spring Initializer](https://start.spring.io/)（通过官网或者IDE）来创建
>
>* 本书使用Java 11
>* 依赖项为Config Server和Spring Boot Actuator
>
>完整依赖见：[pom.xml](../chapter5/pom.xml)，[configserver/pom.xml](../chapter5/configserver/pom.xml)
>
>以下是对[`configserver/pom.xml`](../chapter5/configserver/pom.xml)的说明
>
>~~~xml
><?xml version="1.0" encoding="UTF-8"?>
><project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>		 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
>	<modelVersion>4.0.0</modelVersion>
>	<parent>
>		<groupId>org.springframework.boot</groupId>
>		<artifactId>spring-boot-starter-parent</artifactId>
> 		<!-- (1) Spring Boot版本 -->
>		<version>2.2.4.RELEASE</version>
>		<relativePath/> <!-- 从仓库查找父POM而非本地 -->
>	</parent>
>
>	<properties>
>		<java.version>11</java.version>
>		<docker.image.prefix>ostock</docker.image.prefix>
>		<!-- (2) 选择Spring Cloud的版本 -->
>		<spring-cloud.version>Hoxton.SR1</spring-cloud.version>
>	</properties>
>
> 	<!-- (3) 依赖 -->
>	<dependencies>
>		<dependency>
>			<groupId>org.springframework.cloud</groupId>
>			<artifactId>spring-cloud-config-server</artifactId>
>		</dependency>
>		<dependency>
>			<groupId>org.springframework.boot</groupId>
>			<artifactId>spring-boot-starter-actuator</artifactId>
>		</dependency>
>		<dependency>
>			<groupId>org.springframework.boot</groupId>
>			<artifactId>spring-boot-starter-test</artifactId>
>			<scope>test</scope>
>			<exclusions>
>				<exclusion>
>					<groupId>org.junit.vintage</groupId>
>					<artifactId>junit-vintage-engine</artifactId>
>				</exclusion>
>			</exclusions>
>		</dependency>
>	</dependencies>
>
> 	<!-- (4) Spring Cloud BOM（Bill Of Meterials）definition -->
> 	<!--     包含使用的与当前Spring cloud版本兼容的第三方库和依赖 -->
> 	<!--     版本号定义在上面注释为(2)的位置，本例是Hoxton.SR1 -->
>	<dependencyManagement>
>		<dependencies>
>			<dependency>
>				<groupId>org.springframework.cloud</groupId>
>				<artifactId>spring-cloud-dependencies</artifactId>
>				<version>${spring-cloud.version}</version>
>				<type>pom</type>
>				<scope>import</scope>
>			</dependency>
>		</dependencies>
>	</dependencyManagement>
>
>	...
>~~~

### (2) 配置

`Spring Cloud Configuration Server`的配置文件

> * 可以是application.properties、application.yml、bootstrap.properties或bootstrap.yml
> * 可配置的内容包含：该server的名称，存储配置的git location，加密解密配置等

本例的配置文件：

> * [/configserver/src/main/resources/bootstrap.yml](../chapter5/configserver/src/main/resources/bootstrap.yml)
> * [/configserver/src/main/resources/config/](../chapter5/configserver/src/main/resources/config/)

具体内容后续介绍

### 5.2.1 启动类 

>[/configserver/src/main/java/.../configserver/ConfigurationServerApplication.java](../chapter5/configserver/src/main/java/com/optimagrowth/configserver/ConfigurationServerApplication.java)
>
>~~~java
>@SpringBootApplication
>@EnableConfigServer //用做Spring Cloud Configure Server
>public class ConfigurationServerApplication {
>	public static void main(String[] args) {
>		SpringApplication.run(ConfigurationServerApplication.class, args);
>	}
>}
>~~~
>
>使用`@EnableConfigServer`注解来表示该Spring Boot Application用作Spring  Cloud Configure Server。其它类型的Spring Cloud组件也都有对应的注解

### 5.2.2 本地文件作为配置存储

>如果使用本地文件系统存储配置，代码（[/configserver/src/main/resources/bootstrap.yml](../chapter5/configserver/src/main/resources/bootstrap.yml)）如下
>
>~~~yml
>spring:
>  	application:
>    		name: config-server
>  	profiles:
>    		active: native
>  	cloud:
>    		config:
>     			server:
>     				# 使用本地文件系统存储: This locations can either of classpath or locations in the filesystem.
>     				native:
>     					# 可以从文件系统的目录中读取配置
>     					# search-locations: file:///{FILE_PATH}
>     					# 可以从classpath中读取配置
>     					search-locations: classpath:/config
>server:
>  	port: 8071
>~~~
>
>在`search-location`中配置多个目录时，可以使用逗号分隔

### 5.2.3 本地存储配置的编写和使用

> `5.2.2`小节将Spring Cloud Configureation Server的配置存储设为本地文件，路径是`classpath:/config`

下面是这个路径下为`licensing-service`存储的数据，包括 (1) 业务配置 (2) Actuator配置 (3) 数据库配置

命名方式是：`${appname}-${env}.properties`或者`${appname}-${env}.yml`

>* 默认配置：[/configserver/src/main/resources/config/licensing-service.properties](../chapter5/configserver/src/main/resources/config/licensing-service.properties)
>* 开发环境配置：[/configserver/src/main/resources/config/licensing-service-dev.properties](../chapter5/configserver/src/main/resources/config/licensing-service-dev.properties)
>* 生产环境配置：[/configserver/src/main/resources/config/licensing-service-prod.properties](../chapter5/configserver/src/main/resources/config/licensing-service-prod.properties)

访问`config-server`的REST端点可以获得对应的配置

> * `/licensing-service/dev`（开发环境配置）：[licensing-service.properties](../chapter5/configserver/src/main/resources/config/licensing-service.properties) + [licensing-service-dev.properties](../chapter5/configserver/src/main/resources/config/licensing-service-dev.properties)
> * `/licensing-service/prod`（生产环境配置）：[licensing-service.properties](../chapter5/configserver/src/main/resources/config/licensing-service.properties) + [licensing-service-prod.properties](../chapter5/configserver/src/main/resources/config/licensing-service-prod.properties)
> * `/licensing-service/default`（默认配置 ）：[licensing-service.properties](../chapter5/configserver/src/main/resources/config/licensing-service.properties)

而`licensing-service`具体访问哪个端点，取决于它的`spring.profiles.active`配置

> ```bash
> java  -Dspring.cloud.config.uri=http://localhost:8071 \
>       -Dspring.profiles.active=dev \
>       -jar target/licensing-service-0.0.1-SNAPSHOT.jar
> ```
>
> * 如果`spring.profiles.active`为`dev`则访问`/licensing-service/dev`
> * 如果`spring.profiles.active`为`prod`则访问`/licensing-service/prod`
> * 如果未指定则 访问`/licensing-service/default`

例子：`/licensing-service/default`返回[licensing-service.properties](../chapter5/configserver/src/main/resources/config/licensing-service.properties)中的配置 

>~~~bash
>__________________________________________________________________
>$ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/tmp/
>$ curl http://localhost:8071/licensing-service/default
>{
>    	"name": "licensing-service",
>    	"profiles": ["default"],
>     	"label": null,
>    	"version": null,
>    	"state": null,
>    	"propertySources": [{
>    		"name": "classpath:/config/licensing-service.properties",
>    		"source": {
>     			"example.property": "I AM THE DEFAULT",
>     			"spring.jpa.hibernate.ddl-auto": "none",
>     			"spring.jpa.database": "POSTGRESQL",
>     			"spring.datasource.platform": "postgres",
>     			"spring.jpa.show-sql": "true",
>     			"spring.jpa.hibernate.naming-strategy": "org.hibernate.cfg.ImprovedNamingStrategy",
>     			"spring.jpa.properties.hibernate.dialect": "org.hibernate.dialect.PostgreSQLDialect",
>     			"spring.database.driverClassName": "org.postgresql.Driver",
>     			"spring.datasource.testWhileIdle": "true",
>     			"spring.datasource.validationQuery": "SELECT 1",
>     			"management.endpoints.web.exposure.include": "*",
>     			"management.endpoints.enabled-by-default": "true"
>     		}
>     	}]
>     }
>     ~~~

例子：`/licensing-service/dev`返回[licensing-service.properties](../chapter5/configserver/src/main/resources/config/licensing-service.properties)以及[licensing-service-dev.properties](../chapter5/configserver/src/main/resources/config/licensing-service-dev.properties)的配置，这样`licensing-service`可以载入默认配置，然后对于在dev配置中存在的属性，用dev配置进行覆盖

>~~~bash
>_____________________________________________________
>$ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/tmp/
>$ curl http://localhost:8071/licensing-service/dev
>{
>    	"name": "licensing-service",
>    	"profiles": ["dev"],
>     	"label": null,
>    	"version": null,
>    	"state": null,
>    	"propertySources": [{
>    		"name": "classpath:/config/licensing-service-dev.properties",
>    		"source": {
>     			"example.property": "I AM DEV",
>     			"spring.datasource.url": "jdbc:postgresql://database:5432/ostock_dev",
>     			"spring.datasource.username": "postgres",
>     			"spring.datasource.password": "postgres"
>     		}
>     	},{
>     		"name": "classpath:/config/licensing-service.properties",
>     		"source": {
>     			"example.property": "I AM THE DEFAULT",
>     			"spring.jpa.hibernate.ddl-auto": "none",
>     			"spring.jpa.database": "POSTGRESQL",
>     			"spring.datasource.platform": "postgres",
>     			"spring.jpa.show-sql": "true",
>     			"spring.jpa.hibernate.naming-strategy": "org.hibernate.cfg.ImprovedNamingStrategy",
>     			"spring.jpa.properties.hibernate.dialect": "org.hibernate.dialect.PostgreSQLDialect",
>     			"spring.database.driverClassName": "org.postgresql.Driver",
>     			"spring.datasource.testWhileIdle": "true",
>     			"spring.datasource.validationQuery": "SELECT 1",
>     			"management.endpoints.web.exposure.include": "*",
>     			"management.endpoints.enabled-by-default": "true"
>     		}
>     	}]
>     }
>     ~~~

基于文件的配置存储，不适合大中型云应用程序，这意味着要在云中维护一套支持数量庞大的应用程序的共享文件系统。后续展示基于GitHub和 Hashicorp Vault的配置存储

##  5.3 与Spring Boot客户端集成

`licensing-service`作为`config-server`的客户端

>以Demo项目中的`licensing-service`（许可证服务）作为Spring Boot客户端来访问Configuration Service。在这一小章中，`licensing-service`将使用PostgreSQL来存储业务数据。

PostgreSQL的特点

> (1) 支持MVCC（多版本并发控制）、事务处理性能更高（2）事务执行时不适用读锁（3）支持Hot-Standby即执行维护时无需完全锁定数据库（4）支持视图

### (1) `licensing-service`与`config-server`的交互

><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch05_boot_retrieve_cfg_with_dev_profile.jpg" width="800" /></div>
>

交互过程如上图 ，对应图中标记的数字，共4个环节

1 . 启动`licensing-service`载入关于如何连接`config-server`的配置

2 . `licensing-service`根据其中的`spring.profiles.active`的值决定访问`config-server`哪个REST端点

3 . `config-server`根据REST端点返回具体的配置

4 . `licensing-service`根据`config-server`的response来进行配置

### 5.3.1 依赖

>代码：[/licensing-service/pom.xml](../chapter5/licensing-service/pom.xml)
>
>因为新增了与`spring-cloud-starter-config`的交互以及`postgresql`存储，相比上一章的demo，新增了如下3个依赖
>
>~~~xml
>...
><dependency>
>	<groupId>org.springframework.cloud</groupId>
>	<artifactId>spring-cloud-starter-config</artifactId>
></dependency>
><dependency>
>	<groupId>org.springframework.boot</groupId>
>	<artifactId>spring-boot-starter-data-jpa</artifactId>
></dependency>
><dependency>
>	<groupId>org.postgresql</groupId>
>	<artifactId>postgresql</artifactId>
></dependency>
>...
>~~~

### 5.3.2 服务配置

#### (1) 配置编写

配置文件：[/licensing-service/src/main/resources/bootstrap.yml](../chapter5/licensing-service/src/main/resources/bootstrap.yml)

>~~~yml
># licensing-service与configserver通信所使用的配置
>spring:
>  	application:
>    		name: licensing-service  # 应用程序名字
>  	profiles:
>    		active: dev	# 默认配置为激活dev环境
>  	cloud:
>    		config: 
>     			# (1) docker-compose会使用“configserver”作为配置服务的服务名
>     			#     在编排好的容器中，可以把该服务名用作hostname来访问配置服务
>     			# (2) http://configserver:8071是base_url
>     			# (3) 完整的url是：${base_url}/${application-name}/${profiles-active}
>     			uri: http://configserver:8071 # config-server的uri
>~~~

如果不指定profile来运行（例如`mvn spring-boot:run`)，会使用上面的文件加载dev环境的配置

如果指定profile，则根据profile来获取配置，例如下面的例子，加载prod环境的配置

> ~~~bash
> # 命令行参数会覆盖yml中的配置
> java -Dspring.cloud.config.uri=http://localhost:8071 \
>    	-Dspring.profiles.active=dev \
>    	-jar target/licensing-service-0.0.1-SNAPSHOT.jar
> ~~~

也可以通过设置环境变量来覆盖yml中的配置

> 例如可以在docker-compose.yml中为licensingservice设置如下环境变量 
>
> ~~~yml
> licensingservice:
>    	image: ostock/licensing-service:0.0.1-SNAPSHOT
>    	ports:
> 		- "8080:8080"
> 	environment:
> 		SPRING_PROFILES_ACTIVE: "dev"
> 		SPRING_CLOUD_CONFIG_URI: http://configserver:8071
> ~~~

这些配置的覆盖关系，参考[Spring Configuration Properties](https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch06_configuration.md)

#### (2) 构建镜像创建容器

Step 1：生成`configserver`和`licensing-service`的jar包

> ~~~bash
> mvn clean package
> ~~~

Step 2：docker镜像

> 启动Dorker（mac上的docker desktop，或者linux上的service docker start）
>
> 创建docker镜像，两个Dockerfile所用变量都是从docker-maven-plugin传入（[/configserver/pom.xml](../chapter5/configserver/pom.xml)，[licensing-service/pom.xml](../chapter5/licensing-service/pom.xml)），因此使用docker-maven-plugin来创建docker镜像
>
> ~~~bash
> mvn dockerfile:build # 需要关闭VPN以解除对80端口的拦截
> ~~~
>
> 查看构建好的镜像
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/
> $ docker images
> REPOSITORY                     TAG              IMAGE ID       CREATED             SIZE
> ostock/licensing-service       0.0.2-SNAPSHOT   44a971d34cc5   About an hour ago   469MB
> <none>                         <none>           197eca4d6c2e   About an hour ago   517MB
> ostock/configserver            0.0.1-SNAPSHOT   c111b7ee08ac   About an hour ago   451MB
> <none>                         <none>           b010e16a9a4e   About an hour ago   482MB
> ~~~

Step 3：创建并编排容器

> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter5/docker/
> $ docker-compose up -d
> Starting docker_configserver_1 ... done
> Starting docker_database_1     ... done
> Starting docker_licensingservice_1 ... done
> ~~~
>
> 相关命令：`docker-compose up -d`，`docker-compose logs`，`docker-compose logs ${service_id}`，`docker-compose ps`，`docker-compose stop`，`docker-compose down`，……
>
> 参考：[https://github.com/fangkun119/java_proj_ref/blob/master/101_docker/note/05_docker_compose.md#2-%E5%88%9B%E5%BB%BA%E5%B9%B6%E7%BC%96%E6%8E%92%E5%AE%B9%E5%99%A8](https://github.com/fangkun119/java_proj_ref/blob/master/101_docker/note/05_docker_compose.md#2-%E5%88%9B%E5%BB%BA%E5%B9%B6%E7%BC%96%E6%8E%92%E5%AE%B9%E5%99%A8)

Step  4：在浏览器中访问`http://localhost:8080/actuator/env`可以查看`licensing-service`从`configserver`获得的配置

> ~~~json
> {
>    	"activeProfiles": ["dev"],
>    	"propertySources": [{
>    		"name": "server.ports",
>    		"properties": {
>    			"local.server.port": {
>    				"value": 8080
>    			}
>    		}
>    	},{
>    		"name": "bootstrapProperties-classpath:/config/licensing-service-dev.properties",
>    		"properties": {
>    			"spring.datasource.username": {
>    				"value": "postgres"
>    			},			"spring.datasource.url": {
>    				"value": "jdbc:postgresql://database:5432/ostock_dev"
>    			},
>    			"example.property": {
>    				"value": "I AM DEV"
>    			},
>    			"spring.datasource.password": {
>    				"value": "******"
>    			}
>    		}
>    	},{
>    		"name": "bootstrapProperties-classpath:/config/licensing-service.properties",
>    		"properties": {
>    			"management.endpoints.web.exposure.include": {
>    				"value": "*"
>    			},
>    			"spring.jpa.properties.hibernate.dialect": {
>    				"value": "org.hibernate.dialect.PostgreSQLDialect"
>    			},
>    			"spring.jpa.database": {
>    				"value": "POSTGRESQL"
>    			},
>    			"spring.datasource.testWhileIdle": {
>    				"value": "true"
>    			},
>    			"spring.database.driverClassName": {
>    				"value": "org.postgresql.Driver"
>    			},
>    			"spring.jpa.hibernate.naming-strategy": {
>    				"value": "org.hibernate.cfg.ImprovedNamingStrategy"
>    			},
>    			"example.property": {
>    				"value": "I AM THE DEFAULT"
>    			},
>    			"spring.datasource.validationQuery": {
>    				"value": "SELECT 1"
>    			},
>    			"spring.datasource.platform": {
>    				"value": "postgres"
>    			},
>    			"management.endpoints.enabled-by-default": {
>    				"value": "true"
>    			},
>    			"spring.jpa.hibernate.ddl-auto": {
>    				"value": "none"
>    			},
>    			"spring.jpa.show-sql": {
>    				"value": "true"
>    			}
>    		}
>    	},
>    	...
>    	]
>    }
>    ~~~

### 5.3.3 Data Source配置

与单机程序使用`properties`或者`yml`配置（参考 [Spring Configuration Properties](https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch06_configuration.md)）相同。使用`configserver`后 ，配置会注入到使用它们的地方。

下面代码访问数据库与普通的SpringBoot程序相同

>增加3个类，用来使用数据库存储License数据，代码讲解见原书
>
>(1) [/licensing-service/src/main/java/com/optimagrowth/license/model/License.java](../chapter5/licensing-service/src/main/java/com/optimagrowth/license/model/License.java)
>
>使用lambok编写的entity类
>
>(2) [/licensing-service/src/main/java/com/optimagrowth/license/repository/LicenseRepository.java](../chapter5/licensing-service/src/main/java/com/optimagrowth/license/repository/LicenseRepository.java)
>
>使用Spring Data JPA编写
>
>(3) [/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter5/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)
>
>封装了业务逻辑，供controller使用

代码说明见原书章节，Spring Boot参考如下

> [https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch02_web_app.md](https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch02_web_app.md)
>
> [https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch03_spring_data.md](https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch03_spring_data.md)

###  5.3.4 使用`@ConfigurationProperties`读取properties

下面的代码以配置项`example.property`为例，演示了如何编写自定义的`Configuration Properties` ，并注入到程序中

#### (1) 配置存储

> 用dev环境为例，配置编写在`configserver的存储`中，本例为：[/configserver/src/main/resources/config/licensing-service-dev.properties](../chapter5/configserver/src/main/resources/config/licensing-service-dev.properties)
>
> ~~~properties
> example.property= I AM DEV
> ~~~
>
> 除了配置文件，configserver，来源还可以是环境变量，命令行参数等

#### (2) 配置加载

> 5.3.2小节容器创建后通过`http://localhost:8080/actuator/env`可以看到已加载的配置
>
> ~~~json
> "example.property": {
> 	"value": "I AM DEV"
> },
> ~~~

#### (3) ConfigurationProperties处理器

> 用来处理上面的配置
>
> [/licensing-service/src/main/java/com/optimagrowth/license/config/ServiceConfig.java](../chapter5/licensing-service/src/main/java/com/optimagrowth/license/config/ServiceConfig.java)
>
> ```java
> @Configuration
> @ConfigurationProperties(prefix = "example") // (1) example
> @Getter @Setter 
> public class ServiceConfig{
> 	private String property; // (2) property 
> }
> ```
>
> 将前缀"example"和变量名"property"拼接在一起，就可以找到"example.property"的值 

#### (4) 使用ConfigurationProperties

> [/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter5/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)
>
> ~~~java
> @Service
> public class LicenseService {
> 	...
> 	
> 	@Autowired
> 	ServiceConfig config; //注入ConfigurationProperties处理器
> 
>    	...
> 
> 	public License updateLicense(License license){
> 		licenseRepository.save(license);
> 		return license.withComment(config.getProperty()); // 读取配置
> 	}
> 
>    	...
> }
> ~~~

更多内容参考：[https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch06_configuration.md](https://github.com/fangkun119/spring-in-action-6-samples/blob/main/note/ch06_configuration.md)

### 5.3.5 配置刷新

>任何时候访问`configservice`，都可以拿到最新的配置；但是作为普通Spring Boot Application的`licensingService`默认只在程序启动时加载配置

#### (1) 单个instance配置更新

使用`@RefreshScope`注解，能够通过访问`licensingServiced`的`/refresh`端点来触发它的配置更新

>需要注意的是
>
>* `@RefreshScope`只会自动刷新`自定义的Configuration Properties`
>* Data Source等Spring框架使用的Configuration Properties不会被自动刷新，
>
>~~~java
>@SpringBootApplication
>@RefreshScope // 来自Spring Boot Actuator，会提供一个/refresh端点用于配置更新
>public class LicenseServiceApplication {
>	public static void main(String[] args) {
>		SpringApplication.run(LicenseServiceApplication.class, args);
>	}
>}
>~~~

#### (2) 多个instance批量更新

方法1：使用Spring Cloud Bus推送机制

> 需要RabbitMQ之类的中间件，有些服务不支持（例如Consul）

方法2：查询Service Discovery获得所有instance并调用他们的`/refresh`端点

> 下一章介绍

方法3：重启所有instance

> 在云环境中，重启instance通常非常快

### 5.3.6 整合Git：用Git来存储配置

#### (1) 配置编写

>修改`configserver`的[`bootstrap.yml`](../chapter5/configserver/src/main/resources/bootstrap.yml)如下
>
>~~~yml
>spring:
>  	application:
>    		name: config-server
>  	profiles:
>    		active:
> 		- native, git
> 	cloud:
> 		config:
> 			server:
> 				native:
> 					search-locations: classpath:/config
> 				git:
> 					uri: https://github.com/ihuaylupo/config.git
> 					searchPaths: licensingservice
>...
>~~~
>
>`- native,git`：配置存储方式，逗号分隔，后面的优先级大于前面的
>
>`earchPaths: licensingservice`：服务列表（也是在git repo中查找的子目录列表），逗号分隔

#### (2) 测试

>启动整套环境后，访问http://localhost:8080/actuator/env可以发现，配置的名称已经由`bootstrapProperties-classpath:/config/licensing-service-dev.properties`变成了`bootstrapProperties-https://github.com/ihuaylupo/config.git/licensing-service-dev.properties`
>
>~~~json
>{
>    	"name": "bootstrapProperties-https://github.com/ihuaylupo/config.git/licensing-service-dev.properties",
>    	"properties": {
>     		"spring.datasource.username": {
>     			"value": "postgres"
>     		},
>     		"security.oauth2.resource.userInfoUri": {
>     			"value": "http://authenticationservice:8082/user"
>     		},
>     		"spring.datasource.url": {
>     			"value": "jdbc:postgresql://database:5432/ostock_dev?sslmode=disable"
>     		},
>     		"example.property": {
>     			"value": "I AM DEV"
>     		},
>     		"spring.datasource.password": {
>     			"value": "******"
>     		}
>    	}
>},{
>	"name": "bootstrapProperties-https://github.com/ihuaylupo/config.git/licensing-service.properties",
>    	"properties": {
>    		"management.endpoints.web.exposure.include": {
>     			"value": "*"
>     		},
>     		"spring.cloud.stream.bindings.inboundOrgChanges.group": {
>     			"value": "licensingGroup"
>     		},
>     		"logstash.host": {
>     			"value": "3.136.161.26:5000"
>     		},
>     		...
>	}
>    }
>~~~

### 5.3.7 整合Vault

>`Hashicorp Value`是一个工具，可用作`configserver`的后端存储，支持加密访问，适合密码、证书、API秘钥等配置信息

#### (1) 创建Vault容器

> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter5/docker/
> $ docker pull vault
> Using default tag: latest
> latest: Pulling from library/vault
> 21c83c524219: Pull complete
> 67dfcdbc8a1a: Pull complete
> d5dd4c26996b: Pull complete
> 0b63ae1409e8: Pull complete
> 028736f26ea0: Pull complete
> Digest: sha256:6281c2d63ac61769dd8cdd553ec8a92547ca6ed58252dac9a0a7a664ffb4289a
> Status: Downloaded newer image for vault:latest
> docker.io/library/vault:latest
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter5/docker/
> $ docker run -d -p 8200:8200 --name vault -e 'VAULT_DEV_ROOT_TOKEN_ID=myroot' -e 'VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:8200' vault
> 2480694c8c7d4148a713381025d38af7f88365d6e7211aa212f52d1d39f758d9
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter5/docker/
> $ docker ps
> CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                    NAMES
> 8a7caf637ce6   vault     "docker-entrypoint.s…"   5 seconds ago   Up 4 seconds   0.0.0.0:8200->8200/tcp   vault
> ~~~

`docker run -d -p 8200:8200 --name vault -e 'VAULT_DEV_ROOT_TOKEN_ID=myroot' -e 'VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:8200' vault`

> * `VAULT_DEV_ROOT_TOKEN_ID`：用于生成`root token`（用于配置vault的初始token）的ID
> * `VAULT_DEV_LISTEN_ADDRESS`: development server的IP和端口

更多内容参考：[https://hub.docker.com/_/vault](https://hub.docker.com/_/vault)

###  5.3.8 使用Vault UI

#### (1) 登录Vault UI 

>用浏览器访问development server（`http://0.0.0.0:8200/ui/vault/auth?with=token`，即`VAULT_DEV_LISTEN_ADDRESS`配置的地址），在`Token输入框`中输入`VAULT_DEV_ROOT_TOKEN_ID`配置的值
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch05_vault_login.jpg" width="800" /></div>
>

#### (2) 配置加密引擎 

>对应图中的标注
>
>1. 点击`Secrets`标签进入加密信息Dashboard
>
>2. 点击`Enable new engine`创建加密引擎，进入加密引擎创建向导
>3. 在`加密配置类型选择页`中选择`Generic:KV”`，点击`Next`
>4. 在`路径版本选择页`中
>    1. 路径填写”licensing-service"（给`licensing-service`使用）
>    2. 版本填写2（默认值是1，使用Vault 0.10.0及以上版本时建议选择2）
>    3. 点击`Enable Engine`
>5. 点击`Create secret`进入加密配置创建页面
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch05_vault_ui_secret.jpg" width="800" /></div>
>

#### (3) 添加加密配置

>在名为"license.vault.property"值为“Welcome to Vault”的配置项，该配置项所属的profile为"default"
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch05_vault_ui_secret_2.jpg" width="800" /></div>
>

#### (4) 配置`config-server`，将Vault作为configserver的后端存储

> 修改configserver配置文件中（[/configserver/src/main/resources/bootstrap.yml](../chapter5/configserver/src/main/resources/bootstrap.yml)）可以把vault用作存储后端
>
> ~~~yml
> spring:
> 	application:
> 		name: config-server
> 	profiles:
>     	active:
> 		- vault
> 	cloud:
> 		config:
> 			server:
> 				# vault配置，与之前填写的内容保持一致
> 				vault:
> 					port: 8200
> 					host: 127.0.0.1
> 					kvVersion: 2
> server:
> 	port: 8071
> ~~~

#### (5) 访问config-server进行测试

> 测试命令
>
> ~~~bash
> $ curl -X "GET" "http://localhost:8071/licensing-service/default" -H "X-Config-Token: myroot"
> ~~~
>
> 返回值应当是
>
> ~~~json
> {
> 	"name": "licensing-service",
> 	"profiles": [
> 		"default"
> 	],
> 	"label": null,
> 	"version": null,
> 	"state": null,
> 	"propertySources": [{
> 		"name": "vault:licensing-service",
> 		"source": {
> 			"license.vault.property": "Welcome to vault"
> 		}
> 	}]
> }
> ~~~

## 5.4 保护敏感配置信息

>默认情况下`config-server`以纯文本的形式存储配置信息，为了增强安全性，Spring Cloud支持使用对称加密（共享秘钥）和非对称加密（公钥/私钥）
>

###  5.4.1 启用对称加密

>对称加密是为加密和解密使用同一个秘钥 ，开启方法如下
>
>配置项`encrypt.key`（[/configserver/src/main/resources/bootstrap.yml](../chapter5/configserver/src/main/resources/bootstrap.yml)）
>
>~~~yml
>encrypt:
>	key: fje83Ki8403Iod87dne7Yjsl3THueh48jfuO9j4U2hf64Lo
>~~~
>
>等同于环境变量的`ENCRYPT_KEY`（[/docker/docker-compose.yml](../chapter5/docker/docker-compose.yml)）
>
>~~~yml
>configserver:
>	image: ostock/configserver:0.0.1-SNAPSHOT
>	ports:
>		- "8071:8071"
>	# 处于演示目的，实际应用中不应当硬编码在代码、配置或者Docker相关的文件中
>	environment:
>		ENCRYPT_KEY: "fje83Ki8403Iod87dne7Yjsl3THueh48jfuO9j4U2hf64Lo"
>	...
>~~~

###  5.4.2  property加密解密

>上一小节设置`ENCRYPT_KEY`之后，会为服务增加两个新端点`/crypten`和`decrypt`用来对数据加解密
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch05_encrypt.jpg" width="800" /></div>
>
>向加密端点POST密码明文，可以得到密码加密后的字符串 ，将其添加在`config-server`的数据存储中。
>
>以dev环境的密码为例：[/configserver/src/main/resources/config/licensing-service-dev.properties](../chapter5/configserver/src/main/resources/config/licensing-service-dev.properties)
>
>~~~properties
>example.property= I AM DEV
>
># DataSource settings: set here your own configurations for the database 
>spring.datasource.url = jdbc:postgresql://database:5432/ostock_dev
>spring.datasource.username = postgres
>spring.datasource.password = {cipher}f4609209a3e75d8ac79a5e3063ce151c2cd28aa431170bb06974b9421e807b6a
>~~~
>
>Spring Cloud Config Server要求所有的加密属性都以`{cipher}`为前缀，以表示他们是加密数据

## 5.5 思想总结

>关于使用Config Server的重要性
>

## 5.6 小结

> 略