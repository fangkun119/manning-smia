<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [CH04 使用Docker](#ch04-%E4%BD%BF%E7%94%A8docker)
  - [4.1 容器还是虚拟机](#41-%E5%AE%B9%E5%99%A8%E8%BF%98%E6%98%AF%E8%99%9A%E6%8B%9F%E6%9C%BA)
  - [4.2 Docker](#42-docker)
  - [4.3 Dockerfile](#43-dockerfile)
    - [4.3.1 Dockerfile命令](#431-dockerfile%E5%91%BD%E4%BB%A4)
  - [4.4 Docker compose](#44-docker-compose)
    - [4.4.1 docker-compose使用](#441-docker-compose%E4%BD%BF%E7%94%A8)
    - [4.4.2 docker-compose.yml示例](#442-docker-composeyml%E7%A4%BA%E4%BE%8B)
    - [4.4.3 docker-compose命令](#443-docker-compose%E5%91%BD%E4%BB%A4)
  - [4.5. Docker与微服务集成](#45-docker%E4%B8%8E%E5%BE%AE%E6%9C%8D%E5%8A%A1%E9%9B%86%E6%88%90)
    - [4.5.1 构建Docker镜像](#451-%E6%9E%84%E5%BB%BAdocker%E9%95%9C%E5%83%8F)
      - [(1) 导入Maven插件](#1-%E5%AF%BC%E5%85%A5maven%E6%8F%92%E4%BB%B6)
      - [(2) 简单构建（Basic Dockerfile）](#2-%E7%AE%80%E5%8D%95%E6%9E%84%E5%BB%BAbasic-dockerfile)
      - [(3) 多级构建（Multi-stage Dockerfile）](#3-%E5%A4%9A%E7%BA%A7%E6%9E%84%E5%BB%BAmulti-stage-dockerfile)
    - [4.5.2 使用Spring Boot插件创建Docker镜像](#452-%E4%BD%BF%E7%94%A8spring-boot%E6%8F%92%E4%BB%B6%E5%88%9B%E5%BB%BAdocker%E9%95%9C%E5%83%8F)
      - [(1) `Cloud Native Buildpacks`构建工具](#1-cloud-native-buildpacks%E6%9E%84%E5%BB%BA%E5%B7%A5%E5%85%B7)
        - [添加插件](#%E6%B7%BB%E5%8A%A0%E6%8F%92%E4%BB%B6)
        - [构建docker镜像并创建容器](#%E6%9E%84%E5%BB%BAdocker%E9%95%9C%E5%83%8F%E5%B9%B6%E5%88%9B%E5%BB%BA%E5%AE%B9%E5%99%A8)
      - [(2) 分层JAR（`LAYERED_JAR`）](#2-%E5%88%86%E5%B1%82jarlayered_jar)
        - [将层配置添加到pom.xml](#%E5%B0%86%E5%B1%82%E9%85%8D%E7%BD%AE%E6%B7%BB%E5%8A%A0%E5%88%B0pomxml)
        - [打包应用程序](#%E6%89%93%E5%8C%85%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F)
        - [获得该应用的分层信息](#%E8%8E%B7%E5%BE%97%E8%AF%A5%E5%BA%94%E7%94%A8%E7%9A%84%E5%88%86%E5%B1%82%E4%BF%A1%E6%81%AF)
        - [使用分层信息编写Dockerfile](#%E4%BD%BF%E7%94%A8%E5%88%86%E5%B1%82%E4%BF%A1%E6%81%AF%E7%BC%96%E5%86%99dockerfile)
        - [创建镜像、并创建容器](#%E5%88%9B%E5%BB%BA%E9%95%9C%E5%83%8F%E5%B9%B6%E5%88%9B%E5%BB%BA%E5%AE%B9%E5%99%A8)
    - [4.5.3 使用docker-compose启动服务](#453-%E4%BD%BF%E7%94%A8docker-compose%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1)
  - [4.6 小结](#46-%E5%B0%8F%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

#  CH04 使用Docker

> 作用、容器组件、容器与微服务集成
>
> 更多参考：《Docker in Action》

## 4.1 容器还是虚拟机

容器和虚拟机之间的对比

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch04_vm_cmp_container.jpg" width="800" /></div>

在使用方面

> * 虚拟机中、必须预先设置所需的物理资源；容器中，资源设置不是必须的、可以交给容器引擎来分配
> * 容器不需要完整的OS，可以重用宿主机的OS，更轻量级

容器给微服务带来的好处：

> 便携性，一致的环境，更快地启动和停止，支撑更多的程序运行

## 4.2 Docker

Docker引擎服务器，Rest API，命令行界面CLI

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/sima2_ch04_docker_arch.jpg" width="800" /></div>

Docker注册表（Docker Hub或者私有注册表），镜像 （image），容器（container），Docker Volumns，Docker Networks

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch04_docker_cli.jpg" width="800" /></div>

## 4.3 Dockerfile

例子

> ~~~dockerfile
> FROM openjdk:11-slim
> ARG JAR_FILE=target/*.jar
> COPY ${JAR_FILE} app.jar
> ENTRYPOINT ["java","-jar","/app.jar"]
> ~~~

Docker镜像创建

> <div align="left"><img src="https://raw.githubusercontent.com/kenfang119/pics/main/microservice/smia2_ch04_image_creation.jpg" width="800" /></div>

### 4.3.1 Dockerfile命令

编写Dockerfile时常用的命令

> `FROM`，`LABEL`，`MAINTAINER`，`WORKDIR`，`ADD`，`COPY`，`ENV`，`EXPOSE`，`ARG`，`VOLUMN`，`RUN`，`ENTRYPOINT`，`CMD`
>
> 说明：[https://github.com/fangkun119/java_proj_ref/blob/master/101_docker/note/03_docker_file.md](https://github.com/fangkun119/java_proj_ref/blob/master/101_docker/note/03_docker_file.md)

Dockerfile文档：[https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)

## 4.4 Docker compose

### 4.4.1 docker-compose使用

docker-compose用于在单台数组机上编排容器：创建镜像、容器并使容器之间可以互联互通

docker-compose笔记

> [https://github.com/fangkun119/java_proj_ref/blob/master/101_docker/note/05_docker_compose.md](https://github.com/fangkun119/java_proj_ref/blob/master/101_docker/note/05_docker_compose.md)
>
> 安装、docker-compose.yml编写、镜像和容器构建、让容器间互联互通、docker-compose命令

docker-compose文档 

> [https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)

### 4.4.2 docker-compose.yml示例

> 下面是一个例子，`<>`内的内容替换成实际需要的值
>
> ~~~yml
> # docker-compose版本
> version: <docker-compose-version> 
> # 要部署的服务
> services:
>     # 服务名
>     database:
>         # 该服务所使用的docker镜像
>         image: <database-docker-image-name>
>         # 端口映射
>         ports:
>             - "<databasePort>:<databasePort>"
>         # 环境变量
>         environment:
>             POSTGRES_USER: <databaseUser>
>             POSTGRES_PASSWORD: <databasePassword>
>             POSTGRES_DB:<databaseName>
>     # 服务名
>     <service-name>:
>         # 该服务所使用的docker镜像
>         image: <service-docker-image-name>
>         # 端口映射
>         ports:
>             - "<applicationPort>:<applicationPort>"
>         # 环境变量
>         environment:
>             PROFILE: <profile-name>
>             DATABASESERVER_PORT: "<databasePort>"
>         # 指定容器名，而不是使用默认值
>         container_name: <container_name>
>         # 使用名为backend的网络
>         networks:
>             backend:
>                 # backend网络中其他的服务可以使用别名“alias”来访问该服务
>                 aliases:
>                     - "alias"
> 
> # 创建以后基于bridge的网络，名为backend
> networks:
>     backend:
>         driver: bridge
> ~~~

### 4.4.3 docker-compose命令

> `docker-compose up -d`，`docker-compose logs`，`docker-compose logs ${service_id}`，`docker-compose ps`，`docker-compose stop`，`docker-compose down`

## 4.5. Docker与微服务集成

> Demo项目：[../chapter4/licensing-service/](../chapter4/licensing-service/)
>
> * [/pom.xml](../chapter4/licensing-service/pom.xml)
> * [/docker-compose.yml](../chapter4/licensing-service/docker-compose.yml)
> * [/Dockerfile](../chapter4/licensing-service/Dockerfile)
>
> 构建命令：[../chapter4/README.md](../chapter4/README.md)
>
> ~~~bash
> # Clone this repository
> \$ git clone https://github.com/ihuaylupo/manning-smia
> 
> # Go into the repository
> \$ cd chapter4/licensing-service
> 
> # To build the code examples for Chapter 4 as a docker image, open a command-line 
> # window and execute the following command:
> \$ mvn clean package dockerfile:build
> 
> # Run the app using the docker-compose up command. Remember, you need to execute
> #the command on the root folder of the project where the docker-compose.yml is.
> \$ docker-compose up
> ~~~

### 4.5.1 构建Docker镜像

#### (1) 导入Maven插件

在[pom.xml](../chapter4/licensing-service/pom.xml)中添加`dockerfile-maven-plugin`

> ~~~xml
> <properties>
> 	...
> 	<docker.image.prefix>ostock</docker.image.prefix>
> </properties>
> 
> ...
> 
> <build>
> 	<plugins>
> 		...
>         <!-- This plugin is used to create a docker image and publish it to docker hub-->
> 		<plugin>
> 			<groupId>com.spotify</groupId>
> 			<artifactId>dockerfile-maven-plugin</artifactId>
> 			<version>1.4.13</version>
> 			<configuration>
>                 <!-- docker.image.prefix是在<properties>中定义 -->
> 				<repository>${docker.image.prefix}/${project.artifactId}</repository>
> 				<tag>${project.version}</tag>
> 				<buildArgs>
>                     <!-- Dockerfile中的ARG JAR_FILE命令会取到这个参数的值 -->
> 					<JAR_FILE>target/${project.build.finalName}.jar</JAR_FILE>
> 				</buildArgs>
> 			</configuration>
> 			<executions>
> 				<execution>
> 					<id>default</id>
> 					<phase>install</phase>
> 					<goals>
> 						<goal>build</goal>
> 						<goal>push</goal>
> 					</goals>
> 				</execution>
> 			</executions>
> 		</plugin>
> 	</plugins>
> </build>
> ~~~

#### (2) 简单构建（Basic Dockerfile）

> 将所有内容打包在Spring Boot Jar包中，拷贝到容器内运行
>
> ~~~dockerfile
> # Start with a base image containing Java runtime
> FROM openjdk:11-slim
>  
> # Add Maintainer Info
> LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"
>  
> # The application's jar file
> ARG JAR_FILE
>  
> # Add the application's jar to the container
> COPY ${JAR_FILE} app.jar
>  
> # execute the application
> ENTRYPOINT ["java","-jar","/app.jar"]
> ~~~

#### (3) 多级构建（Multi-stage Dockerfile）

用途：使Docker分层中、每一层尽可能小

> * Dockerfile中每条指令都会在镜像上添加一层，需要在移往下一层之前清除不需要artifact
> * 通常需要一个用于开发的Dockerfile（包含所有内容），一个用于生产环境（只保留精简后的内容），但是维护两个Dockerfile不何时，因此实际的做法是：(1) `Dockerfile.build`

详细说明文档：[https://docs.docker.com/develop/develop-images/multistage-build/](https://docs.docker.com/develop/develop-images/multistage-build/)

本章的例子：

>Dockerfile: [../chapter4/licensing-service/Dockerfile](../chapter4/licensing-service/Dockerfile)
>
>~~~dockerfile
># 每个FROM指令会指定一个新的层
>
># 阶段1：
># 阶段1命名为build，可以被后续的FROM命令使用，以作为另一个阶段的基础
>FROM openjdk:11-slim as build 
>
>LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"
>ARG JAR_FILE
># ${JAR_FILE}
># * 设置在pom.xml的<configuration><buildArgs>标签中
># * 在执行mvn clean package时生成
>COPY ${JAR_FILE} app.jar
># 将多个命令压缩成一个、避免创建不必要的层
>RUN mkdir -p target/dependency && (cd target/dependency; jar -xf /app.jar)
>
># 阶段2：不会包含app.jar，但是会包含从app.jar中解压出来只用于程序运行的部分
># 每个阶段可以选择不同的基础，例如FROM builder
># 也可以有选择地将artifact从一个阶段复制到另一个阶段，例如下面的COPY命令
>FROM openjdk:11-slim 
>VOLUME /tmp   # 创建容器时将/tmp映射到宿主机随机命名的目录中，用docker inspect命令可查看详情
>
># --from=build表示从名为build的阶段中拷贝文件
>ARG DEPENDENCY=/target/dependency
>COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
>COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
>COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app
>
>ENTRYPOINT ["java","-cp","app:app/lib/*","com.optimagrowth.license.LicenseServiceApplication"]
>~~~
>
>生成jar包：`mvn clean package`
>
>测试Docker镜像创建：`mvn package dockerfile:build`（需要关闭VPN解除对80端口的拦截）
>
>~~~bash
>[INFO] Scanning for projects...
>[INFO] 
>[INFO] -----------------< com.optimagrowth:licensing-service >-----------------
>[INFO] Building License Service 0.0.1-SNAPSHOT
>[INFO] --------------------------------[ jar ]---------------------------------
>[INFO] 
>[INFO] --- dockerfile-maven-plugin:1.4.13:build (default-cli) @ licensing-service ---
>[INFO] dockerfile: null
>[INFO] contextDirectory: /Users/fangkun/Dev/git/manning-smia/chapter4/licensing-service
>[INFO] Building Docker context /Users/fangkun/Dev/git/manning-smia/chapter4/licensing-service
>[INFO] Path(dockerfile): null
>[INFO] Path(contextDirectory): /Users/fangkun/Dev/git/manning-smia/chapter4/licensing-service
>[INFO] 
>[INFO] Image will be built as ostock/licensing-service:0.0.1-SNAPSHOT
>[INFO] 
>[INFO] Step 1/12 : FROM openjdk:11-slim as build
>[INFO] 
>[INFO] Pulling from library/openjdk
>[INFO] Digest: sha256:7b562ad0bd7d193b2e33562fc8337412813768ba880afa7a88ad3a32b6e46ffc
>[INFO] Status: Image is up to date for openjdk:11-slim
>[INFO]  ---> f0cb34b07f7d
>[INFO] Step 2/12 : LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"
>[INFO] 
>[INFO]  ---> Using cache
>[INFO]  ---> 6a692f776854
>[INFO] Step 3/12 : ARG JAR_FILE
>[INFO] 
>[INFO]  ---> Using cache
>[INFO]  ---> c9cb68566027
>[INFO] Step 4/12 : COPY ${JAR_FILE} app.jar
>[INFO] 
>[INFO]  ---> 0dc58ad208f8
>[INFO] Step 5/12 : RUN mkdir -p target/dependency && (cd target/dependency; jar -xf /app.jar)
>[INFO] 
>[INFO]  ---> Running in 3ba6ad9344f5
>[INFO] Removing intermediate container 3ba6ad9344f5
>[INFO]  ---> 1f5aca189221
>[INFO] Step 6/12 : FROM openjdk:11-slim
>[INFO] 
>[INFO] Pulling from library/openjdk
>[INFO] Digest: sha256:7b562ad0bd7d193b2e33562fc8337412813768ba880afa7a88ad3a32b6e46ffc
>[INFO] Status: Image is up to date for openjdk:11-slim
>[INFO]  ---> f0cb34b07f7d
>[INFO] Step 7/12 : VOLUME /tmp
>[INFO] 
>[INFO]  ---> Running in c3d9c1c67fc1
>[INFO] Removing intermediate container c3d9c1c67fc1
>[INFO]  ---> 15d917cd28e7
>[INFO] Step 8/12 : ARG DEPENDENCY=/target/dependency
>[INFO] 
>[INFO]  ---> Running in 40739a0ec92d
>[INFO] Removing intermediate container 40739a0ec92d
>[INFO]  ---> acd72027e8c3
>[INFO] Step 9/12 : COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
>[INFO] 
>[INFO]  ---> cc2d606266e5
>[INFO] Step 10/12 : COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
>[INFO] 
>[INFO]  ---> 6643fa1593e1
>[INFO] Step 11/12 : COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app
>[INFO] 
>[INFO]  ---> dcca9ca7d2d1
>[INFO] Step 12/12 : ENTRYPOINT ["java","-cp","app:app/lib/*","com.optimagrowth.license.LicenseServiceApplication"]
>[INFO] 
>[INFO]  ---> Running in 1d8f5ab13677
>[INFO] Removing intermediate container 1d8f5ab13677
>[INFO]  ---> 3a3e21cc78fe
>[INFO] Successfully built 3a3e21cc78fe
>[INFO] Successfully tagged ostock/licensing-service:0.0.1-SNAPSHOT
>[INFO] 
>[INFO] Detected build of image with id 3a3e21cc78fe
>[INFO] Building jar: /Users/fangkun/Dev/git/manning-smia/chapter4/licensing-service/target/licensing-service-0.0.1-SNAPSHOT-docker-info.jar
>[INFO] Successfully built ostock/licensing-service:0.0.1-SNAPSHOT
>[INFO] ------------------------------------------------------------------------
>[INFO] BUILD SUCCESS
>[INFO] ------------------------------------------------------------------------
>[INFO] Total time:  22.515 s
>[INFO] ------------------------------------------------------------------------
>Process finished with exit code 0
>~~~
>
>查看生成的docker镜像
>
>~~~bash
>__________________________________________________________________
>$ /fangkundeMacBook-Pro/ fangkun@fangkundeMacBook-Pro.local:~/Dev/git/manning-smia/chapter4/
>$ docker images  | grep ostock
>ostock/licensing-service       0.0.1-SNAPSHOT   3a3e21cc78fe   About a minute ago   443MB
>~~~
>
>一些与创建docker容器相关的命令
>
>~~~bash
># 创建容器、启动应用程序
>docker run ostock/licensing-service:0.0.1-SNAPSHOT
># 查看运行中的容器
>docker ps
># 停止容器
>docker stop <container_id>
># 删除容器
>docker rm <container_id>
># 删除镜像
>dockre rmi ostock/licensing-service:0.0.1-SNAPSHOT
>~~~

### 4.5.2 使用Spring Boot插件创建Docker镜像 

> Spring Boot 2.3增加了创建Docker镜像的功能，本节演示使用方法
>
> 依赖：
>
> 1. 安装了docker和docker-compose
> 2. SpringBoot的版本高于2.3（[pom.xml](../chapter4/licensing-service/pom.xml)）
>
> ~~~xml
> <parent>
> 	<groupId>org.springframework.boot</groupId>
> 	<artifactId>spring-boot-starter-parent</artifactId>
> 	<version>2.4.0</version>
> 	<relativePath/> <!-- lookup parent from repository -->
> </parent>
> ~~~

#### (1) `Cloud Native Buildpacks`构建工具

> Spring Boot 2.3.0引入，用来支持Docker镜像构建（用demo实验过程中发现[2.3.0-RELEASE会构建失败](https://stackoverflow.com/questions/61833679/spring-boot-2-3-build-image-failed)，更新到2.4.1之后问题解决）

##### 添加插件

Maven：添加[Spring Boot Maven插件](https://docs.spring.io/spring-boot/docs/2.3.0.M1/maven-plugin/html/)（[pom.xml](../chapter4/licensing-service/pom.xml)）

> ~~~xml
> <properties>
> 	...
> 	<docker.image.prefix>ostock</docker.image.prefix>
> </properties>
> 
> ...
> 
> <build>
>     <plugins>
>         <plugin>
>             <groupId>org.springframework.boot</groupId>
>             <artifactId>spring-boot-maven-plugin</artifactId>
>             <configuration>
>                 <!-- 可选指定镜像的名称 -->
>                 <!--
>                 <image>
>                     <name>${docker.image.prefix}/${project.artifactId}:latest</name>
>                 </image>
>                 -->
>             </configuration>
>         </plugin>
> 	</plugins>
> </build>
> ~~~

Gradle：添加[Spring Boot Gradle插件](https://docs.spring.io/spring-boot/docs/2.3.0.M1/gradle-plugin/reference/html/)

##### 构建docker镜像并创建容器

> `./mvnw clean package`
>
> `./mvnw spring-boot:build-image` (需要关闭VPN)
>
> ~~~bash
> [INFO] Scanning for projects...
> [INFO] 
> [INFO] -----------------< com.optimagrowth:licensing-service >-----------------
> [INFO] Building License Service 0.0.1-SNAPSHOT
> [INFO] --------------------------------[ jar ]---------------------------------
> [INFO] 
> ...
> [INFO] --- spring-boot-maven-plugin:2.4.1:build-image (default-cli) @ licensing-service ---
> [INFO] Building image 'docker.io/library/licensing-service:0.0.1-SNAPSHOT'
> [INFO] 
> [INFO]  > Pulling builder image 'docker.io/paketobuildpacks/builder:base' 0%
> [INFO]  > Pulling builder image 'docker.io/paketobuildpacks/builder:base' 1%
> ...
> [INFO]  > Pulled builder image 'paketobuildpacks/builder@sha256:a14f7b512128798290d6269d9ac3c6a05d16bb2705041e7f18eef4f3f55dee1d'
> [INFO]  > Pulling run image 'docker.io/paketobuildpacks/run:base-cnb' 0%
> [INFO]  > Pulling run image 'docker.io/paketobuildpacks/run:base-cnb' 7%
> ...
> [INFO]     [creator]     Setting default process type 'web'
> [INFO]     [creator]     *** Images (045116f040d2):
> [INFO]     [creator]           docker.io/library/licensing-service:0.0.1-SNAPSHOT
> [INFO]
> [INFO] Successfully built image 'docker.io/library/licensing-service:0.0.1-SNAPSHOT'
> ~~~
>
> 最后一行日志会输出构建出的镜像名称，以下命令可以创建容器
>
> ~~~bash
> docker run -it -p8080:8080 docker.io/library/licensing-service:0.0.1-SNAPSHOT
> ~~~

#### (2) 分层JAR（`LAYERED_JAR`）

> Spring Boot引入的新的Jar包布局，在该布局中，lib和classes文件被拆分到不同的层（layer）中，是`Buildpacks`之外的另一个选择，使用步骤如下：
>
> 1. 将配置添加到pom.xml文件中。
> 2. 打包应用程序。
> 3. 获得该应用的分层信息。
> 4. 使用分层信息编写Dockerfile。
> 5. 创建镜像、并创建容器

##### 将层配置添加到pom.xml

> ~~~xml
> <plugin>
> 	<groupId>org.springframework.boot</groupId>
> 	<artifactId>spring-boot-maven-plugin</artifactId>
> 	<configuration>
> 		<layers>
> 			<enabled>true</enabled>
> 		</layers>
> 	</configuration>
> </plugin>
> ~~~

##### 打包应用程序

> ~~~bash
> mvn clean package
> ~~~

#####  获得该应用的分层信息

> 在应用程序根目录下执行，以显示layer并将其添加到Dockerfile中
>
> ~~~bash
> $ java -Djarmode=layertools -jar target/licensing-service-0.0.1-SNAPSHOT.jar list
> dependencies
> spring-boot-loader
> snapshot-dependencies
> application
> ~~~

##### 使用分层信息编写Dockerfile

>  ~~~dockerfile
> FROM openjdk:11-slim as build
> WORKDIR application
> ARG JAR_FILE=target/*.jar
> COPY ${JAR_FILE} application.jar
> RUN java -Djarmode=layertools -jar application.jar extract
>  
> FROM openjdk:11-slim
> WORKDIR application
> COPY --from=build application/dependencies/ ./
> COPY --from=build application/spring-boot-loader/ ./
> COPY --from=build application/snapshot-dependencies/ ./
> COPY --from=build application/application/ ./
> ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
>  ~~~

##### 创建镜像、并创建容器

> ~~~bash
> docker build . --tag licensing-service
> docker run -it -p8080:8080 licensing-service:latest
> ~~~

### 4.5.3 使用docker-compose启动服务

Demo：[/docker-compose.yml](../chapter4/licensing-service/docker-compose.yml)

> ~~~yml
> version: '3.7'
> 
> services:
> 	licensingservice:
> 		image: ostock/licensing-service:0.0.1-SNAPSHOT
> 		ports:
> 			- "8080:8080"
> 		environment:
> 			- "SPRING_PROFILES_ACTIVE=dev"
> 		networks:
> 			backend:
> 				aliases:
> 					- "licenseservice"
> networks:
> 	backend:
> 		driver: bridge
> ~~~
>
> ~~~bash
> # 创建镜像和容器
> docker-compose up
> ~~~
>
> 各行配置的含义，及docker-compose命令参考`4.4`小节

## 4.6 小结 

> 略