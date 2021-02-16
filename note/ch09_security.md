<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [CH9 身份验证及访问管理](#ch9-%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E5%8F%8A%E8%AE%BF%E9%97%AE%E7%AE%A1%E7%90%86)
  - [9.1 OAuth2](#91-oauth2)
  - [9.2 KeyCloak](#92-keycloak)
  - [9.3 Keycloak的搭建和配置](#93-keycloak%E7%9A%84%E6%90%AD%E5%BB%BA%E5%92%8C%E9%85%8D%E7%BD%AE)
    - [9.3.1 在Docker中添加Keycloak服务](#931-%E5%9C%A8docker%E4%B8%AD%E6%B7%BB%E5%8A%A0keycloak%E6%9C%8D%E5%8A%A1)
      - [(1) 在docker-compose.yml中添加Keycloak](#1-%E5%9C%A8docker-composeyml%E4%B8%AD%E6%B7%BB%E5%8A%A0keycloak)
      - [(2) 端口修改](#2-%E7%AB%AF%E5%8F%A3%E4%BF%AE%E6%94%B9)
    - [9.3.2 设置KeyCloak](#932-%E8%AE%BE%E7%BD%AEkeycloak)
      - [(1) 创建容器](#1-%E5%88%9B%E5%BB%BA%E5%AE%B9%E5%99%A8)
      - [(2) 配置KeyCloak](#2-%E9%85%8D%E7%BD%AEkeycloak)
        - [登录`KeyCloak Console`](#%E7%99%BB%E5%BD%95keycloak-console)
        - [创建`Realm`](#%E5%88%9B%E5%BB%BArealm)
    - [9.3.3 注册客户端应用程序](#933-%E6%B3%A8%E5%86%8C%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F)
      - [(1) 添加客户端](#1-%E6%B7%BB%E5%8A%A0%E5%AE%A2%E6%88%B7%E7%AB%AF)
      - [(2) 为客户端添加角色](#2-%E4%B8%BA%E5%AE%A2%E6%88%B7%E7%AB%AF%E6%B7%BB%E5%8A%A0%E8%A7%92%E8%89%B2)
      - [(3) 生成客户端凭据（`Credentials`）](#3-%E7%94%9F%E6%88%90%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%87%AD%E6%8D%AEcredentials)
      - [(4) 创建Realm角色](#4-%E5%88%9B%E5%BB%BArealm%E8%A7%92%E8%89%B2)
  - [9.3.4 配置Ostock用户](#934-%E9%85%8D%E7%BD%AEostock%E7%94%A8%E6%88%B7)
    - [9.3.5 验证用户](#935-%E9%AA%8C%E8%AF%81%E7%94%A8%E6%88%B7)
      - [(1) 用来获取access_token的End Point](#1-%E7%94%A8%E6%9D%A5%E8%8E%B7%E5%8F%96access_token%E7%9A%84end-point)
      - [(2) 用PostMan模拟客户端获取access token](#2-%E7%94%A8postman%E6%A8%A1%E6%8B%9F%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%8E%B7%E5%8F%96access-token)
      - [(3) 使用https://jwt.io来解码access token](#3-%E4%BD%BF%E7%94%A8httpsjwtio%E6%9D%A5%E8%A7%A3%E7%A0%81access-token)
  - [9.4 使用KeyCloak保护organization-service](#94-%E4%BD%BF%E7%94%A8keycloak%E4%BF%9D%E6%8A%A4organization-service)
    - [9.4.1 添加`Spring Security`和`KeyCloak`依赖项](#941-%E6%B7%BB%E5%8A%A0spring-security%E5%92%8Ckeycloak%E4%BE%9D%E8%B5%96%E9%A1%B9)
    - [9.4.2 配置服务以指向KeyCloak服务器](#942-%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1%E4%BB%A5%E6%8C%87%E5%90%91keycloak%E6%9C%8D%E5%8A%A1%E5%99%A8)
    - [9.4.3 定义服务访问规则](#943-%E5%AE%9A%E4%B9%89%E6%9C%8D%E5%8A%A1%E8%AE%BF%E9%97%AE%E8%A7%84%E5%88%99)
      - [(1) 只容许授权的用户访问](#1-%E5%8F%AA%E5%AE%B9%E8%AE%B8%E6%8E%88%E6%9D%83%E7%9A%84%E7%94%A8%E6%88%B7%E8%AE%BF%E9%97%AE)
        - [(a) 配置](#a-%E9%85%8D%E7%BD%AE)
        - [(b) 测试](#b-%E6%B5%8B%E8%AF%95)
      - [(2) 只容许符合特定角色的用户访问](#2-%E5%8F%AA%E5%AE%B9%E8%AE%B8%E7%AC%A6%E5%90%88%E7%89%B9%E5%AE%9A%E8%A7%92%E8%89%B2%E7%9A%84%E7%94%A8%E6%88%B7%E8%AE%BF%E9%97%AE)
    - [9.4.4 传播访问令牌](#944-%E4%BC%A0%E6%92%AD%E8%AE%BF%E9%97%AE%E4%BB%A4%E7%89%8C)
      - [(1) 访问令牌传播和验证过程](#1-%E8%AE%BF%E9%97%AE%E4%BB%A4%E7%89%8C%E4%BC%A0%E6%92%AD%E5%92%8C%E9%AA%8C%E8%AF%81%E8%BF%87%E7%A8%8B)
      - [(2) 代码修改](#2-%E4%BB%A3%E7%A0%81%E4%BF%AE%E6%94%B9)
        - [(a) gateway-server路由转发Header配置](#a-gateway-server%E8%B7%AF%E7%94%B1%E8%BD%AC%E5%8F%91header%E9%85%8D%E7%BD%AE)
        - [(b) licensing-service](#b-licensing-service)
          - [添加keyCloak依赖](#%E6%B7%BB%E5%8A%A0keycloak%E4%BE%9D%E8%B5%96)
          - [配置服务以指向KeyCloak服务器](#%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1%E4%BB%A5%E6%8C%87%E5%90%91keycloak%E6%9C%8D%E5%8A%A1%E5%99%A8)
          - [定义服务访问规则，定义可以传播Authorization HTTP Header的KeycloakRestTemplate](#%E5%AE%9A%E4%B9%89%E6%9C%8D%E5%8A%A1%E8%AE%BF%E9%97%AE%E8%A7%84%E5%88%99%E5%AE%9A%E4%B9%89%E5%8F%AF%E4%BB%A5%E4%BC%A0%E6%92%ADauthorization-http-header%E7%9A%84keycloakresttemplate)
          - [使用`KeycloakRestTemplate`发送请求并传播access token](#%E4%BD%BF%E7%94%A8keycloakresttemplate%E5%8F%91%E9%80%81%E8%AF%B7%E6%B1%82%E5%B9%B6%E4%BC%A0%E6%92%ADaccess-token)
    - [9.4.5 解析JWT中填写的自定义信息](#945-%E8%A7%A3%E6%9E%90jwt%E4%B8%AD%E5%A1%AB%E5%86%99%E7%9A%84%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BF%A1%E6%81%AF)
      - [(1) 添加依赖](#1-%E6%B7%BB%E5%8A%A0%E4%BE%9D%E8%B5%96)
      - [(2) 添加过滤器、对AccessToken解码](#2-%E6%B7%BB%E5%8A%A0%E8%BF%87%E6%BB%A4%E5%99%A8%E5%AF%B9accesstoken%E8%A7%A3%E7%A0%81)
      - [(3) 运行](#3-%E8%BF%90%E8%A1%8C)
  - [9.5 微服务Security其他范畴](#95-%E5%BE%AE%E6%9C%8D%E5%8A%A1security%E5%85%B6%E4%BB%96%E8%8C%83%E7%95%B4)
  - [9.6 小结](#96-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

#  CH9 身份验证及访问管理

> 微服务安全涉及多个方面：应用层用户身份验证、基础架构层的组件漏洞识别（例如使用OWASP依赖项检查等）、网络层的网络控制等。本章仅涉及`身份验证`，内容包括：
>
> 1. OAuth2和OpenID
> 2. 安装和配置KeyCloak，用于身份验证、授权、微服务调用验证
> 3. 在服务之间传播Access Token
>
> 原书章节：[https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-9/v-8](https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-9/v-8)
>
> 本章Demo：[../chapter9/](../chapter9/)

## 9.1 OAuth2

> OAuth2有多种授权模式，内容较多，放在《附录B：OAuth2授权类型》中介绍，也可以参考《OAuth2 in Action， Justin richer and Antonio Sano，2017》这本书。
>
> 本章及Demo只涉及一种使用情况：通过用户名密码授权，用Access Token来进行网络调用

## 9.2 KeyCloak

KeyCloak是开源的身份验证和访问管理软件，支持SAML v2 / OIDC（OpenID Connect）/OAuth2，具有如下特点：

> - 集中式的身份验证以及单点登录身份验证（SSO），支持two-factor authentication，符合LDAP
> - 提供了多个适配器，不需要代码（或只需编写很少代码）使开发人员可以专注于业务功能
> - 允许自定义密码策略

整个应用抽象成为4个部分（组件）：`Protected Resource`、`Resource Owner`、`Application`、`Authentication/Authorization Server`。相互之间的作用关系如下：

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_auth_flow.jpg" width="800" /></div>
>
> 与图中的序号相对应：
>
> 1. `Protected Resource`：在Demo中即为具体的服务（liscensing-service，organization-service），只有经过授权和身份验证的用户才能访问
> 2. `Resource Owner`：定义哪些`Application`可以调用服务以及可以使用该服务做什么。每个`Application`都需要一个由`Resource Owner`授予的**应用程序名称**和**应用程序秘钥**，作为验证一个acccess token时所需凭据的一部分
> 3. `Application`：代表用户用来调用服务的程序（客户端）
> 4. `Authentication/Authorization Server`：用于用户身份验证以及access  token验证的中间模块

验证授权及服务访问过程如下：

> 1. 用户（Application）：使用其凭据向Keycloak服务器（Authentication/Authorzation Server）进行身份验证，验证通过后拿到一个access token
> 2. 用户（Application）访问具体的服务（Protected Resource）时，将access token一同传递
> 3. 服务（Protected Resource）联系Keycloak服务器（Authentication/Authorzation Server）验证token的有效性并 获取该用户已分配的角色，来确定他们可以访问哪些资源

## 9.3 Keycloak的搭建和配置

### 9.3.1 在Docker中添加Keycloak服务

#### (1) 在docker-compose.yml中添加Keycloak

> ~~~yml
> keycloak:
> 	image: jboss/keycloak
> 	restart: always
> 	environment:
> 		KEYCLOAK_VERSION: 6.0.1
> 		KEYCLOAK_USER: admin
> 		KEYCLOAK_PASSWORD: admin
> 	... 
> 	ports:
> 		- "8080:8080"
>     networks:
> 		backend:
> 			aliases:
> 				- "keycloak"
> ~~~
>
> 代码：[../chapter9/docker/docker-compose.yml](../chapter9/docker/docker-compose.yml)
>
> Keycloak需要使用数据库一起使用，支持H2、PostgreSQL、MySQL、Microsoft SQL Server， Oracle，MariaDB。Demo中为了方便演示没有配置数据库、进而使用了默认的嵌入式H2数据库。如果需要使用其他数据库，参考：[https://github.com/keycloak/keycloak-containers/tree/master/docker-compose-examples](https://github.com/keycloak/keycloak-containers/tree/master/docker-compose-examples)

#### (2) 端口修改

> KeyCloak Console占用了8080端口，因此licensing-service的端口映射由`8080:8080`改为`8081:8080`
>
> ~~~yml
> licensingservice:
> 	image: ostock/licensing-service:0.0.3-SNAPSHOT
> 	...
> 	ports:
> 		- "8180:8080"
> ~~~
>
> 代码：[../chapter9/docker/docker-compose.yml](../chapter9/docker/docker-compose.yml)

### 9.3.2 设置KeyCloak

#### (1) 创建容器

> 创建容器，等待服务都启动，在Eureka Dashboard（http://localhost:8070/）上的状态都是UP
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter9/
> docker-compose -f docker/docker-compose.yml up
> ~~~

#### (2) 配置KeyCloak

##### 登录`KeyCloak Console`

在浏览器访问`http://localhost:8080/auth/`打开KeyCloak控制台，页面如下

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_welcome.jpg" width="650" /></div>
>
> 点击”Administration Console"，进入登录页面
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_login.jpg" width="650" /></div>
>
> 使用配置在docker-compose.yml中的用户名密码登录

##### 创建`Realm` 

`Realm`是`KeyCload`用来引用一组`用户/凭据/角色/组`的对象的概念

> 创建项目demo所使用的`Spmia-realm`（SPring Microservice In Action realm）
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_realm_creation.jpg" width="800" /></div>
>
> 该realm使用OpenID，SAML 2.0
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_realm_cfg.jpg" width="800" /></div>

### 9.3.3 注册客户端应用程序

#### (1) 添加客户端

客户端是可以请求用户身份验证的实体

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_spmia_realm_clients.jpg" width="800" /></div>
>

点击右上角的“Create”，填入基本信息创建客户端“ostock”

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_client_add.jpg" width="600" /></div>

配置客户端

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_client_additional_cfg.jpg" width="650" /></div>
>
> `Access Type`：confidential，初始化登录协议是需要使用秘钥
>
> `Service Accounts Enabled`，`Authorization Enabled`：启用账户服务，开启授权功能
>
> `Valid Redirect URIs`：登录成功后跳转到的URI
>
> `Web Origins`：CORS Origins

客户端数量无限制，通常会创建多个细分的客户端配置。这个Demo为了演示的比较简洁，只创建一个通用的，可以用于所有Service的客户端。

#### (2) 为客户端添加角色

demo中使用两个角色：管理员（可以执行所有服务），常规用户（只能执行某些服务）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_client_list.jpg" width="800" /></div>
>
> 点击刚刚创建的ostock客户端（Spmia-realm → Clients → ostock），进入Client配置页面

为ostock客户端添加两个角色：`ADMIN`和`USER`

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_client_add_roles.jpg" width="450" /></div>
>
> 创建之的页面如下
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_role_display.jpg" width="800" /></div>
>

#### (3) 生成客户端凭据（`Credentials`）

在`客户端ostock`的`Credentials`标签页添加

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_client_secret.jpg" width="800" /></div>
>

#### (4) 创建Realm角色

进入realm角色页面：Spmia-realm → Roles → Roles

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_realm_role_page.jpg" width="800" /></div>

为这个Realm添加角色，一共添加两个`ostock-admin`和`ostock-user`，这样可以为后续添加用户时提供方便

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_add_admin_role.jpg" width="500" /></div>

`Ostock-admin`角色的详细配置

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_admin_role_additional_cfg.jpg" width="800" /></div>
>
> `Composite Roles`：ON，开启复合配置模式，这样就可以通过下面`Associated Roles`的方式把一组Client角色复合在一起，便于一次为用户授予多个权限
>
> `Client Roles`：选择之前创建的ostock客户端之后，页面会显示可以用于这个客户端的具体配置
>
> `Associated Roles`：一栏中选择ADMIN，将`ostock-admin`这个realm角色关联到之前为ostock客户端创建的Client角色`ADMIN`上

`Ostock-user`角色的详细配置			

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_ostock_user_role_detail.jpg" width="800" /></div>
>
> 其他配置相同，除了`Associated Roles`为USER，关联到ostock客户端角色`USER`上

创建好之后的realm Roles如下

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_realm_roles_2.jpg" width="600" /></div>

##  9.3.4 配置Ostock用户

在demo中将创建两个账户

> `illary.huaylupo`：有ostock-admin角色（关联了ostock客户端的ADMIN角色）
>
> `john.carnell`：有ostock-user角色（关联了ostock客户端的USER角色）

创建用户的页面（`Spmia-realm`→`Users`→`Add user`）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_user_creation_2.jpg" width="600" /></div
>
> `Email Verified`：表示用户email是否已经验证

保存后在该用户的`凭据`选项卡中设置密码

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_passwd_2.jpg" width="800" /></div>
>
> 将`Temporary`设为 `OFF`、否则登录时会要求用户改密码
>
> 点击`Set Password`之后密码生效

为该用户关联角色

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_keycloak_user_role_mapping.jpg" width="800" /></div>
>
> 将`ostock-admin`关联给用户`illary.huaylupo`

另一个用户`john.carnell`的创建和授权过程也类似，只不过关联的权限是`ostock-user`

### 9.3.5 验证用户

#### (1) 用来获取access_token的End Point

修改`/etc/hosts`，将127.0.0.1的host设为docker-compose.yml中keycloak的服务名，以便能够得到与容器环境中一致的endpoint url

> ~~~bash
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/java_proj_ref/002_micro_service/
> $ sudo vim /etc/hosts # 在/etc/hosts文件中添加关于keycloak的host配置
> Password:
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/java_proj_ref/002_micro_service/
> $ head /etc/hosts | grep -v '^#'  #第一条127.0.0.1	keycloak即为新增的配置
> 127.0.0.1	keycloak
> 127.0.0.1	localhost
> 255.255.255.255	broadcasthost
> ::1             localhost
> ~~~

在浏览器访问`http://keycloak:8080/auth/`（需要关闭VPN）并登录后，进入如下页面（`Spmia-realm` → `Realm Setting` → `General`）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_realm_openid_endpoints.jpg" width="800" /></div>
>

点击页面中的`OpenID Endpoint Configuration`，可以看到与该Endpoint相关的配置

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_realm_roles_to_created_user.jpg" width="800" /></div>
>

其中的` "token_endpoint": "http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/token"`就是用来获取access token的端点

#### (2) 用PostMan模拟客户端获取access token

> 使用POST请求，
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_basic_auth.jpg" width="800" /></div>
>
> Authorization Tab填入Client ID和Client Secret，具体的值是先前创建ostock客户端时配置值，在KeyCloak console的页面中可以找到

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_request_access_token_2.jpg" width="800" /></div>
>
> Body中使用`x-www-form-urlencoded`表单来提交查询参数
>
> 在Response中收到了如下的返回数据
>
> ~~~json
>{
>     "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJodGpNdWZKYXlGTkROOVZxVlVmbFpZQV80Q3ZuSFR1bGRNVmlldnE4VzFvIn0.eyJleHAiOjE2MTMyOTc2MDMsImlhdCI6MTYxMzI5NzMwMywianRpIjoiNTdjY2ZlYWUtYjEyMy00YjY5LTg4YTYtYjk0N2Y5MGQyNjNmIiwiaXNzIjoiaHR0cDovL2tleWNsb2FrOjgwODAvYXV0aC9yZWFsbXMvc3BtaWEtcmVhbG0iLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiNGU4ZjJjMjgtZjA2My00ODBlLThiM2ItNWY3MWMwNzJjNWI5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoib3N0b2NrIiwic2Vzc2lvbl9zdGF0ZSI6IjMzYWE5N2QyLTlhNjYtNDNiNy04YmQ1LWQ1ZGM3ZjVhMGI2NyIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiKiJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiIsIm9zdG9jay1hZG1pbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7Im9zdG9jayI6eyJyb2xlcyI6WyJBRE1JTiJdfSwiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJwcm9maWxlIGVtYWlsIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsInByZWZlcnJlZF91c2VybmFtZSI6ImlsbGFyeS5odWF5bHVwbyJ9.i3dFZTMnyq-dJkv3BeEdge4t57Dfgy_jbOeswuobnQXd5hxL7hA10pjvm1O_nWJwXOhaBZw2FOqzOK1IsUd_jDhe521gIp9tC-ayPINeib6ZdqYEWXH0tJjXXk9pAmyn-mSnMaIXEKPgL0GrRHnf5a-k8xk2uTWazESP0tziI5Hfn5nZHTvvunyYKKWRtvryuOTb7PRbBQFF2618Ff-k-18wuVyNyXHLfeCEb1QtNwMo-uxI53iJsHvWfO2wHty0HDYVKo-ilysYigMflIYhP75Lvx1xrdoc6JFw29mJiUqTUm89Osq1c5WWHI2FOjqMeNRO-E1ZGSomW5Tkh-Jb6g",
>    "expires_in": 300,
>     "refresh_expires_in": 1800,
>    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI5YWNiNzBlOC1iMTcxLTQ2ZGMtOTQzNC1jYTkxOTU3NjMwYjUifQ.eyJleHAiOjE2MTMyOTkxMDMsImlhdCI6MTYxMzI5NzMwMywianRpIjoiMDBhZTI0M2YtNjhjMC00Y2Y2LWFhZDgtZDhjMjc1MzkwOTM4IiwiaXNzIjoiaHR0cDovL2tleWNsb2FrOjgwODAvYXV0aC9yZWFsbXMvc3BtaWEtcmVhbG0iLCJhdWQiOiJodHRwOi8va2V5Y2xvYWs6ODA4MC9hdXRoL3JlYWxtcy9zcG1pYS1yZWFsbSIsInN1YiI6IjRlOGYyYzI4LWYwNjMtNDgwZS04YjNiLTVmNzFjMDcyYzViOSIsInR5cCI6IlJlZnJlc2giLCJhenAiOiJvc3RvY2siLCJzZXNzaW9uX3N0YXRlIjoiMzNhYTk3ZDItOWE2Ni00M2I3LThiZDUtZDVkYzdmNWEwYjY3Iiwic2NvcGUiOiJwcm9maWxlIGVtYWlsIn0.rEFEPpNIhQZEfgb7YxE-v8ME38B_HnUqkdBPS91RsZ4",
>     "token_type": "Bearer",
>    "not-before-policy": 0,
>     "session_state": "33aa97d2-9a66-43b7-8bd5-d5dc7f5a0b67",
>    "scope": "profile email"
> }
>~~~
> 
>`access_token`：是用来调用各个Service API所需要的数据
> 
>`expires_in`：是access token在多少秒之后会过期
> 
>`token_type`：是access token的类型
> 
>`refresh_token`：过期后用来刷新access token时用到的token
> 
>`scope`：the defined  scope for which the token is valid

####  (3) 使用https://jwt.io来解码access token

> access token解码后的内容形式如下
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_user_info_look_up.jpg" width="800" /></div>
>

## 9.4 使用KeyCloak保护organization-service

> 本小节先为`orgnization-service`添加KeyCloak保护（下一小节再为`licensing-service`添加KeyCloak保护），步骤如下

### 9.4.1 添加`Spring Security`和`KeyCloak`依赖项

> ~~~xml
> <dependencies>
> 	...
> 	<dependency>
> 		<groupId>org.keycloak</groupId>
> 		<artifactId>keycloak-spring-boot-starter</artifactId>
> 	</dependency>
> 	<dependency>
> 		<groupId>org.springframework.boot</groupId>
> 		<artifactId>spring-boot-starter-security</artifactId>
> 	</dependency>
> </dependencies>
> <dependencyManagement>
> 	<dependencies>
> 		<dependency>
> 			<groupId>org.springframework.cloud</groupId>
> 			<artifactId>spring-cloud-dependencies</artifactId>
> 			<version>${spring-cloud.version}</version>
> 			<type>pom</type>
> 			<scope>import</scope>
> 		</dependency>
> 		<dependency>
> 			<groupId>org.keycloak.bom</groupId>
> 			<artifactId>keycloak-adapter-bom</artifactId>
> 			<version>11.0.2</version>
> 			<type>pom</type>
> 			<scope>import</scope>
> 		</dependency>
> 	</dependencies>
> </dependencyManagement>
> ~~~
>
> 代码：[../chapter9/organization-service/pom.xml](../chapter9/organization-service/pom.xml)

### 9.4.2 配置服务以指向KeyCloak服务器

> 让organization-service作为受保护的资源，意味着每次服务调用：
>
> * 请求中必须包含Authentication HTTP Header并携带Bearer Access  Token
> * 而organization-service必须回调KeyCloak服务器，查看Access  Token是否有效
>
> 为organization-service增加如下配置，配置项的值与之前在KeyCloak Console中进行配置时的值一样，特别是`keycloak.credentials.secret`
>
> ~~~properties
> keycloak.realm = spmia-realm 
> keycloak.auth-server-url = http://keycloak:8080/auth
> keycloak.ssl-required = external
> keycloak.resource = ostock
> keycloak.credentials.secret = 4f73ad80-a9d3-4e55-8a22-f44f3d4ad841
> keycloak.use-resource-role-mappings = true
> keycloak.bearer-only = true
> ~~~
>
> 代码：[../chapter9/configserver/src/main/resources/config/organization-service.properties](../chapter9/configserver/src/main/resources/config/organization-service.properties)

### 9.4.3 定义服务访问规则

#### (1) 只容许授权的用户访问

##### (a) 配置

> 通过扩展`KeycloakWebSecurityConfigurerAdapter`类覆盖该类的方法来实现
>
> ~~~java
> @Configuration  	// 作为一个Configuration类，用来生成Bean以供装配 
> @EnableWebSecurity	// 这个Configuration类用于Global WebSecurity
> @EnableGlobalMethodSecurity(jsr250Enabled = true)	// 为了能够使用@RoleAllowed注解 
> public class SecurityConfig extends KeycloakWebSecurityConfigurerAdapter {  // 需要扩展KeycloakWebSecurityConfigurerAdapter基类
>     // security访问规则
> 	@Override
> 	protected void configure(HttpSecurity http) throws Exception {
> 		super.configure(http);
> 		// (1) 放行所有请求
> 		// http.authorizeRequests().anyRequest().permitAll();
>         // (2) 只有经过身份验证的用户可以访问，访问范围是organization-service的所有url
> 		http.authorizeRequests().anyRequest().authenticated();
> 		http.csrf().disable();
> 	}
> 
> 	// 注册KeyCloakAuthentication Provider
> 	@Autowired
> 	public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
> 		KeycloakAuthenticationProvider keycloakAuthenticationProvider = keycloakAuthenticationProvider();
> 		keycloakAuthenticationProvider.setGrantedAuthoritiesMapper(new SimpleAuthorityMapper());
> 		auth.authenticationProvider(keycloakAuthenticationProvider);
> 	}
> 
> 	// 定义session authentication strategy
> 	@Bean
> 	@Override
> 	protected SessionAuthenticationStrategy sessionAuthenticationStrategy() {
> 		return new RegisterSessionAuthenticationStrategy(new SessionRegistryImpl());
> 	}
> 
>     // 默认情况下Spring Security Adapter将会查找key-cloak.json文件
> 	@Bean
> 	public KeycloakConfigResolver KeycloakConfigResolver() {
> 		return new KeycloakSpringBootConfigResolver();
> 	}
> }
> ~~~
>
> 完整代码：[../chapter9/organization-service/src/main/java/com/optimagrowth/organization/config/SecurityConfig.java](../chapter9/organization-service/src/main/java/com/optimagrowth/organization/config/SecurityConfig.java)

##### (b) 测试

如果HTTP请求中没有携带Access Token，会返回`401 Unauthorized Status Code`

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_401_unauthorized.jpg" width="800" /></div>
>

如果HTTP请求中携带了Access Token（生成Token的方法见9.2.5小节），这回返回`200 OK Status Code`

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_200_ok_with_access_token.jpg" width="800" /></div>
>

#### (2) 只容许符合特定角色的用户访问

> 例如，某个执行DELETE操作的REST End Point，只容许具有ADMIN角色的用户使用，这要通过@RolesAllowed注解来实现 

代码：

> ~~~java
> @RestController
> @RequestMapping(value="v1/organization")
> public class OrganizationController {
> 	@Autowired
> 	private OrganizationService service;
> 
> 	// 前3个方法，具有ADMIN和USER角色的用户都可以访问
> 	@RolesAllowed({ "ADMIN", "USER" })  
> 	@RequestMapping(value="/{organizationId}",method = RequestMethod.GET)
> 	public ResponseEntity<Organization> getOrganization( @PathVariable("organizationId") String organizationId) {
> 		return ResponseEntity.ok(service.findById(organizationId));
> 	}
> 
> 	@RolesAllowed({ "ADMIN", "USER" }) 
> 	@RequestMapping(value="/{organizationId}",method = RequestMethod.PUT)
> 	public void updateOrganization( @PathVariable("organizationId") String id, @RequestBody Organization organization) {
> 		service.update(organization);
> 	}
> 
> 	@RolesAllowed({ "ADMIN", "USER" }) 
> 	@PostMapping
> 	public ResponseEntity<Organization>  saveOrganization(@RequestBody Organization organization) {
> 		return ResponseEntity.ok(service.create(organization));
> 	}
> 
> 	// 最后一个方法，只有具有ADMIN角色的用户才可以访问
> 	@RolesAllowed("ADMIN")    
> 	@DeleteMapping(value="/{organizationId}")
> 	@ResponseStatus(HttpStatus.NO_CONTENT)
> 	public void deleteLicense(@PathVariable("organizationId") String organizationId) {
> 		service.delete(organizationId);
> 	}
> 
> }
> 
> ~~~
>
> 如果使用john.carnell（角色是USER）的access token访问，会返回403 Forbidden Statis
>
> ~~~json
> {
> 	"timestamp": "2020-10-19T01:19:56.534+0000",
> 	"status": 403,
> 	"error": "Forbidden",
> 	"message": "Forbidden",
> 	"path": "/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c7"
> }
> ~~~
>
> 如果使用illary.huaylupo（角色是ADMIN）的access token访问，会返回204 No Content Status并执行删除操作

### 9.4.4 传播访问令牌

#### (1) 访问令牌传播和验证过程

> 前面的几个小节只能保护一个服务，如果要保护整个Transaction，则需要让access token在服务调用之间传播，通过gateway-server与各个服务之间的交互来完成，过程如下
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_propogate_access_token.jpg" width="800" /></div>
>
> 对应图中的序号：
>
> 1. 用户已经通过KeyCloak身份验证并向Web App发出请求，Access Token存储在用户会话中。Web App通过HTTP Heder的`Authorization字段`添加了Access Token
> 2. gateway-server将请求路由到负责该请求的REST服务端点licensing-service上，转发请求时复制`Authorization HTTP Header`
> 3. licensing-service
>     * 从`Authorization HTTP Header`中拿到access token，发给KeyCloak服务器做验证，确认发起请求的用户具有适当的角色权限
>     * 执行处理该请求的业务逻辑
>     * 业务逻辑中涉及到了调用`organization-service`，因此将`Authorization HTTP Header` 复制到发往organization-service的请求中
> 4. organization-service
>     * 同样从`Authorization HTTP Header`中拿到access  token，向KeyCloak服务器验证该token

#### (2) 代码修改

> 如果不能把access token传到下游服务，调用到这些服务时会发生401 Unauthorized错误
>
> ~~~
> message": "401 : {[status:401,
> error: Unauthorized
> message: Unauthorized,
> path: /v1/organization/d898a142-de44-466c-8c88-9ceb2c2429d3}]
> ~~~
>
> 代码修改包括如下几处：

##### (a) gateway-server路由转发Header配置

> 默认情况下，网关不会将敏感的HTTP Header（例如Cookie、Set-Cookie、Authorization）转发到下游服务，因此要覆盖默认的配置（每一条路由都配置，不论是自动路由映射还是手动路由映射）
>
> ~~~yml
> spring:
> 	...
> 	cloud:
> 		loadbalancer.ribbon.enabled: false
> 		gateway:
> 			routes:
> 			- id: organization-service
> 				uri: lb://organization-service
> 				predicates:
> 				- Path=/organization/**
> 				filters:
> 				- RewritePath=/organization/(?<path>.*), /$\{path}
> 				# 只删除Cookie，Set-Cookie Header，保留Authorization Header
> 				- RemoveRequestHeader= Cookie,Set-Cookie
> 			- id: licensing-service
> 				uri: lb://licensing-service
> 				predicates:
> 				- Path=/license/**
> 				filters:
> 				- RewritePath=/license/(?<path>.*), /$\{path}
> 				# 只删除Cookie，Set-Cookie Header，保留Authorization Header
> 				- RemoveRequestHeader= Cookie,Set-Cookie
> ~~~
>
> 代码：[../chapter9/configserver/src/main/resources/config/gateway-server.yml](../chapter9/configserver/src/main/resources/config/gateway-server.yml)

##### (b) licensing-service

###### 添加keyCloak依赖

> ```xml
> <dependencies>
> 	<dependency>
> 		<groupId>org.keycloak</groupId>
> 		<artifactId>keycloak-spring-boot-starter</artifactId>
> 	</dependency>
> 	<dependency>
> 		<groupId>org.springframework.boot</groupId>
> 		<artifactId>spring-boot-starter-security</artifactId>
> 	</dependency>
> </dependencies>
> <dependencyManagement>
> 	<dependencies>
> 		<dependency>
> 			<groupId>org.springframework.cloud</groupId>
> 			<artifactId>spring-cloud-dependencies</artifactId>
> 			<version>${spring-cloud.version}</version>
> 			<type>pom</type>
> 			<scope>import</scope>
> 		</dependency>
> 		<dependency>
> 			<groupId>org.keycloak.bom</groupId>
> 			<artifactId>keycloak-adapter-bom</artifactId>
> 			<version>11.0.2</version>
> 			<type>pom</type>
> 			<scope>import</scope>
> 		</dependency>
> 	</dependencies>
> </dependencyManagement>
> ```
>
> 代码：[../chapter9/licensing-service/pom.xml](../chapter9/licensing-service/pom.xml)

###### 配置服务以指向KeyCloak服务器

> ~~~properties
> keycloak.realm = spmia-realm
> keycloak.auth-server-url = http://keycloak:8080/auth
> keycloak.ssl-required = external
> keycloak.resource = ostock
> keycloak.credentials.secret = 4f73ad80-a9d3-4e55-8a22-f44f3d4ad841
> keycloak.use-resource-role-mappings = true
> keycloak.bearer-only = true
> ~~~
>
> 完整代码：[../chapter9/configserver/src/main/resources/config/licensing-service.properties](../chapter9/configserver/src/main/resources/config/licensing-service.properties)
>
> 具体配置方法与9.4.2小节相同，配置内容需要与之前在KeyCloak Console中配置相同（而不是与上面的代码相同 ），特别是`keycloak.credentials.secret`

###### 定义服务访问规则，定义可以传播Authorization HTTP Header的KeycloakRestTemplate

> ~~~java
> @Configuration		// 作为一个Configuration类，用来生成Bean以供装配 
> @EnableWebSecurity	// 这个Configuration类用于Global WebSecurity
> @ComponentScan(basePackageClasses = KeycloakSecurityComponents.class) // 限制包扫描的范围
> public class SecurityConfig extends KeycloakWebSecurityConfigurerAdapter { // 需要扩展KeycloakWebSecurityConfigurerAdapter基类
> 	/******************************************************************
> 	* 通过KeyCloak提供的KeycloakRestTemplate可以直接Authorization HTTP Header复制到发往下游的Request中
> 	* 而不必手动实现Filter提取逻辑和Header拷贝逻辑
> 	**/	
> 	@Autowired
> 	public KeycloakClientRequestFactory keycloakClientRequestFactory;
> 
> 	@Bean
> 	@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
> 	public KeycloakRestTemplate keycloakRestTemplate() {
> 		return new KeycloakRestTemplate(keycloakClientRequestFactory);
> 	}
> 
> 	/******************************************************************
> 	* 定义服务访问规则
> 	**/
> 
> 	// security访问规则
> 	@Override
> 	protected void configure(HttpSecurity http) throws Exception {
> 		super.configure(http);
> 		// 只有经过身份验证的用户可以访问，访问范围是organization-service的所有url
> 		http.authorizeRequests().anyRequest().authenticated();
> 		http.csrf().disable();
> 	}
> 
> 	// 注册KeyCloakAuthentication Provider
> 	@Autowired
> 	public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
> 		KeycloakAuthenticationProvider keycloakAuthenticationProvider = keycloakAuthenticationProvider();
> 		keycloakAuthenticationProvider.setGrantedAuthoritiesMapper(new SimpleAuthorityMapper());
> 		auth.authenticationProvider(keycloakAuthenticationProvider);
> 	}
> 
> 	// 定义session authentication strategy
> 	@Bean
> 	@Override
> 	protected SessionAuthenticationStrategy sessionAuthenticationStrategy() {
> 		return new RegisterSessionAuthenticationStrategy(new SessionRegistryImpl());
> 	}
> 
> 	// 默认情况下Spring Security Adapter将会查找key-cloak.json文件
> 	@Bean
> 	public KeycloakConfigResolver KeycloakConfigResolver() {
> 		return new KeycloakSpringBootConfigResolver();
> 	}
> }
> ~~~
>
> 代码：[../chapter9/licensing-service/src/main/java/com/optimagrowth/license/config/SecurityConfig.java](../chapter9/licensing-service/src/main/java/com/optimagrowth/license/config/SecurityConfig.java)
>
> `定义服务访问规则`部分与`9.4.3.(1)`小节相同、
>
> `定义可以传播Authorization HTTP Header的KeycloakRestTemplate`部分如代码注释 

###### 使用`KeycloakRestTemplate`发送请求并传播access token

> ~~~java
> @Component
> public class OrganizationRestTemplateClient {
> 	// 使用KeycloakRestTemplate
>     @Autowired
> 	KeycloakRestTemplate restTemplate;
> 
> 	public Organization getOrganization(String organizationId){
> 		// 发往gateway-server，其中"gateway"是配置在docker-compose.yml中的服务名
> 		ResponseEntity<Organization> restExchange = restTemplate.exchange(
> 				"http://gateway:8072/organization/v1/organization/{organizationId}",
> 				HttpMethod.GET,
> 				null, Organization.class, organizationId);
> 		return restExchange.getBody();
> 	}
> }
> ~~~
>
> 测试
>
> ~~~bash
> http://localhost:8072/license/v1/organization/d898a142-de44-466c-8c88-9ceb2c2429d3/license/f2a9c9d4-d2c0-44fa-97fe-724d77173c62
> ~~~
>
> 返回结果中可以看到来自oragnization-service的数据
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_pass_access_token_to_services.jpg" width="800" /></div>

### 9.4.5 解析JWT中填写的自定义信息

> 之前使用`https://jwt.io`接下Access Token，可以提取出token的数据；同样也可以编写代码在程序中解析Access Token。下面的代码在gateway-server中接卸Access Token并将其输出到日志中
>

#### (1) 添加依赖

> ~~~xml
> <dependency>
> 	<groupId>commons-codec</groupId>
> 	<artifactId>commons-codec</artifactId>
> </dependency>
> <dependency>
> 	<groupId>org.json</groupId>
> 	<artifactId>json</artifactId>
> 	<version>20190722</version>
> </dependency>
> ~~~
>
> 代码：[../chapter9/gatewayserver/pom.xml](../chapter9/gatewayserver/pom.xml)

#### (2) 添加过滤器、对AccessToken解码

> ~~~java
> @Order(1)
> @Component
> public class TrackingFilter implements GlobalFilter {
> 	private static final Logger logger = LoggerFactory.getLogger(TrackingFilter.class);
> 
> 	@Autowired
> 	FilterUtils filterUtils;
> 
> 	@Override
> 	public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
> 		HttpHeaders requestHeaders = exchange.getRequest().getHeaders();
> 		...
> 		logger.debug("The authentication name from the token is : " + getUsername(requestHeaders));
> 		return chain.filter(exchange);
> 	}
> 
> 	// 在过滤器中，从HttpHeaders中提取access token并解码
> 	private String getUsername(HttpHeaders requestHeaders){
> 		String username = "";
> 		if (filterUtils.getAuthToken(requestHeaders)!=null){
> 			// 从Authorization HTTP Header取出Access Token
> 			String authToken = filterUtils.getAuthToken(requestHeaders).replace("Bearer ","");
> 			// 将Access Token解码成JSON
> 			JSONObject jsonObj = decodeJWT(authToken);
> 			try {
> 				// 从json中提取user name
> 				username = jsonObj.getString("preferred_username");
> 			} catch(Exception e) {
> 				logger.debug(e.getMessage());
> 			}
> 		}
> 		return username;
> 	}
> 
> 	// 将Access Token解码成JSON
> 	private JSONObject decodeJWT(String JWTToken) {
> 		// 从Access Token中提取BASE64 encode body
> 		String[] split_string = JWTToken.split("\\.");
> 		String base64EncodedBody = split_string[1];
> 		// BASE64解码
> 		Base64 base64Url = new Base64(true);
> 		String body = new String(base64Url.decode(base64EncodedBody));
> 		// 将解码后的数据转换成Json Object并返回
> 		JSONObject jsonObj = new JSONObject(body);
> 		return jsonObj;
> 	}
> }
> ~~~

#### (3) 运行

> 日志中可以看到用户名被输出
>
> ~~~bash
> tmx-correlation-id found in tracking filter: 26f2b2b7-51f0-4574-9d84-07e563577641.
> The authentication name from the token is : illary.huaylupo
> ~~~

## 9.5 微服务Security其他范畴

除了使用OpenID、OAuth2实现微服务身份验证和授权服务以外，还有更多的Security范畴，还包括

> * 使用HTTPS/SSL进行所有服务间的通信
> * 所有服务调用都通过API网关
> * 将服务划分为公有API和私有API
> * 关闭不需要的网络端口来限制微服务的攻击面

下图是完整的微服务安全体系结构

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch09_microservice_security_arch.jpg" width="800" /></div>
>

与图中序号相对

1 . 使用HTTPS/SSL进行所有服务通信

> 在生产环境中，微服务应当仅通过HTTPS和SSL提供的加密通道进行通信，这些可以通过DevOps脚本自动进行

2 . 使用服务网关访问微服务

> 服务网关可以充当针对所有服务执行的策略执行点（PEP），所有请求都必须通过网关来进行

3 . 将服务划分为公共API和专用API

> 将服务分为两个不同的区域来实现最小特权：`公共区域`和`私有区域`
>
> 公共区域：给客户端使用的公共API，位于网关之后并具有身份验证和授权的公共能
>
> 私有区域：只能通过单个指定的端口访问，并且仅接受来自专用服务的网络子网的网络流量

4 . 通过锁定不需要的网络端口来限制微服务攻击面

> 只打开所需要的端口，防止攻击中从其他端口发起攻击或取走数据

## 9.6 小结

> 略