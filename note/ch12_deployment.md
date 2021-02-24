<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!--**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*-->

- [CH12 微服务部署](#ch12-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E9%83%A8%E7%BD%B2)
  - [12.1 `Build and Deployment Pipeline`结构](#121-build-and-deployment-pipeline%E7%BB%93%E6%9E%84)
    - [(1) Build and Deployment Pipeline](#1-build-and-deployment-pipeline)
    - [(2) 单元测试、集成测试、平台测试](#2-%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95%E9%9B%86%E6%88%90%E6%B5%8B%E8%AF%95%E5%B9%B3%E5%8F%B0%E6%B5%8B%E8%AF%95)
    - [(3) `构建/部署`环节的四个核心模式（practice pattern）](#3-%E6%9E%84%E5%BB%BA%E9%83%A8%E7%BD%B2%E7%8E%AF%E8%8A%82%E7%9A%84%E5%9B%9B%E4%B8%AA%E6%A0%B8%E5%BF%83%E6%A8%A1%E5%BC%8Fpractice-pattern)
    - [(4) 本章Demo内容说明](#4-%E6%9C%AC%E7%AB%A0demo%E5%86%85%E5%AE%B9%E8%AF%B4%E6%98%8E)
    - [(5) Kubernates参考](#5-kubernates%E5%8F%82%E8%80%83)
  - [12.2 在AWS上部署数据库（PostgreSQL和Redis）](#122-%E5%9C%A8aws%E4%B8%8A%E9%83%A8%E7%BD%B2%E6%95%B0%E6%8D%AE%E5%BA%93postgresql%E5%92%8Credis)
    - [(1) 服务部署结构](#1-%E6%9C%8D%E5%8A%A1%E9%83%A8%E7%BD%B2%E7%BB%93%E6%9E%84)
    - [(2) 前置准备](#2-%E5%89%8D%E7%BD%AE%E5%87%86%E5%A4%87)
    - [(3) AWS参考](#3-aws%E5%8F%82%E8%80%83)
    - [(4) 费用说明](#4-%E8%B4%B9%E7%94%A8%E8%AF%B4%E6%98%8E)
    - [12.2.1 在Amazon RDS上创建PostgreSQL](#1221-%E5%9C%A8amazon-rds%E4%B8%8A%E5%88%9B%E5%BB%BApostgresql)
    - [12.2.2 在Amazon上创建Redis集群](#1222-%E5%9C%A8amazon%E4%B8%8A%E5%88%9B%E5%BB%BAredis%E9%9B%86%E7%BE%A4)
  - [12.3 在AWS上部署ELK（Elasticsearch，Logstash，Kibana）和Services](#123-%E5%9C%A8aws%E4%B8%8A%E9%83%A8%E7%BD%B2elkelasticsearchlogstashkibana%E5%92%8Cservices)
    - [12.3.1 创建用于部署ELK Stack的EC2](#1231-%E5%88%9B%E5%BB%BA%E7%94%A8%E4%BA%8E%E9%83%A8%E7%BD%B2elk-stack%E7%9A%84ec2)
    - [12.3.2 在EC2 instance上部署ELK  stack（ElasticSearch、LogStash、 Kibana）](#1232-%E5%9C%A8ec2-instance%E4%B8%8A%E9%83%A8%E7%BD%B2elk--stackelasticsearchlogstash-kibana)
    - [12.3.3 创建EKS Cluster并部署服务](#1233-%E5%88%9B%E5%BB%BAeks-cluster%E5%B9%B6%E9%83%A8%E7%BD%B2%E6%9C%8D%E5%8A%A1)
      - [(1) 创建Kubernetes集群](#1-%E5%88%9B%E5%BB%BAkubernetes%E9%9B%86%E7%BE%A4)
      - [(2) 将各服务的Docker Image推送到ECR](#2-%E5%B0%86%E5%90%84%E6%9C%8D%E5%8A%A1%E7%9A%84docker-image%E6%8E%A8%E9%80%81%E5%88%B0ecr)
      - [(3) 在EKS集群部署微服务](#3-%E5%9C%A8eks%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2%E5%BE%AE%E6%9C%8D%E5%8A%A1)
        - [Step 1：根据AWS环境更新config-server存储的服务配置](#step-1%E6%A0%B9%E6%8D%AEaws%E7%8E%AF%E5%A2%83%E6%9B%B4%E6%96%B0config-server%E5%AD%98%E5%82%A8%E7%9A%84%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE)
        - [Step 2：部署Kafaka和Zookeeper](#step-2%E9%83%A8%E7%BD%B2kafaka%E5%92%8Czookeeper)
        - [Step 3：将`docker-comose.yml`转换为兼容Kubernetes的文件](#step-3%E5%B0%86docker-comoseyml%E8%BD%AC%E6%8D%A2%E4%B8%BA%E5%85%BC%E5%AE%B9kubernetes%E7%9A%84%E6%96%87%E4%BB%B6)
        - [后续步骤1：为PostgreSQL指定external name](#%E5%90%8E%E7%BB%AD%E6%AD%A5%E9%AA%A41%E4%B8%BApostgresql%E6%8C%87%E5%AE%9Aexternal-name)
        - [后续步骤2：创建`VPC peering`以及路由表以便让RDS和EKS集群可以互相通信](#%E5%90%8E%E7%BB%AD%E6%AD%A5%E9%AA%A42%E5%88%9B%E5%BB%BAvpc-peering%E4%BB%A5%E5%8F%8A%E8%B7%AF%E7%94%B1%E8%A1%A8%E4%BB%A5%E4%BE%BF%E8%AE%A9rds%E5%92%8Ceks%E9%9B%86%E7%BE%A4%E5%8F%AF%E4%BB%A5%E4%BA%92%E7%9B%B8%E9%80%9A%E4%BF%A1)
  - [12.4 `build and deployment pipeline`所用到的技术](#124-build-and-deployment-pipeline%E6%89%80%E7%94%A8%E5%88%B0%E7%9A%84%E6%8A%80%E6%9C%AF)
  - [12.5 Github和Jenkins](#125-github%E5%92%8Cjenkins)
    - [(1) 在EKS中搭建Jenkins环境](#1-%E5%9C%A8eks%E4%B8%AD%E6%90%AD%E5%BB%BAjenkins%E7%8E%AF%E5%A2%83)
    - [12.5.1 设置Github](#1251-%E8%AE%BE%E7%BD%AEgithub)
      - [(1) 创建GitHub Webhook](#1-%E5%88%9B%E5%BB%BAgithub-webhook)
    - [12.5.2 在Jenkins中构建Services](#1252-%E5%9C%A8jenkins%E4%B8%AD%E6%9E%84%E5%BB%BAservices)
      - [(1) 理解Pipeline运行过程](#1-%E7%90%86%E8%A7%A3pipeline%E8%BF%90%E8%A1%8C%E8%BF%87%E7%A8%8B)
      - [(2) 安装Jenkins插件](#2-%E5%AE%89%E8%A3%85jenkins%E6%8F%92%E4%BB%B6)
      - [(3) 创建Kubernetes Credential和ECR Credential](#3-%E5%88%9B%E5%BB%BAkubernetes-credential%E5%92%8Cecr-credential)
        - [(a) 创建Kubernetes Credential](#a-%E5%88%9B%E5%BB%BAkubernetes-credential)
        - [(b) 创建ECR Credential](#b-%E5%88%9B%E5%BB%BAecr-credential)
          - [在AWS  IAM页面上创建用户并下载Credential](#%E5%9C%A8aws--iam%E9%A1%B5%E9%9D%A2%E4%B8%8A%E5%88%9B%E5%BB%BA%E7%94%A8%E6%88%B7%E5%B9%B6%E4%B8%8B%E8%BD%BDcredential)
          - [在Jenkins中添加Credential](#%E5%9C%A8jenkins%E4%B8%AD%E6%B7%BB%E5%8A%A0credential)
      - [(4) 创建Jenkins Pipeline](#4-%E5%88%9B%E5%BB%BAjenkins-pipeline)
    - [12.5.3 理解和编写pipeline script](#1253-%E7%90%86%E8%A7%A3%E5%92%8C%E7%BC%96%E5%86%99pipeline-script)
    - [12.5.4 生成kubernetes deploy阶段的命令脚本](#1254-%E7%94%9F%E6%88%90kubernetes-deploy%E9%98%B6%E6%AE%B5%E7%9A%84%E5%91%BD%E4%BB%A4%E8%84%9A%E6%9C%AC)
  - [12.6 关于本书Demo构建过程的说明](#126-%E5%85%B3%E4%BA%8E%E6%9C%AC%E4%B9%A6demo%E6%9E%84%E5%BB%BA%E8%BF%87%E7%A8%8B%E7%9A%84%E8%AF%B4%E6%98%8E)
  - [12.7 小结](#127-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# CH12 微服务部署

> 本章内容
>
> * 自动化、可重复、完整的、不可变的部署，对微服务至关重要
> * 构建Build and Deployment Pipeline，使项目运行在AWS上 
>
> 原书章节：[https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-12/v-8/](https://livebook.manning.com/book/spring-microservices-in-action-second-edition/chapter-12/v-8/)
>
> 代码：[../chapter12/](../chapter12/)

## 12.1 `Build and Deployment Pipeline`结构

### (1) Build and Deployment Pipeline

> 下面是Build and Deployment Pipeline的结构示意图
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_automates_build_and_delpoy.jpg" width="800" /></div>
>
> 对应图中序号：
>
> 1. 代码提交到git repo
> 2. pipeline检测到更改时启动构建
> 3. 自动执行编译、单元测试、集成测试，将项目打包成一个可以运行Artifact（对Spring Boot来说是一个可以运行的JAR包）
> 4. 根据容器编排脚本，生成镜像，并部署出一套测试环境
> 5. 进行一些列平台测试（platform test），确认程序运行正确后，容许promete这套镜像到下一套环境中（例如从dev环境到test环境）
> 6. 在每一次向更高级别环境（例如从test环境到prod环境），都要求做平台测试（platform test），并且要保证镜像不可变

### (2) 单元测试、集成测试、平台测试

### (3) `构建/部署`环节的四个核心模式（practice pattern）

> 1. `Continuous Integration/Continuous Delivery (CI/CD)`：对提交及时进行构建和测试
> 2. `Infrastructure as code`：任何`provisioning scripts`都应当向代码一样被保管
> 3. `Immutable servers`：确保环境不会受“configuration drift”的影响 

### (4) 本章Demo内容说明

> 涵盖如下操作
>
> 1. 将Maven构建脚本集成到Jenkins的持续集成/部署工具中
> 2. 为每个服务构建不可变的Docker映像，并将这些映像推送到集中式存储库
> 3. 使用Amazon的Kubernetes容器服务（EKS）将整个微服务套件部署到AWS

### (5) Kubernates参考

> Kubernates In Action：
>
> 第一版（2017年出版）：https://www.manning.com/books/kubernetes-in-action
>
> 第二版（预计2021.7出版），早期预览版本：https://www.manning.com/books/kubernetes-in-action-second-edition

##  12.2 在AWS上部署数据库（PostgreSQL和Redis）

### (1) 服务部署结构

> PostgreSQL、Redis等基础设施从Docker切换到AWS上；而各个service以Docker的容器的方式部署到单节点的Amazon EKS cluster上：部署结构如下图 
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_docker_for_amazon_eks.jpg" width="600" /></div>
>
> 对应图中的序号：
>
> 1. 图片下部所有service都部署为单节点EKS集群内的Docker容器。EKS会为Docker容器配置VM、并且在服务崩溃时重启服务
> 2. 使用Amazon RDS提供的PostgreSQL和Redis，使用Amazon ElastiCache提供的ElasticSearch
> 3. 配置Amazon Security Group、只暴露gateway-server的8072端口给外界，所有请求都只能通过网关
> 4. 仍然使用OAuth2
> 5. gateway-server以外的所有server（包括Kafka）都只能在内部访问，不对外界公开

### (2) 前置准备

> 需要如下准备：
>
> 1. Amazon Web Services（AWS）帐户，基本了解AWS控制台以及相关概念
> 2. Web浏览器
>
> 3. AWS CLI，管理AWS服务的统一工具。https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html
>
> 4. Kubectl 。与Kubernetes集群通信交互的工具。https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl
>
> 5. IAM身份验证器。 为Kubernetes集群提供身份验证的工具。https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html
>
> 6. Eksctl。 命令行工具、用于在AWS账户中管理和创建AWS EKS集群。https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

### (3) AWS参考

> Amazon Web Services in Action
>
> https://www.manning.com/books/amazon-web-services-in-action-second-edition

### (4) 费用说明

> 本章Demo尽量使用AWS免费服务，付费的EKS cluster使用的m4.large server费用大约是每小时0.10美分（0.10 cents/hour）
>
> EC2的价格可参考 [https://aws.amazon.com/ec2/pricing/on-demand/](https://aws.amazon.com/ec2/pricing/on-demand/)

### 12.2.1 在Amazon RDS上创建PostgreSQL

前置操作：配置AWS账户

创建PostgreSQL过程：

1 . 登录AWS Console：https://aws.amazon.com/console/

2 . 点击RDS连接进入RDS Dashboard

3 . 点击“Create Database”进入数据库创建页面

4 . 数据库列表中选择PostgreSQL，单击"选项"，进入PostgreSQL创建页面

5 . 数据库模板选择“Free tier”，设置Master Account（与程序访问数据的账号不是同一个）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_aws_rds_postgresql_1.jpg" width="800" /></div>
>

6 . 数据库实例大小、存储、可用性、认证等都使用默认配置

7 . 设置为虚拟私有云（VPC）、公众可访问、填入安全组组名，端口为5432

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_aws_rds_postgresql_2.jpg" width="800" /></div>
>

8 . 等待几分钟后，数据库创建完毕并显示在RDS仪表盘中

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_aws_rds_postgresql_3.jpg" width="800" /></div>
>

9 . 数据库配置好之后，可以将AWS中数据库连接信息（endpoint，port等）更新到在config-server的配置存储中

> config-server配置：[../chapter12/configserver/src/main/resources/bootstrap.yml](../chapter12/configserver/src/main/resources/bootstrap.yml)
>
> ~~~yml
> spring:
> 	application:
> 		name: config-server
> 	profiles:
> 		active:
> 		- git
> 	cloud:
> 		config:
> 			server:
> 				#native:
> 					#search-locations: classpath:/config
> 			git:
> 				uri: https://github.com/ihuaylupo/config.git
> ~~~
>
> 配置存储：[https://github.com/ihuaylupo/config.git](https://github.com/ihuaylupo/config.git)
>
> 备注：
>
> * 原书描述"使用config-server本地文件存储配置并用aws-dev作为spring profile标识"，应当是笔误
>
> * 后文`12.3.3`小节以及Demo代码都已经使用git 作为config-server存储 ，应当以后文为准，可待书籍正式出版后核对

### 12.2.2 在Amazon上创建Redis集群

Amazon ElastiCache服务提供了Redis和Memcached（([https://memcached.org/](https://memcached.org/))服务，用于构建内存缓存

在AWS Console页面搜索ElastiCache，点击ElastiCache连接进入ElasCache Console，在页面中选择“Redis”进入“创建向导”

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_aws_redis_1.jpg" width="800" /></div>
>
> 上图是基本设置，关于高级设置
>
> * 安全组：与PostgreSQL选择一致
>
> * 启用自动备份：不勾选
>
> 填写完毕后，点击“创建”，大约需要几分钟时间来创建Redis集群

Redis集群创建好之后的详细信息如下图

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_aws_redis_2.jpg" width="800" /></div>

上图中的EndPoint用于配置Service使他们可以连接到Redis（在本书Demo中，是licensing-service，配置位于config-service的配置存储中）

## 12.3 在AWS上部署ELK（Elasticsearch，Logstash，Kibana）和Services

> 本小节将在AWS EC2 Instances（VM）上部署ELK，在AWS EKS容器中部署项目的各个Services
>

### 12.3.1 创建用于部署ELK Stack的EC2

执行以下步骤

Step 1. 在EC2 Console页面，点击“启动实例”按钮，进入EC2创建向导页面，在向导页面选择AMI （Amazon Machine Image）类型为"Amazon Linux 2 AMI"

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_elk_instance.jpg" width="800" /></div>
>

Step 2. 实例类型选择m4.large，因为它具有8GB内存，同时每小时0.01美分比较便宜

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_elk_instance_type.jpg" width="800" /></div>
>

Step 3 - 5.  `Configure Instance Details`, `Storage` and `Tags`，都选择默认配置

Step 6. 创建一个新的安全组配置、或选择已有的安全组配置

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_ec2_security_group.jpg" width="800" /></div>
>
> 本书Demo为了简单起见，配置了`0.0.0.0/0`容许来自整个Internet的请求，但实际应用中作者仍然建议仔细分析和指定入站规则

Step 7. 点击`Review and Launch`按钮，检查配置无误后开始创建EC2 Instance

Step 8. EC2 Instance创建好之后，需要创建秘钥，以便能够使用ssh、scp等命令访问EC2机器

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_ec2_security_group_create.jpg" width="800" /></div>
>
> 秘钥创建好之后，将该秘钥下载到本地

Step 9. 创建好的EC2 Instance如下图

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_ec2_dashboard.jpg" width="800" /></div>
>
> 其中的`IPv4 Public IP`就是用来访问EC2实例的IP地址

Step 10. 上传EC2和EKS部署脚本到EC2 Instance

> 创建的一个本地目录`~/project/folder/from/root`，目录内存放来[https://github.com/ihuaylupo/manning-smia/tree/master/chapter12/AWS/](https://github.com/ihuaylupo/manning-smia/tree/master/chapter12/AWS)的EC2及EKS部署脚本
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/
> $ mkdir -p ~/project/folder/from/root
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/
> $ cd ~/project/folder/from/root
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/project/folder/from/root/
> $ cp -r ~/Dev/git/manning-smia/chapter12/AWS/ ./AWS/
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/project/folder/from/root/
> $ find . -type f
> ./AWS/EKS/organizationservice-service.yaml
> ./AWS/EKS/organizationservice-deployment.yaml
> ./AWS/EKS/gatewayserver-service.yaml
> ./AWS/EKS/configserver-service.yaml
> ./AWS/EKS/eurekaserver-service.yaml
> ./AWS/EKS/eurekaserver-deployment.yaml
> ./AWS/EKS/gatewayserver-deployment.yaml
> ./AWS/EKS/postgres.yaml
> ./AWS/EKS/licensingservice-service.yaml
> ./AWS/EKS/authenticationservice-service.yaml
> ./AWS/EKS/licensingservice-deployment.yaml
> ./AWS/EKS/configserver-deployment.yaml
> ./AWS/EKS/authenticationservice-deployment.yaml
> ./AWS/jenkins_Setup.md
> ./AWS/EC2/config/logstash.conf
> ./AWS/EC2/docker-compose.yml
> ~~~
>
> 进入保存秘钥（Step 8创建）的目录，执行以下命令，将部署脚本上传到EC2 Instance
>
> ~~~bash
> # 将<permKey>`和`<IPv4>`替换成真实的值 
> chmod 400 <pemKey>.pem
> scp -r -i <pemKey>.pem ~/project/folder/from/root ec2-user@<IPv4>:~/
> ~~~
>
> `<pemKey>`：Step 8下载秘钥文件时的文件名
>
> `<IPv4>`：Step 9创建EC2 Instance之后Instance的IP地址
>
> `-i <pemKey>.pem`：scp命令将本地目录远程复制到EC2 Instance时、为该命令指定秘钥以建立ssh链接
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_loginto_ec2.jpg" width="800" /></div>

### 12.3.2 在EC2 instance上部署ELK  stack（ElasticSearch、LogStash、 Kibana）

使用下载下来的秘钥文件，可以登录到EC2 Instance

> ~~~bash
>ssh -i <pemKey>.pem ec2-user@<IPv4>
> ~~~

在EC2 Instance上执行以下命令，来部署ELK stack

> ~~~bash
> # 安装Docker
> sudo yum update
> sudo yum install docker
> # 安装Docker Compose
> sudo curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-`uname -s`-`uname -m` | sudo tee /usr/local/bin/docker-compose > /dev/null
> sudo chmod +x /usr/local/bin/docker-compose
> sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
> # 启动Docker守护进程
> sudo service docker start
> # 使用Docker Compose创建容器部署ELK Stack
> sudo docker-compose -f AWS/EC2/docker-compose.yml up -d
> ~~~

其中用到的docker-compose.yml的代码：

> [https://github.com/ihuaylupo/manning-smia/tree/master/chapter12/AWS/EC2](https://github.com/ihuaylupo/manning-smia/tree/master/chapter12/AWS/EC2)

创建完毕后，用浏览器访问`http://<IPv4>: 5601/`可以看到Kibana仪表盘

### 12.3.3 创建EKS Cluster并部署服务

EKS是AWS提供的Elastic Kubernetes Service的缩写，使用Kubernetes在及群众进行容器编排

本小节需要先配置AWS CLI，可参考一下文档

> [https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)

具体步骤如下

#### (1) 创建Kubernetes集群

Step 1. 创建 

> ~~~bash
> $ eksctl create cluster --name=ostock-dev-cluster --nodes=1 --node-type=m4.large
> ...
> [✓] EKS cluster "ostock-dev-cluster" in "region-code" region is ready
> ~~~
>
> `--name=ostock-dev-cluster`：集群名称
>
> `--nodes=1`：节点数量
>
> `--node-type=m4.large`：节点类型
>
> 关于EC2实例的价格，可访问：[https://aws.amazon.com/ec2/pricing/on-demand/](https://aws.amazon.com/ec2/pricing/on-demand/)

Step 2. 验证

> ~~~bash
> $ kubectl get svc
> NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
> kubernetes       ClusterIP    10.100.0.1   <none>        443/TCP   1m
> ~~~

#### (2) 将各服务的Docker Image推送到ECR

> ECR（Amazon Elastic Container Registry）与Docker Hub类似，是用于存储Docker Image的基础设置，更多内容可参考：[[https://docs.aws.amazon.com/AmazonECR/latest/userguide /what-is-ecr.html](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)]

Step 1. 生成Docker镜像

> ~~~bash
> $ mvn clean package dockerfile:build
> ~~~

Step 2. 查看已经生成的Docker镜像

> ~~~bash
> $ docker images
> REPOSITORY                       TAG        IMAGE ID         SIZE
> ostock/organization-service     chapter12   b1c7b262926e     485MB
> ostock/gatewayserver            chapter12   61c6fc020dcf     450MB
> ostock/configserver             chapter12   877c9d855d91     432MB
> ostock/licensing-service        chapter12   6a76bee3e40c     490MB
> ostock/authentication-service   chapter12   5e5e74f29c2      452MB
> ostock/eurekaserver             chapter12   e6bc59ae1d87     451MB
> ~~~

Step 3. 获取AWS账户ID和密码

> ~~~bash
> $ aws ecr get-login-password
> $ aws sts get-caller-identity --output text --query "Account"
> ~~~

Step 4. 使用获取的账户ID和密码来让Docker客户端登录

> ~~~bash
> # 将[password], [aws_account_id], [region]替换为真实值
> docker login -u AWS -p [password] https://[aws_account_id].dkr.ecr.[region].amazonaws.com
> ~~~

Step 5. 创建ECR Docker Image Repository

> ~~~bash
> $ aws ecr create-repository --repository-name ostock/configserver
> {
>     "repository": {
>         "repositoryArn": "arn:aws:ecr:us-east-2: 8193XXXXXXX43:repository/ostock/configserver",
>         "registryId": "8193XXXXXXX43",
>         "repositoryName": "ostock/configserver",
>         "repositoryUri": "8193XXXXXXX43.dkr.ecr.us-east-2.amazonaws.com/ostock/configserver",
>         "createdAt": "2020-06-18T11:53:06-06:00",
>         "imageTagMutability": "MUTABLE",
>         "imageScanningConfiguration": {
>             "scanOnPush": false
>         }
>     }
> }
> $ aws ecr create-repository --repository-name ostock/gatewayserver
> $ aws ecr create-repository --repository-name ostock/eurekaserver
> $ aws ecr create-repository --repository-name ostock/authentication-service
> $ aws ecr create-repository --repository-name ostock/licensing-service
> $ aws ecr create-repository --repository-name ostock/organization-service
> ~~~
>
> 后面五条命令的输出省略
>
> 确保将输出的`repositoryUri`记录下来

Step 6. 为镜像创建标签

> ~~~bash
> $ docker tag ostock/configserver:chapter12 [configserver-repository-uri]:chapter12
> $ docker tag ostock/gatewayserver:chapter12 [gatewayserver-repository-uri]:chapter12
> $ docker tag ostock/eurekaserver:chapter12 [eurekaserver-repository-uri]:chapter12
> $ docker tag ostock/authentication-service:chapter12 [authentication-service-repository-uri]:chapter12
> $ docker tag ostock/licensing-service:chapter12 [licensing-service-repository-uri]:chapter12
> docker tag ostock/organization-service:chapter12 [organization-service-repository-uri]:chapter12
> ~~~
>
> 该命令将为所有镜像创建一个新标签，如果此时执行docker images命令，输出类似下图
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_docker_images_in_aws_registry.jpg" width="800" /></di



Step 7. 将Docker Image推送到ECR Docker Image Repository

> ~~~bash
> # 将[*-repository-uri]替换为Step 5中ECR返回的值
> $ docker push [configserver-repository-uri]:chapter12
> $ docker push [gatewayserver-repository-uri]:chapter12
> $ docker push [eurekaserver-repository-uri]:chapter12
> $ docker push [authentication-service-repository-uri]:chapter12
> $ docker push [licensing-service-repository-uri]:chapter12
> $ docker push [organization-service-repository-uri]:chapter12
> ~~~

Step 8. 此时在ECR可以看到内容类似下图

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_elastic_container_registry_repositories.jpg" width="800" /></div>
>

#### (3) 在EKS集群部署微服务

##### Step 1：根据AWS环境更新config-server存储的服务配置

主要是各个Server的PostgreSQL、Redis、ELK Stack等配置

Note

> 本章代码（ [https://github.com/ihuaylupo/manning-smia/tree/master/chapter12/configserver/src/main/resources/bootstrap.yml](https://github.com/ihuaylupo/manning-smia/tree/master/chapter12/configserver/src/main/resources/bootstrap.yml)）中，config-server配置存储已经由前几章的本地文件存储改为了github存储，可以跟随该bootstrap.yml的配置内容，在[https://github.com/ihuaylupo/config](https://github.com/ihuaylupo/config)中找到对应与AWS环境中的配置文件

##### Step 2：部署Kafaka和Zookeeper

> 有多种方法创建，本书使用Helm Charts来（Helm是Kubernetes集群上的包管理器，一个Helm Chart是Helm 的一个包，提供了运行服务所需资源定义

安装Helm

> 参考[https://helm.sh/docs/intro/install/](https://helm.sh/docs/intro/install/)

安装Helm之后运行如下命令部署Zookeeper和Kafka

> ~~~bash
> helm install zookeeper bitnami/zookeeper \
>   --set replicaCount=1 \
>   --set auth.enabled=false \
>   --set allowAnonymousLogin=true
>  
> helm install kafka bitnami/kafka \
>   --set zookeeper.enabled=false \
>   --set replicaCount=1 \
>   --set externalZookeeper.servers=zookeeper
> ~~~

检查服务运行

> ~~~bash
> $ kubectl get pods
> NAME          READY   STATUS    RESTARTS   AGE
> kafka-0       1/1     Running   0          77s
> zookeeper-0   1/1     Running   0          101s
> ~~~

##### Step 3：将`docker-comose.yml`转换为兼容Kubernetes的文件

本章使用Kompose工具进行转换

> Kompose：是一个Docker的转换工具，可使用`kompose convert docker-compose.yml`命令来进行转换，使用`kompose up`命令来编排容器等
>
> 文档：[https://kompose.io/](https://kompose.io/)
>
> 本章Demo转换好的文件：存放在[https://github.com/ihuaylupo/manning-smia/tree/master/chapter12/AWS/EKS](https://github.com/ihuaylupo/manning-smia/tree/master/chapter12/AWS/EKS)
>
> ~~~bash
> __________________________________________________________________
> $ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter12/AWS/EKS/
> $ ls
> authenticationservice-deployment.yaml gatewayserver-service.yaml
> authenticationservice-service.yaml    licensingservice-deployment.yaml
> configserver-deployment.yaml          licensingservice-service.yaml
> configserver-service.yaml             organizationservice-deployment.yaml
> eurekaserver-deployment.yaml          organizationservice-service.yaml
> eurekaserver-service.yaml             postgres.yaml
> gatewayserver-deployment.yaml
> ~~~

Kubernetes的四种服务类型

> - `ClusterIP`：仅在集群内部可见的服务
>
> - `NodePort`：NodePort是一个静态端口值 ，服务在该端口上对外公开 
>
>     Kubernetes分配的端口范围默认是3000到32767，可使用`<service_name>-service.yaml`文件中`spec.containers.commands`部分中的`–-service-node-port-range`标志来更改
>
> - `LoadBalancer`：使用Cloud Load Balancer对外部公开的服务
>
> - `ExternalName`：服务被映射到这个`ExternalName`上
>
> 如果`<service_name>-service.yaml`文件中没有指定服务类型，则使用默认值`type=ClusterIP`

创建服务

> 使用AWS/ELK/下面这些用Kompose转换好的文件
>
> ~~~bash
> # 替换<service_name>为具体的服务名
> kubectl apply -f <service_name>-service.yaml,<service_name>-deployment.yaml
> ~~~
>
> 以下是configserver的例子
>
> ~~~bash
> kubectl apply -f configserver-service.yaml,configserver-deployment.yaml
> kubectl get pods
> # 替换<POD_NAME>为上一条命令的输出
> kubect logs <POD_NAME> --follow
> ~~~

向安全组添加规则，以容许所有来自NodePort的传入流量

> ~~~bash
> # 检索安全组ID
> aws ec2 describe-security-groups --filters Name=group-name,Values="*eksctl-ostock-dev-cluster-nodegroup*"  --query "SecurityGroups[*].{Name:GroupName,ID:GroupId}"
> # 创建入站规则（替换命令中的[security-group-id]为上一条命令输出的安全组ID）
> aws ec2 authorize-security-group-ingress --protocol tcp --port 31000 --group-id [security-group-id] --cidr 0.0.0.0/0
> ~~~

获取服务的对外IP

> 使用`kubectl get nodes -o wide`命令，输出类似下图
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_node_ext_ip_addr.jpg" width="800" /></div>
>

使用外部IP可以在浏览器中访问服务

> ~~~bash
> http:<node-external-ip>:<NodePort>/actuator
> ~~~

如果要删除Pod，可执行`kubectl delete <POD_NAME>`

如果要删除service, deployment，可执行`kubectl delete -f <service_name>-service.yaml <service_name>-deployment.yaml`

##### 后续步骤1：为PostgreSQL指定external name

> ~~~yaml
> apiVersion: v1
> kind: Service
> metadata:
> 	labels:
> 		app: postgres-service
> 	name: postgres-service
> spec:
> 	# 指定external name
> 	externalName: ostock-aws.cjuqpwnyahhy.us-east-2.rds.amazonaws.com
> 	selector:
> 		app: postgres-service
> 	type: ExternalName
> status:
> 	loadBalancer: {}
> ~~~
>
> 代码：[../chapter12/AWS/EKS/postgres.yaml](../chapter12/AWS/EKS/postgres.yaml)
>
> 执行以下命令以使修改生效
>
> ~~~bash
> kubectl apply -f postgres.yaml
> ~~~

##### 后续步骤2：创建`VPC peering`以及路由表以便让RDS和EKS集群可以互相通信

> 步骤参考：[https://dev.to/bensooraj/accessing-amazon-rds-from-aws-eks-2pc3](https://dev.to/bensooraj/accessing-amazon-rds-from-aws-eks-2pc3)
>
> VPC Peering介绍：[https://docs.aws.amazon.com/vpc/latest/peering/create-vpc-peering-connection.html](https://docs.aws.amazon.com/vpc/latest/peering/create-vpc-peering-connection.html)

## 12.4 `build and deployment pipeline`所用到的技术

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_tech_for_build.jpg" width="800" /></div>
>
> 对应图中的序号：
>
> 1. GitHub：[http://github.com](http://github.com)
> 2. Genkins：[https://www.jenkins.io/](https://www.jenkins.io/)
> 3. Maven/Spotify Docker Plugin（[https://github.com/spotify/dockerfile-maven](https://github.com/spotify/dockerfile-maven)）或Spring Boot Build Image Target（[https://spring.io/guides/gs/spring-boot-docker/](https://spring.io/guides/gs/spring-boot-docker/)）：用来通过mvn命令创建Docker镜像
> 4. Docker：[https://www.docker.com/](https://www.docker.com/)
> 5. Amazon Elastic Container Registry（https://aws.amazon.com/ecr/）：即之前提到的AWS ECR、用来在AWS上存储Docker镜像
> 6. AWS EKS：容器运行环境

## 12.5 Github和Jenkins

### (1) 在EKS中搭建Jenkins环境

> 设置Github和Jenkins之前，需要先搭建一套Jenkin环境，可以参考下面的文档
>
> [`../chapter12/AWS/jenkins_Setup.md`](../chapter12/AWS/jenkins_Setup.md)

### 12.5.1 设置Github

#### (1) 创建GitHub Webhook

> 在GitHub中创建一个Webhook，以便在进行代码推送时通知Jenkins启动特定的构建过程
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_git_setup.jpg" width="800" /></div>
>
> 对应上图，需要执行以下 步骤
>
> 1 . 进入项目 Repository的页面
>
> 2 . 点击顶部Setting标签
>
> 3 . 选择左侧边栏的Webhooksb标签
>
> 4 . 点击"Add Webhook"按钮
>
> 5 . 填入Payload URL
>
> 6 . 选择“Just the push event”选线
>
> 7 . 点击“Add Webhook”按钮

### 12.5.2 在Jenkins中构建Services

> 之前章节都是手动运行Maven脚本打包、手动执行Docker的Maven插件、手动运行docker-compose -up创建容器，这一小节使用Jenkins来自动执行这个过程

#### (1) 理解Pipeline运行过程

> 核心是JenkinsFile，当GitHub Repository上发生git push时，Webhook发送payload给Jenkins，Jenkins Job搜索JenkinsFile以启动构建过程
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_jenkins_setup_steps.jpg" width="800" /></div>
>
> 对应图中的序号，操作步骤如下：
>
> 1. 开发人员将项目代码修改push到git hub上
> 2. GitHub通过Webhook通知Jenkins，Jenkins从Github check out出源代码，以执行构建和部署过程
> 3. Jenkins设置用于构建的工具、环境变量等各种基本配置
> 4. Jenkins执行单元测试、集成测试，将项目构建打包成JAR文件
> 5. Jenkins执行maven docker-file插件以创建Docker镜像
> 6. Jenkins使用AWS ECR repository名称来给Docker镜像加上tag
> 7. Jenkins将镜像推送到具有相同tag的ECR repository
> 8. 连接到EKS集群，使用service.yaml和deployment.yaml文件部署服务
>
> 接下来介绍如何创建Jenkins Pipeline来让Jenkins执行这些步骤

#### (2) 安装Jenkins插件

需要如下插件

> GitHub Integration，GitHub，Maven Invoker，Maven Integration，Docker Pipeline，ECR，Kubernetes Continuous deploy，Kubernetes CLI

安装方法需参考Jenkins官方文档，例如

> Kubernetes CLI：[https://plugins.jenkins.io/kubernetes-cli/](https://plugins.jenkins.io/kubernetes-cli/)

#### (3) 创建Kubernetes Credential和ECR Credential

> 用来让Jenkins可以访问Kubernetes和ECR

##### (a) 创建Kubernetes Credential

> 页面：Manage Jenkins → Manage Credentials → Jenkins → Global Credentials
>
> 随后单击Add Credentials，选择Kubernetes配置（kubeconfig）并使用Kubernetes集群信息来填写

获取Kubernetes集群信息

> ~~~bash
> kubectl config view
> ~~~

使用获取的信息填写表单，类似下图

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_eks_kubeconfig.jpg" width="800" /></div>
>

##### (b) 创建ECR Credential

###### 在AWS  IAM页面上创建用户并下载Credential

> 在AWS Console中找到IAM页面，在页面中找到Users Tab并点击“Add User”。
>
> 在用户创建页面输入username、access type
>
> ~~~properties
> User name: ecr-user
> Access type: Programmatic access
> ~~~
>
> 接下来的页面设置user policy
>
> * 选择“attach existing policies directly”
> * 搜索“AmazonEC2ContainerRegistryFullAccess”并将其选中
>
> 最后的页面上单击“下载csv”，得到Credential文件并保存在本地
>
> 点击保存按钮

###### 在Jenkins中添加Credential

> 首先确保已经在Jenkins Consols中安装了ECR Pluging
>
> 跳转到如下页面：`Manage Jenkins` → ` Manage Credentials` → `Jenkins` → `Global Configuration` 
>
> 点击`add credentials`，在页面中填入如下数据（将后两项替换成csv Credential 文件中的内容）
>
> ~~~properties
> ID: ecr-user
> Description: ECR User
> Access Key ID: <Access_key_id_from_csv>
> Secret Access Key: <Secret_access_key_from_csv>
> ~~~

#### (4) 创建Jenkins Pipeline

在Jenkins Dashboard点击屏幕左上方的New Item按钮，进入Item创建页面（如下图），点击“Pipeline”

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_jenkins_pipeline.jpg" width="650" /></div>

填入git hub repository的URL，选择GITSccm Polling来轮询Github Webhook触发器（设置次选项之后，上一节配置的Github Webhook将能够在代码push时通知Jenkins）

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_github_webhook.jpg" width="650" /></div>

最后一步，从SCM选择Pipeline Script

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_gitbut_webhok_in_jenkins_pipeline.jpg" width="800" /></div>
>
> 在页面中依次选择：Defination：Pipeline script from SCM → SCM：Git
>
> 然后依次填入：
>
> * Git Repository URL
> * Git Repository Branch Name
> * Jenkinsfile的Script Path

下一小节介绍如何编写Jenkinsfile

### 12.5.3 理解和编写pipeline script

> Jenkinsfile以stage为单位进行组织，下面是一个例子
>
> ~~~javascript
> node {
> 	// 定义变量，该变量在阶段1被赋值，在之后的阶段中使用
> 	def mvnHome
> 
> 	// 1 准备阶段
> 	stage('Preparation') {
> 		// 添加git repo的配置，这样后续步骤中就不需要手动重复传入git repo url了
> 		git 'https://github.com/ihuaylupo/spmia.git'
> 		// 将maven的安装路径赋值给变量mvnHome，以方便各个阶段mvn命令的使用
> 		mvnHome = tool 'M2_HOME'
> 	}
> 
> 	// 2 构建阶段
> 	stage('Build') {
> 		// 执行maven clean package命令
> 		withEnv(["MVN_HOME=$mvnHome"]) {
> 			if (isUnix()) {
> 				sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
> 			} else {
> 				bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
> 			}
> 		}
> 	}
> 
> 	// 3 构建结果处理阶段
> 	stage('Results') {
> 		// 获取Junit测试结果
> 		junit '**/target/surefire-reports/TEST-*.xml'
> 		// 打包*jar文件用于创建Docker Image
> 		archiveArtifacts 'configserver/target/*.jar'
> 	}
> 
>     // 4 创建Docker镜像阶段
> 	stage('Build image') {
> 		// 创建镜像，同时使用来自ECR repository的信息给Docker镜像增加tag        
> 		// 执行maven spotify-dockerfile plugin的mvn dockerfile:build命令，在pom.xml可看到该插件的信息
>         // * -Ddocker.image.prefix : 设置docker image前缀
>         // * -Dproject.artifactId=configserver : 设置artifact名称
>         // * -Ddocker.image.version : 设置artifact版本
>         // 另外这这个阶段可以设置使用的配置文件（Dev、Staging、Prod）
>         // 也可以指定不同的version参数创建不同版本的镜像
> 		sh "'${mvnHome}/bin/mvn' -Ddocker.image.prefix=8193XXXXXXX43.dkr.ecr.us-east-2.amazonaws.com/ostock -Dproject.artifactId=configserver -Ddocker.image.version=latest dockerfile:build"
> 	}
> 
> 	// 5 Push Docker阶段
> 	stage('Push image') {
> 		// 将创建的Docker镜像Push到之前创建的AWS ECR repository
> 		docker.withRegistry('https://8193XXXXXX43.dkr.ecr.us-east-2.amazonaws.com', 'ecr:us-east-2:ecr-user') {
> 			sh "docker push 8193XXXXXX43.dkr.ecr.us-east-2.amazonaws.com/ostock/configserver:latest"
> 		}
> 	}
> 
> 	// 6 Kubernetes构建阶段
> 	stage('Kubernetes deploy') {
> 		// 这个阶段的命令根据不同的配置，会有较大的差距
>         // 下一小节介绍如何生成这条命令
> 		kubernetesDeploy configs: 'configserver-deployment.yaml', kubeConfig: [path: ''], kubeconfigId: 'kubeconfig', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
> 	}
> }
> ~~~

### 12.5.4 生成kubernetes deploy阶段的命令脚本

> 使用Jenkins的Pipeline Syntax Option来生成上一小节Kubenetes构建阶段的命令
>
> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch12_kubernetes_pipeline.jpg" width="800" /></div>
>
> 具体步骤如下：
>
> 1. 在Jenkins Dashboard页面点击摘要
> 2. 点击左侧的Pipeline Syntax Option
> 3. 在Snippet Generator页面，点击”Sample Step“下拉列表，并选择`kubernetesDeploy:Deploy to Kubernetes`选项
> 4. 在Kubeconfig下拉列表中选择在之前步骤中创建的Kubernetes Credentials
> 5. 在Config Files下拉列表中选择`<server-name>-deployment.yaml`，这个文件在git repo中可以看到
> 6. 其他变量使用默认值
> 7. 点击generate pipeline script，页面显示自动生成的脚本代码
>
> 说明：处于演示需要，本书将所有微服务都塞在一个git repo中，实际项目中建议为每个微服务使用独立的git repo和构建过程

## 12.6 关于本书Demo构建过程的说明

> 1. 本书Demo做了简化，实际应用中会由DevOPs Team将构建过程分解成独立的步骤（compile → package → deploy → test），并由Dev Team将它们的微服务构建脚本hook到构建过程中
> 2. 本书Demo对VM环境设置进行了简化，这类设置（VM或Container中OS的设置）可以通过[Ansible](https://github.com/ansible/ansible)、[Puppet](https:// github.com/puppetlabs/puppet)、[Chef](https://github.com/chef/chef)等工具来完成
> 3. 本书Demo把整套系统部署在了一台Server上，实际应用中，每个微服务会有他们自己的构建脚本，并且会彼此独立地部署在一个Cluster EKS Container中

## 12.7 小结

> 略





