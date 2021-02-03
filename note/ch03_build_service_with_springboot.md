<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [CH03 使用Spring Boot构建微服务](#ch03-%E4%BD%BF%E7%94%A8spring-boot%E6%9E%84%E5%BB%BA%E5%BE%AE%E6%9C%8D%E5%8A%A1)
  - [3.1 架构视角：设计微服务架构](#31-%E6%9E%B6%E6%9E%84%E8%A7%86%E8%A7%92%E8%AE%BE%E8%AE%A1%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84)
    - [3.1.1 业务问题拆解过程](#311-%E4%B8%9A%E5%8A%A1%E9%97%AE%E9%A2%98%E6%8B%86%E8%A7%A3%E8%BF%87%E7%A8%8B)
    - [3.1.2 把握服务拆分粒度](#312-%E6%8A%8A%E6%8F%A1%E6%9C%8D%E5%8A%A1%E6%8B%86%E5%88%86%E7%B2%92%E5%BA%A6)
      - [(1) 目标](#1-%E7%9B%AE%E6%A0%87)
      - [(2) 拆分方法](#2-%E6%8B%86%E5%88%86%E6%96%B9%E6%B3%95)
      - [(3) 微服务设计的“Bad Smell”](#3-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%AE%BE%E8%AE%A1%E7%9A%84bad-smell)
        - [拆分粒度太粗时的表征](#%E6%8B%86%E5%88%86%E7%B2%92%E5%BA%A6%E5%A4%AA%E7%B2%97%E6%97%B6%E7%9A%84%E8%A1%A8%E5%BE%81)
        - [拆分粒度太细时的表现](#%E6%8B%86%E5%88%86%E7%B2%92%E5%BA%A6%E5%A4%AA%E7%BB%86%E6%97%B6%E7%9A%84%E8%A1%A8%E7%8E%B0)
    - [3.1.3 定义服务接口](#313-%E5%AE%9A%E4%B9%89%E6%9C%8D%E5%8A%A1%E6%8E%A5%E5%8F%A3)
      - [(1) 目标](#1-%E7%9B%AE%E6%A0%87-1)
      - [(2) 方法](#2-%E6%96%B9%E6%B3%95)
  - [3.2 何时不适合使用微服务](#32-%E4%BD%95%E6%97%B6%E4%B8%8D%E9%80%82%E5%90%88%E4%BD%BF%E7%94%A8%E5%BE%AE%E6%9C%8D%E5%8A%A1)
    - [3.2.1 构建分布式系统带来的复杂度](#321-%E6%9E%84%E5%BB%BA%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F%E5%B8%A6%E6%9D%A5%E7%9A%84%E5%A4%8D%E6%9D%82%E5%BA%A6)
    - [3.2.2 容器数量（Server Sprawl）](#322-%E5%AE%B9%E5%99%A8%E6%95%B0%E9%87%8Fserver-sprawl)
    - [3.2.3 应用类型](#323-%E5%BA%94%E7%94%A8%E7%B1%BB%E5%9E%8B)
    - [3.2.4 数据转换和一致性](#324-%E6%95%B0%E6%8D%AE%E8%BD%AC%E6%8D%A2%E5%92%8C%E4%B8%80%E8%87%B4%E6%80%A7)
  - [3.3 开发视角：使用Spring Boot构建服务](#33-%E5%BC%80%E5%8F%91%E8%A7%86%E8%A7%92%E4%BD%BF%E7%94%A8spring-boot%E6%9E%84%E5%BB%BA%E6%9C%8D%E5%8A%A1)
    - [3.3.1 Controller](#331-controller)
      - [(1) 代码](#1-%E4%BB%A3%E7%A0%81)
      - [(2) REST Endpoint命名](#2-rest-endpoint%E5%91%BD%E5%90%8D)
      - [(3) 运行](#3-%E8%BF%90%E8%A1%8C)
      - [(4) 访问REST服务端点](#4-%E8%AE%BF%E9%97%AErest%E6%9C%8D%E5%8A%A1%E7%AB%AF%E7%82%B9)
    - [3.3.2 国际化](#332-%E5%9B%BD%E9%99%85%E5%8C%96)
      - [(1) 启用`国际化`](#1-%E5%90%AF%E7%94%A8%E5%9B%BD%E9%99%85%E5%8C%96)
      - [(2) 国际化Message Source配置](#2-%E5%9B%BD%E9%99%85%E5%8C%96message-source%E9%85%8D%E7%BD%AE)
      - [(3) 使用Message Source](#3-%E4%BD%BF%E7%94%A8message-source)
        - [从Controller获取用户的语言环境](#%E4%BB%8Econtroller%E8%8E%B7%E5%8F%96%E7%94%A8%E6%88%B7%E7%9A%84%E8%AF%AD%E8%A8%80%E7%8E%AF%E5%A2%83)
        - [根据Locale查找属于该语言环境的数据](#%E6%A0%B9%E6%8D%AElocale%E6%9F%A5%E6%89%BE%E5%B1%9E%E4%BA%8E%E8%AF%A5%E8%AF%AD%E8%A8%80%E7%8E%AF%E5%A2%83%E7%9A%84%E6%95%B0%E6%8D%AE)
      - [(4) 访问服务端点：](#4-%E8%AE%BF%E9%97%AE%E6%9C%8D%E5%8A%A1%E7%AB%AF%E7%82%B9)
    - [3.3.3 使用HATEOAS返回相关API的链接](#333-%E4%BD%BF%E7%94%A8hateoas%E8%BF%94%E5%9B%9E%E7%9B%B8%E5%85%B3api%E7%9A%84%E9%93%BE%E6%8E%A5)
      - [(1) 功能](#1-%E5%8A%9F%E8%83%BD)
      - [(2) 添加依赖](#2-%E6%B7%BB%E5%8A%A0%E4%BE%9D%E8%B5%96)
      - [(3) 让Entity类继承基类`RepresentationModel <License>`，以便有能力添加链接](#3-%E8%AE%A9entity%E7%B1%BB%E7%BB%A7%E6%89%BF%E5%9F%BA%E7%B1%BBrepresentationmodel-license%E4%BB%A5%E4%BE%BF%E6%9C%89%E8%83%BD%E5%8A%9B%E6%B7%BB%E5%8A%A0%E9%93%BE%E6%8E%A5)
      - [(4) 为某个API添加`相关API URL`](#4-%E4%B8%BA%E6%9F%90%E4%B8%AAapi%E6%B7%BB%E5%8A%A0%E7%9B%B8%E5%85%B3api-url)
  - [3.4 DevOPs视角：构建严苛的运行时环境](#34-devops%E8%A7%86%E8%A7%92%E6%9E%84%E5%BB%BA%E4%B8%A5%E8%8B%9B%E7%9A%84%E8%BF%90%E8%A1%8C%E6%97%B6%E7%8E%AF%E5%A2%83)
    - [(1) 微服务部署的四个原则](#1-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E9%83%A8%E7%BD%B2%E7%9A%84%E5%9B%9B%E4%B8%AA%E5%8E%9F%E5%88%99)
    - [(2) 微服务部署步骤](#2-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E9%83%A8%E7%BD%B2%E6%AD%A5%E9%AA%A4)
    - [3.4.1 打包和部署](#341-%E6%89%93%E5%8C%85%E5%92%8C%E9%83%A8%E7%BD%B2)
    - [3.4.2 服务引导（bootstrapping)和配置管理](#342-%E6%9C%8D%E5%8A%A1%E5%BC%95%E5%AF%BCbootstrapping%E5%92%8C%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86)
    - [3.4.3 服务注册和发现](#343-%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E5%92%8C%E5%8F%91%E7%8E%B0)
    - [3.4.4 健康检查](#344-%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5)
      - [(1) 服务发现代理的健康检查和故障路由绕行（route out）](#1-%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0%E4%BB%A3%E7%90%86%E7%9A%84%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5%E5%92%8C%E6%95%85%E9%9A%9C%E8%B7%AF%E7%94%B1%E7%BB%95%E8%A1%8Croute-out)
      - [(2) 构建运行状况检查接口](#2-%E6%9E%84%E5%BB%BA%E8%BF%90%E8%A1%8C%E7%8A%B6%E5%86%B5%E6%A3%80%E6%9F%A5%E6%8E%A5%E5%8F%A3)
        - [(a)  依赖](#a--%E4%BE%9D%E8%B5%96)
        - [(b) 修改默认配置（可选）](#b-%E4%BF%AE%E6%94%B9%E9%BB%98%E8%AE%A4%E9%85%8D%E7%BD%AE%E5%8F%AF%E9%80%89)
        - [(c) 监控端点列表](#c-%E7%9B%91%E6%8E%A7%E7%AB%AF%E7%82%B9%E5%88%97%E8%A1%A8)
        - [(d) 演示](#d-%E6%BC%94%E7%A4%BA)
  - [3.5 观点整合](#35-%E8%A7%82%E7%82%B9%E6%95%B4%E5%90%88)
  - [3.6 小结](#36-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# CH03 使用Spring Boot构建微服务

> 本章内容涵盖：
>
> * 微服务如何适应云架构
> * 将业务领域分解为一组微服务，构建基于微服务的应用程序
> * 何时不使用微服务
> * 使用Spring Boot，Spring Actuator，Spring HATEOAS和国际化类库来实现微服务
>
> 构建微服务要考虑到各种参与角色，本章重点从其中的三类进行讲解：(1) 架构师（2）开发人员（3）DevOps工程师

## 3.1 架构视角：设计微服务架构

> 目标：提供解决问题的工作模型
>
> 专注于三个关键任务：（1）分解业务问题（2）划定服务粒度（3）确定服务接口

### 3.1.1 业务问题拆解过程

面对复杂性：将复杂问题分解为可管理独立单元

>对微服务来说，意味着将业务问题分解为代表`"离散活动域（特定的业务规则和数据逻辑）"`的块

识别和拆解业务问题

>1. 描述业务问题、并注意其围绕的数据域：例如本书例子中的`合同`、`软件许可证`、`资产`
>2. 描述业务流程、并注意其中的动作轮廓：例如本书例子中`初始化新PC时会查找许可证数量，如果有许可证则安装软件并更新许可证数量`
>3. 寻找数据“内聚力”（cohesion）：例如忽然发现大量数据、与目前已知的“高内聚数据域”完全不同，意味着很可能需要从这些新数据中抽像出新的微服务

下图是与本书项目中、与需求部门讨论后提炼出的业务流程

><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_interview.jpg" width="600" /></div>

根据讨论结果，将项目拆解为以下数据域

> - 合同信息相关（Emma）
> - 许可证信息（许可证存储、成本管理、许可证类型，许可证所有者、许可证合同）相关
> - 资产信息（PC）相关，需要在PC上设置许可证（Jenny）
> - 组织信息相关（许可证属于多个组织）

其数据模型简化如下

>
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_datamodel.jpg" width="400" /></div>

### 3.1.2 把握服务拆分粒度

#### (1) 目标

上一节将问题拆解为`资产`、`许可证`、`合同`、`组织`四个数据域，对应着四个潜在的微服务

><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_microservices_arch.jpg" width="800" /></div>

本节的目标是将其提取到完全独立的单元中

>* 代码打包在彼此独立的项目中
>* 每个服务也仅允许访问特定域中的表

#### (2) 拆分方法

难点在于确定微服务分解的粒度，太粗糙或太细都会带来问题，方法建议如下

(1) 从`较粗粒度的初始方案`开始、在分析过程中将其重构为`较细粒度的拆解方案`

> 与之相反以细粒度拆解为起始，容易产生过度拆解、同时各个细小微服务沦落为细粒度的数据服务

(2) 首先关注在服务之间的交互上

> 有助于构建出反映问题域业务逻辑的服务力度，同时粗粒度服务比细粒度服务相对容易重构

(3) 随着业务需求的增加，对问题域的了解也不断增长，微服务会承担更多职责

> 最初的单个微服务可能会扩展成多个服务
>
> 而原始的微服务则充当这些新服务的编排层，并封装这些新拆分服务的功能

以”进化“的思维过程来开发，因为在第一时间完全预测出未来的业务变化不切实际，这也是以较粗粒度的拆分作为起始，逐渐像细粒度进化的原因；同时也存在需要将两个细粒度微服务合并的可能。

#### (3) 微服务设计的“Bad Smell”

##### 拆分粒度太粗时的表征

> * `职责过多的服务`：流程复杂、强制（enforcing）塞入过量发散（overly diverse）的业务规则
> * `数据表越界`：微服务管理着过量的数据表，或者开始使用其他问题域的表，意味着该服务可能承担太多责任
> * `测试用例数量庞杂`：随着服务规模和责任的增加，测试用例数量明显超出经验合理值（例如一个微服务拥有数百个单元和集成测试），成为该服务可能过度复杂需要拆分的一个指征

##### 拆分粒度太细时的表现

> * 应用程序拥有数十个微服务，而每个微服务仅与一个数据表进行交互
> * 微服务之间高度依赖，同一个问题域中的一部分微服务一直在彼此来回调用以完成单个用户请求
> * 微服务成为简单的CRUE服务集合

### 3.1.3 定义服务接口

#### (1) 目标

>微服务接口应当是直观的，使服务接口更易于理解和使用
>
>开发人员可以通过学习1、2个服务来掌握这个服务如何在整个系统中工作

#### (2) 方法

> * 拥抱REST哲学
> * 使用URI传达意图
> * 相比XML更建议使用JSON来进行请求和响应
> * 借助HTTP丰富的状态代码传达结果、并在所有服务中一致地使用他们

## 3.2 何时不适合使用微服务

>从以下四个方面考虑：（1）构建分布式系统带来的复杂度（2）容器数量（3）应用类型（4）数据转换和一致性

### 3.2.1 构建分布式系统带来的复杂度

>将系统分布式化会带来一定程度的复杂度，需要高成熟度的自动化运维（自动化监控、扩展(scaling)等），要考虑是否愿意进行这部分的投入

### 3.2.2 容器数量（Server Sprawl）

>考虑拆分服务带来的容器数量的增加，以及与之相关的容器内程序管理和监控工作

### 3.2.3 应用类型

>微服务面向可重用性，偏向于使用在高度弹性和可扩展的大型系统上

### 3.2.4 数据转换和一致性

>考虑整个应用的`整体数据使用模式`
>
>* 微服务是一种更适合执行“operational”类型任务的机制，例如执行创建、新增、以及一些低复杂度的查询操作
>* 当应用需要跨多个数据源进行复杂的数据聚合、转换时，微服务的分布式特性使其变得困难，同时单个服务承担过多职责也更容易出现性能问题

## 3.3 开发视角：使用Spring Boot构建服务

> 内容包含（1）Controller（2）国际化（3）使用Spring HATEOAS为用户提供用于与服务器交互的信息
>
> 项目位置：[../chapter3/licensing-service/](../chapter3/licensing-service/)

### 3.3.1 Controller

#### (1) 代码

Entity：[/src/main/java/com/optimagrowth/license/model/License.java](../chapter3/licensing-service/src/main/java/com/optimagrowth/license/model/License.java)

> 包含id、licenseId、description、organizationId、productName、licenseType六个字段
>
> 其中用到的lambok参考
>
> * [https://www.baeldung.com/intro-to-project-lombok](https://www.baeldung.com/intro-to-project-lombok)
> * [https://www.baeldung.com/lombok-ide](https://www.baeldung.com/lombok-ide)

Service：[/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter3/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)

Controller： [/src/main/java/com/optimagrowth/license/controller/LicenseController.java](../chapter3/licensing-service/src/main/java/com/optimagrowth/license/controller/LicenseController.java)

>~~~java
>// 用于REST服务
>@RestController 
>// 根URL，其中{organizationId}可以用@PathVariable("orgnizationID")来提取
>@RequestMapping(value="v1/organization/{organizationId}/license")
>public class LicenseController {
>	// 注入service层的类对象
>	@Autowired
>	private LicenseService licenseService;
>
>	// 查询，以下两种注解方式等价
>	// @RequestMapping(value="/{licenseId}",method = RequestMethod.GET)
>	@GetMapping(value="/{licenseId}")
>	public ResponseEntity<License> getLicense(
>			@PathVariable("organizationId") String organizationId, 
>			@PathVariable("licenseId") String licenseId) {
>		License license = licenseService.getLicense(licenseId, organizationId);
>		license.add( 
>				linkTo(methodOn(LicenseController.class).getLicense(organizationId, license.getLicenseId())).withSelfRel(),
>				linkTo(methodOn(LicenseController.class).createLicense(organizationId, license, null)).withRel("createLicense"),
>				linkTo(methodOn(LicenseController.class).updateLicense(organizationId, license)).withRel("updateLicense"),
>				linkTo(methodOn(LicenseController.class).deleteLicense(organizationId, license.getLicenseId())).withRel("deleteLicense")
>		);
>		// HTTP Response
>		// 状态码：200（OK）
>		// Body：License对象
>		return ResponseEntity.ok(license);
>	}
>
>	// 修改
>	@PutMapping
>	public ResponseEntity<String> updateLicense(
>			// 参数来自URL路径
>			@PathVariable("organizationId") String organizationId, 
>			// 参数来自HTTP Request Body
>			@RequestBody License request) {
>		return ResponseEntity.ok(licenseService.updateLicense(request, organizationId));
>	}
>
>	// 增加
>	@PostMapping
>	public ResponseEntity<String> createLicense(@PathVariable("organizationId") String organizationId, @RequestBody License request,
>			@RequestHeader(value = "Accept-Language",required = false) Locale locale) {
>		return ResponseEntity.ok(licenseService.createLicense(request, organizationId, locale));
>	}
>
>	// 删除
>	@DeleteMapping(value="/{licenseId}")
>	public ResponseEntity<String> deleteLicense(@PathVariable("organizationId") String organizationId, @PathVariable("licenseId") String licenseId) {
>		return ResponseEntity.ok(licenseService.deleteLicense(licenseId, organizationId));
>	}
>}
>~~~

#### (2) REST Endpoint命名

> 1. 使用清晰的`URL名称`来代表服务提供的`资源`
> 2. URL能够表示资源之间的父子关系
> 3. 尽早建立URL的版本控制方案

#### (3) 运行

>~~~bash
>mvn spring-boot:run
>~~~
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_service_starting.jpg" width="800" /></div>
>

#### (4) 访问REST服务端点

>使用POSTMAN或CURL
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_get_del.jpg" width="600" /></div>
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_post_put.jpg" width="600" /></div>

### 3.3.2 国际化

>开发提供多种格式和语言内容的应用程序

#### (1) 启用`国际化`

> 需要装配`LocaleResolver`和`ResouceBundleMessageSource`两个Bean
>
> 代码：[/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java](../chapter3/licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java)
>
> ```java
> @Bean
> public LocaleResolver localeResolver() {
> 	SessionLocaleResolver localeResolver = new SessionLocaleResolver();
> 	// Request未指定要求的语言时，使用默认值Local.US 
> 	localeResolver.setDefaultLocale(Locale.US); 
> 	return localeResolver;
> }
> 
> @Bean
> public ResourceBundleMessageSource messageSource() {
> 	ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
> 	// 当找不到所需要的消息时，返回Message Code而非抛出异常
> 	messageSource.setUseCodeAsDefaultMessage(true);
> 	// 消息源的base_name
> 	// 当它设为“messages“时、对应消息源为/src/main/resources目录的以下文件
> 	// messages_en.properties	：Local.EN的消息源
> 	// messages_es.properties	：Local.ES的消息源
> 	// ...
> 	// messages.properties		：默认消息源
> 	messageSource.setBasenames("messages");
> 	return messageSource;
> }
> ```

#### (2) 国际化Message Source配置

> [/src/main/resources/messages_en.properties](../chapter3/licensing-service/src/main/resources/messages_en.properties)
>
> ~~~properties
> license.create.message = License created %s
> license.update.message = License %s updated
> license.delete.message = Deleting license with id %s for the organization %s
> ~~~
>
> [/src/main/resources/messages_es.properties](../chapter3/licensing-service/src/main/resources/messages_es.properties)
>
> ~~~properties
> license.create.message = Licencia creada %s
> license.update.message = Licencia %s creada
> license.delete.message = Eliminando licencia con id %s para la organization %s license
> ~~~

#### (3) 使用Message Source

#####  从Controller获取用户的语言环境

> 代码： [/src/main/java/com/optimagrowth/license/controller/LicenseController.java](../chapter3/licensing-service/src/main/java/com/optimagrowth/license/controller/LicenseController.java)
>
> ```java
> @PostMapping
> public ResponseEntity<String> createLicense(
>     	@PathVariable("organizationId") String organizationId, @RequestBody License request,
> 		// 语言环境来自HTTP Header的"Accept-Language"字段
> 		// 如果HTTP Header未指定该字段，将使用默认的语言环境
> 		@RequestHeader(value = "Accept-Language", required = false) Locale locale) {
> 	return ResponseEntity.ok(licenseService.createLicense(request, organizationId, locale));
> }
> ```

##### 根据Locale查找属于该语言环境的数据

> 代码：[/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java](../chapter3/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java)
>
> ```java
> @Autowired
> MessageSource messages;
> 
> public String createLicense(License license, String organizationId, Locale locale/*语言环境*/) {
> 	String responseMessage = null;
> 	if (!StringUtils.isEmpty(license)) {
> 		license.setOrganizationId(organizationId);
> 		responseMessage = String.format(
>             // 获取语言环境中配置的字符串
>             // messages_es.properties：license.create.message = Licencia creada %s
>             // messages_en.properties：license.create.message = License created %s
>             // 当第3个参数locale为null时，将使用默认环境中的字符串
> 			messages.getMessage("license.create.message", null, locale), 
> 			license.toString()
>         );
> 	}
> 	return responseMessage;
> }
> ```

#### (4) 访问服务端点：

>设为置HTTP Header的`Accept-Language`字段为`es`
> 
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_accept_lang_header.jpg" width="600" /></div>

### 3.3.3 使用HATEOAS返回相关API的链接

#### (1) 功能

> HATEOAS是超媒体应用状态引擎，作用是让API在每个Response中返回每个Resource相关的API URL，为客户端提供指南。HATEOAS不是核心功能，也不是必须功能。但用来获取与某个资源相关的API列表，是个不错的选择，例如：
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/tmp/
> $  curl http://localhost:8080/v1/organization/1/license/api
> {
>     "id": 474,
>     "licenseId": "api",
>     "description": "Software product",
>     "organizationId": "1",
>     "productName": "Ostock",
>     "licenseType": "full",
>     "_links": {
>         "self": {
>             "href": "http://localhost:8080/v1/organization/1/license/api"
>         },
>         "createLicense": {
>             "href": "http://localhost:8080/v1/organization/1/license"
>         },
>         "updateLicense": {
>             "href": "http://localhost:8080/v1/organization/1/license"
>         },
>         "deleteLicense": {
>             "href": "http://localhost:8080/v1/organization/1/license/api"
>         }
>     }
> }
> ~~~
>
> 也可以为某个API的Response添加相关API URL，例如下面`第(4)小节`的例子

#### (2) 添加依赖

>~~~xml
><dependency>
>	<groupId>org.springframework.boot</groupId>
>	<artifactId>spring-boot-starter-hateoas</artifactId>
></dependency>
>~~~

#### (3) 让Entity类继承基类`RepresentationModel <License>`，以便有能力添加链接

>代码：[/src/main/java/com/optimagrowth/license/model/License.java](../chapter3/licensing-service/src/main/java/com/optimagrowth/license/model/License.java)
>
>~~~java
>... 
>import org.springframework.hateoas.RepresentationModel;
> 
>@Getter @Setter @ToString
>public class License extends RepresentationModel<License> {
>      private int id;
>      private String licenseId;
>      private String description;
>      private String organizationId;
>      private String productName;
>      private String licenseType;
>}
>~~~

#### (4) 为某个API添加`相关API URL`

> ```java
> @RestController
> @RequestMapping(value = "v1/organization/{organizationId}/license")
> public class LicenseController {
> 	... 
> 	@RequestMapping(value = "/{licenseId}", method = RequestMethod.GET)
> 	public ResponseEntity<License> getLicense(
> 			@PathVariable("organizationId") String organizationId,
> 			@PathVariable("licenseId") String licenseId) {
> 		License license = licenseService.getLicense(licenseId, organizationId);
> 		license.add(
>             // methodOn(...)	：通过虚拟调用获得某个方法的路径
> 			// linkTo(...)		：生成URL
>             // withRel(...)		：生成打印在Response中的Relation Name
> 			linkTo(methodOn(LicenseController.class).getLicense(organizationId, license.getLicenseId())).withSelfRel(),
> 			linkTo(methodOn(LicenseController.class).createLicense(organizationId, license, null)).withRel("createLicense"),
> 			linkTo(methodOn(LicenseController.class).updateLicense(organizationId, license)).withRel("updateLicense"),
> 			linkTo(methodOn(LicenseController.class).deleteLicense(organizationId, license.getLicenseId())).withRel("deleteLicense")
> 		);
> 		return ResponseEntity.ok(license);
> 	}
>     ...
> }
> ```
>
> 测试
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/tmp/
> $ curl http://localhost:8080/v1/organization/1/license/65793w6-2sdfw-124qw
> {
>     "id": 414,
>     "licenseId": "65793w6-2sdfw-124qw",
>     "description": "Software product",
>     "organizationId": "1",
>     "productName": "Ostock",
>     "licenseType": "full",
>     "_links": {
>         "self": {
>             "href": "http://localhost:8080/v1/organization/1/license/65793w6-2sdfw-124qw"
>         },
>         "createLicense": {
>             "href": "http://localhost:8080/v1/organization/1/license"
>         },
>         "updateLicense": {
>             "href": "http://localhost:8080/v1/organization/1/license"
>         },
>         "deleteLicense": {
>             "href": "http://localhost:8080/v1/organization/1/license/65793w6-2sdfw-124qw"
>         }
>     }
> }
> ~~~

## 3.4 DevOPs视角：构建严苛的运行时环境

### (1) 微服务部署的四个原则

> 1. `自包含`且`独立部署`：通过1个artifact可以启动和拆除多个实例
> 2. `可配置`：使用中央配置存储或通过环境变量传递，无需人工干预即
> 3. `对客户端透明`：客户端不应当知道服务的确切位置、而是通过服务发现代理来定位服务实例
> 4. `能传达其健康状况`：服务发现代理可以绕过不良服务实例进行路由，本书使用`Spring Boot Actuator`来实现

### (2) 微服务部署步骤

>上面的四个原则，可以映射为微服务部署生命周期中的四个步骤
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_start_steps.jpg" width="800" /></div>
>
>1. `服务组装`：打包和部署，确保在所有instances上部署相同的Runtime 
>2. `服务自举（bootstrapping)`：将应用程序与（专属于各个环境的）配置分开，以便在任何环境中快速启动和部署（而无需人工柑橘）
>3. `服务注册/发现`：其他应用程序客户端可以发现新部署的服务实例
>4. `服务监控`：满足高可用性需求，监控微服务实例，微服务路由可以绕过故障的instance，并确保故障的服务实例得以清除

### 3.4.1 打包和部署

>
>将程序、所有的依赖、运行时引擎（例如tomcat）打包在一个Artifact中，以便能够快速部署多个实例
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_assembly_steps.jpg" width="600" /></div>
>
>Maven和Spring Boot已经提供了这样的功能，可以将上述内容打包成一个可执行的Jar文件
>
>~~~bash
>mvn clean package && java -jar target/licensing-service-0.0.1-SNAPSHOT.jar
>~~~

### 3.4.2 服务引导（bootstrapping)和配置管理

><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch02_conf_repo.jpg" width="600" /></div>
>
>如上图所示，为了确保大量的微服务实例，能够使用相同配置，需要将配置数据存储在服务外部
>
>配置存储需要满足
>
>* 使用适合频繁读、不频繁写，支持简单数据的存储（关系型数据库过于重量级不合适）
>* 低延迟，高可用

### 3.4.3 服务注册和发现

>为了能够将微服务Instance视为短期的一次性对象，可以批量启动和删除，不需要永久IP，需要使用`服务注册和发现`
>
>(1) 服务注册
>
>* 微服务实例向服务发现代理注册、告知（1）自己的IP Address或Domain Address（2）用于查找该服务的`逻辑名`
>* 某些服务发现代理还需要返回注册服务的URL，以执行运行状况检查
>
>(2) 服务发现：客户端与发现代理进行通信以查找服务的位置
>
><div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_service_discovery.jpg" width="800" /></div>

### 3.4.4 健康检查

#### (1) 服务发现代理的健康检查和故障路由绕行（route out）

>服务发现代理通过访问实例的健康接口、持续监控实例运行状况，从路由表中剔除异常实例、启动新实例等，来保证实例异常不会影响客户端
> 
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch03_health_check.jpg" width="800" /></div>
> 

#### (2) 构建运行状况检查接口

> 最简单的方法是提供可以返回JSON和HTTP Status Code的HTTP端点。Spring Boot简化了开发工作、过程如下 。
>
> Demo项目位置：[../chapter3/licensing-service/](../chapter3/licensing-service/)

##### (a)  依赖

> [/pom.xml](../chapter3/licensing-service/pom.xml)
>
> ~~~xml
> <dependency>
> 	<groupId>org.springframework.boot</groupId>
> 	<artifactId>spring-boot-starter-actuator</artifactId>
> </dependency>
> ~~~

##### (b) 修改默认配置（可选）

> [/src/main/resources/application.properties](../chapter3/licensing-service/src/main/resources/application.properties)
>
> ~~~bash
> # 设置actuator services的base path
> # 默认值为 http://${hostname}:${port}/actuator/health
> # 下面的设置将其改为 http://localhost:8080/health
> management.endpoints.web.base-path=/
> 
> # 禁止或启用某些服务
> management.endpoints.enabled-by-default=false
> management.endpoint.health.enabled=true
> management.endpoint.health.show-details=always
> management.health.db.enabled=false
> management.health.diskspace.enabled=true
> ~~~

##### (c) 监控端点列表

> [https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-endpoints.html](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-endpoints.html)

##### (d) 演示

>~~~bash
>$ curl http://localhost:8080/actuator/health
>{
>    "status": "UP",
>    "components": {
>        "diskSpace": {
>            "status": "UP",
>            "details": {
>                "total": 250685575168,
>                "free": 26827116544,
>                "threshold": 10485760
>            }
>        },
>        "ping": {
>            "status": "UP"
>        }
>    }
>}
>~~~

## 3.5 观点整合

> 从架构师、Developer、DevOPs的视角来看到微服务

## 3.6 小结

> 略 

