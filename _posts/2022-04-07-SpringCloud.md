---
layout: post
#标题配置
title: SpringCloud
#时间配置
date:   2022-04-07 01:18:00 +0800
#目录配置
categories: 微服务
#标签配置
tag: 学习笔记
---

* content
{:toc}



# SpringCloud

## 1. 初识 Spring Cloud

### 1.1 微服务架构

+ 架构演进

![](https://img-blog.csdnimg.cn/ca3aeff8df5a4dc5bdf4a2aa7bcbc5b0.png)

+ "微服务”一词源于 Martin Fowler的名为 Microservices 的博文,可以在他的官方博客上找到http://martinfowler.com/articles/microservices.html

+ 微服务是系统架构上的一种设计风格,它的主旨是将一个原本独立的系统拆分成多个小型服务,这些小型服务都在各自独立的进程中运行,服务之间一般通过 HTTP 的 RESTfuL API 进行通信协作。

+ 被拆分成的每一个小型服务都围绕着系统中的某一项或些耦合度较高的业务功能进行构建,并且每个服务都维护着自 身的数据存储、业务开发自动化测试案例以及独立部署机制。

+ 由于有了轻量级的通信协作基础,所以这些微服务可以使用不同的语言来编写。 
+ 微服务架构例图

![](https://img-blog.csdnimg.cn/31f4f39425e74c018565c4d3355ad174.png)





### 1.2 走进 Spring Cloud

![](https://img-blog.csdnimg.cn/d2ca62934ccc4f03a1d4c83fa20da236.png)

+ Spring Cloud 是一系列框架的有序集合。

+ Spring Cloud 并没有重复制造轮子，它只是将目前各家公司开发的比较成熟、经得起实际考验的服务框架组合起来。

+ 通过 Spring Boot 风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。

+ 它利用Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、 断路器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。

+ Spring Cloud项目官方网址：https://spring.io/projects/spring-cloud 
+ Spring Cloud 版本命名方式采用了伦敦地铁站的名称，同时根据字母表的顺序来对应版本时间顺序，比如：最早的Release版本：Angel，第二个Release版本：Brixton，然后是Camden、Dalston、Edgware，Finchley，Greenwich，Hoxton。
+ 目前最新的是Hoxton版本。

| Release Train | Boot Version |
| ------------- | ------------ |
| Hoxton        | 2.2.x        |
| Greenwich     | 2.1.x        |
| Finchley      | 2.0.x        |
| Edgware       | 1.5.x        |
| Dalston       | 1.5.x        |

### 1.3 Spring Cloud 与 Dubbo 对比

+ Spring Cloud 与 Dubbo 都是实现微服务有效的工具。

+ Dubbo 只是实现了服务治理，而 Spring Cloud 子项目分别覆盖了微服务架构下的众多部件。

+ Dubbo 使用 RPC 通讯协议，Spring Cloud 使用 RESTful 完成通信，Dubbo 效率略高于 Spring Cloud。

|              | Dubbo         | Spring Cloud                 |
| ------------ | ------------- | ---------------------------- |
| 服务注册中心 | Zookeeper     | Spring Cloud Netflix Eureka  |
| 服务调用方式 | RPC           | REST API                     |
| 服务监控     | Dubbo-monitor | Spring Boot Admin            |
| 断路器       | 不完善        | Spring Cloud Netflix Hystrix |
| 服务网关     | 无            | Spring Cloud Gatewav         |
| 分布式配置   | 无            | Spring Cloud Config          |
| 服务跟踪     | 无            | Spring Cloud Sleuth          |
| 消息总线     | 无            | Spring Cloud Bus             |
| 数据流       | 无            | Spring Cloud Stream          |
| 批量任务     | 无            | Spring Cloud Task            |

### 1.4 小结

+ 微服务就是将项目的各个模块拆分为可独立运行、部署、测试的架构设计风格。

+ Spring 公司将其他公司中微服务架构常用的组件整合起来，并使用 SpringBoot 简化其开发、配置。称为 Spring Cloud

+ Spring Cloud 与 Dubbo都是实现微服务有效的工具。Dubbo 性能更好，而 Spring Cloud 功能更全面。

## 2. Spring Cloud 服务治理

### 2.1. Eureka

#### 2.1.1 Eureka介绍

+ Eureka 是 Netflix 公司开源的一个服务注册与发现的组件 。

+ Eureka 和其他 Netflix 公司的服务组件（例如负载均衡、熔断器、网关等） 一起，被 Spring Cloud 社区整合为Spring-Cloud-Netflix 模块。 

+ Eureka 包含两个组件：Eureka Server (注册中心) 和 Eureka Client (服务提供者、服务消费者)。

![](https://img-blog.csdnimg.cn/1a9b7b81cfd248d59b2f954cc9de7223.png)

#### 2.1.2 Eureka快速入门

![](https://img-blog.csdnimg.cn/b8d8a7ac08f249199746c4f32b40e012.png)

+ 1.**搭建 Provider 和 Consumer 服务**。

+ 2.使用 **RestTemplate** 完成远程调用。
  + Spring提供的一种简单便捷的模板类，用于在 java 代码里访问 restful 服务。
  + 其功能与 HttpClient 类似，但是 RestTemplate 实现更优雅，使用更方便。

+ 3.搭建 **Eureka Server** 服务。
  + ①创建 eureka-server 模块
  + ②引入 SpringCloud 和 euraka-server 相关依赖
  + ③完成 Eureka Server 相关配置
  + ④启动该模块

+ 4.改造 Provider 和 Consumer 称为 **Eureka Client**。
  + ①引入 eureka-client 相关依赖
  + ②完成 eureka client 相关配置
  + ③启动 测试

+ 5.Consumer 服务 通过从 Eureka Server 中抓取 Provider 地址 完成 **远程调用**

#### 2.1.3 **Eureka –** **相关配置及特性**

+ eureka 一共有4部分 配置
  + 1.server : eureka 的服务端配置
  + 2.client : eureka 的客户端配置
  + 3.instance : eureka 的实例配置
  + 4.dashboard : eureka 的web控制台配置

+ **Eureka –** **相关配置及特性** **- instance** 

  ```yaml
  eureka:
  	instance:
          hostname: localhost # 主机名    
          prefer-ip-address: # 是否将自己的ip注册到eureka中，默认false 注册 主机名 
          ip-address: # 设置当前实例ip
          instance-id:  # 修改instance-id显示  
          lease-renewal-interval-in-seconds: 30 # 每一次eureka client 向 eureka server发送心跳的时间间隔  
          lease-expiration-duration-in-seconds: 90 # 如果90秒内eureka server没有收到eureka client的心跳包，则剔除该服务
  ```

+ **Eureka –** **相关配置及特性** **- server**

  ```yaml
  eureka:
  	server:
      #是否开启自我保护机制，默认true  
      enable-self-preservation:
      #清理间隔（单位毫秒，默认是60*1000）
      eviction-interval-timer-in-ms:
      
  ```

  ![](SpringCloud图片\Eureka-server.png)

  ```yaml
  eureka:
  	instance:
      	lease-renewal-interval-in-seconds: 30 # 每一次eureka client 向 eureka server发送心跳的时间间隔 
      	lease-expiration-duration-in-seconds: 90 # 如果90秒内eureka server没有收到eureka client的心跳包，则剔除该服务
  ```

+ **Eureka –** **相关配置及特性** **- client**

  ```yaml
  eureka:
  	client:
      	service-url:
           # eureka服务端地址，将来客户端使用该地址和eureka进行通信         
           defaultZone: 
      register-with-eureka: # 是否将自己的路径 注册到eureka上。
      fetch-registry: # 是否需要从eureka中抓取数据。
  
  ```

+ **Eureka –** **相关配置及特性** **- dashboard** 

  ```yaml
  eureka:
  	dashboard:
      enabled: true # 是否启用eureka web控制台
      path: / # 设置eureka web控制台默认访问路径
  ```

#### 2.1.4 **Eureka –** **高可用**

![](https://img-blog.csdnimg.cn/566016020e7a47b5a8b8a7ebdf093cbd.png)

+ 1.准备两个Eureka Server 

+ 2.分别进行配置，相互注册

+ 3.Eureka Client 分别注册到这两个 Eureka Server中

#### 2.1.5Euraka配置详解

Eureka包含四个部分的配置

1. instance：当前Eureka Instance实例信息配置
2. client：Eureka Client客户端特性配置
3. server：Eureka Server注册中心特性配置
4. dashboard：Eureka Server注册中心仪表盘配置

##### ①Eureka Instance实例信息配置

Eureka Instance的配置信息全部保存在org.springframework.cloud.netflix.eureka.EurekaInstanceConfigBean配置类里，实际上它是com.netflix.appinfo.EurekaInstanceConfig的实现类，替代了netflix的com.netflix.appinfo.CloudInstanceConfig的默认实现。

Eureka Instance的配置信息全部以eureka.instance.xxx的格式配置。

**配置列表**

- appname = unknown

应用名，首先获取spring.application.name的值，如果取值为空，则取默认unknown。

- appGroupName = null

应用组名

- instanceEnabledOnit = false

实例注册到Eureka上是，是否立刻开启通讯。有时候应用在准备好服务之前需要一些预处理。

- nonSecurePort = 80

非安全的端口

- securePort = 443

安全端口

- nonSecurePortEnabled = true

是否开启非安全端口通讯

- securePortEnabled = false

是否开启安全端口通讯

- leaseRenewalIntervalInSeconds = 30

实例续约间隔时间

- leaseExpirationDurationInSeconds = 90

实例超时时间，表示最大leaseExpirationDurationInSeconds秒后没有续约，Server就认为他不可用了，随之就会将其剔除。

- virtualHostName = unknown

虚拟主机名，首先获取spring.application.name的值，如果取值为空，则取默认unknown。

- instanceId

注册到eureka上的唯一实例ID，不能与相同appname的其他实例重复。

- secureVirtualHostName = unknown

安全虚拟主机名，首先获取spring.application.name的值，如果取值为空，则取默认unknown。

- metadataMap = new HashMap();

实例元数据，可以供其他实例使用。比如spring-boot-admin在监控时，获取实例的上下文和端口。

- dataCenterInfo = new MyDataCenterInfo(DataCenterInfo.Name.MyOwn);

实例部署的数据中心。如AWS、MyOwn。

- ipAddress=null

实例的IP地址

- statusPageUrlPath = "/actuator/info"

实例状态页相对url

- statusPageUrl = null

实例状态页绝对URL

- homePageUrlPath = "/"

实例主页相对URL

- homePageUrl = null

实例主页绝对URL

- healthCheckUrlUrlPath = "/actuator/health"

实例健康检查相对URL

- healthCheckUrl = null

实例健康检查绝对URL

- secureHealthCheckUrl = null

实例安全的健康检查绝对URL

- namespace = "eureka"

配置属性的命名空间（Spring Cloud中被忽略）

- hostname = null

主机名,不配置的时候讲根据操作系统的主机名来获取

- preferIpAddress = false

是否优先使用IP地址作为主机名的标识

##### ②Eureka Client客户端特性配置

Eureka Client客户端特性配置是对作为Eureka客户端的特性配置，包括Eureka注册中心，本身也是一个Eureka Client。

Eureka Client特性配置全部在org.springframework.cloud.netflix.eureka.EurekaClientConfigBean中，实际上它是com.netflix.discovery.EurekaClientConfig的实现类，替代了netxflix的默认实现。

Eureka Client客户端特性配置全部以eureka.client.xxx的格式配置。

**配置列表**

- enabled=true

是否启用Eureka client。

- registryFetchIntervalSeconds=30

定时从Eureka Server拉取服务注册信息的间隔时间

- instanceInfoReplicationIntervalSeconds=30

定时将实例信息（如果变化了）复制到Eureka Server的间隔时间。（InstanceInfoReplicator线程）

- initialInstanceInfoReplicationIntervalSeconds=40

首次将实例信息复制到Eureka Server的延迟时间。（InstanceInfoReplicator线程）

- eurekaServiceUrlPollIntervalSeconds=300

拉取Eureka Server地址的间隔时间（Eureka Server有可能增减）

- proxyPort=null

Eureka Server的代理端口

- proxyHost=null

Eureka Server的代理主机名

- proxyUserName=null

Eureka Server的代理用户名

- proxyPassword=null

Eureka Server的代理密码

- eurekaServerReadTimeoutSeconds=8

从Eureka Server读取信息的超时时间

- eurekaServerConnectTimeoutSeconds=5

连接Eureka Server的超时时间

- backupRegistryImpl=null

Eureka Client第一次启动时获取服务注册信息的调用的回溯实现。Eureka Client启动时首次会检查有没有BackupRegistry的实现类，如果有实现类，则优先从这个实现类里获取服务注册信息。

- eurekaServerTotalConnections=200

Eureka client连接Eureka Server的链接总数

- eurekaServerTotalConnectionsPerHost=50

Eureka client连接单台Eureka Server的链接总数

- eurekaServerURLContext=null

当Eureka server的列表在DNS中时，Eureka Server的上下文路径。如http://xxxx/eureka。

- eurekaServerPort=null

当Eureka server的列表在DNS中时，Eureka Server的端口。

- eurekaServerDNSName=null

当Eureka server的列表在DNS中时，且要通过DNSName获取Eureka Server列表时，DNS名字。

- region="us-east-1"

实例所属区域。

- eurekaConnectionIdleTimeoutSeconds = 30

Eureka Client和Eureka Server之间的Http连接的空闲超时时间。

- heartbeatExecutorThreadPoolSize=2

心跳（续约）执行器线程池大小。

- heartbeatExecutorExponentialBackOffBound=10

心跳执行器在续约过程中超时后的再次执行续约的最大延迟倍数。默认最大延迟时间=10 * eureka.instance.leaseRenewalIntervalInSeconds

- cacheRefreshExecutorThreadPoolSize=2

cacheRefreshExecutord的线程池大小（获取注册信息）

- cacheRefreshExecutorExponentialBackOffBound=10

cacheRefreshExecutord的再次执行的最大延迟倍数。默认最大延迟时间=10 *eureka.client.registryFetchIntervalSeconds

- serviceUrl= new HashMap();serviceUrl.put(DEFAULT_ZONE, DEFAULT_URL);

Eureka Server的分区地址。默认添加了一个defualtZone。也就是最常用的配置eureka.client.service-url.defaultZone=xxx

- registerWithEureka=true

是否注册到Eureka Server。

- preferSameZoneEureka=true

是否使用相同Zone下的Eureka server。

- logDeltaDiff=false

是否记录Eureka Server和Eureka Client之间注册信息的差异

- disableDelta=false

是否开启增量同步注册信息。

- fetchRemoteRegionsRegistry=null

获取注册服务的远程地区，以逗号隔开。

- availabilityZones=new HashMap()

可用分区列表。用逗号隔开。

- filterOnlyUpInstances = true

是否只拉取UP状态的实例。

- fetchRegistry=true

是否拉取注册信息。

- shouldUnregisterOnShutdown = true

是否在停止服务的时候向Eureka Server发起Cancel指令。

- shouldEnforceRegistrationAtInit = false

是否在初始化过程中注册服务。

##### ③Eureka Server注册中心端配置

Eureka Server注册中心端的配置是对注册中心的特性配置。Eureka Server的配置全部在org.springframework.cloud.netflix.eureka.server.EurekaServerConfigBean里，实际上它是com.netflix.eureka.EurekaServerConfig的实现类，替代了netflix的默认实现。

Eureka Server的配置全部以eureka.server.xxx的格式进行配置。

**配置列表**

- enableSelfPreservation=true

是否开启自我保护

- renewalPercentThreshold = 0.85

自我保护续约百分比阀值因子。如果实际续约数小于续约数阀值，则开启自我保护

- renewalThresholdUpdateIntervalMs = 15 * 60 * 1000

续约数阀值更新频率。

- peerEurekaNodesUpdateIntervalMs = 10 * 60 * 1000

Eureka Server节点更新频率。

- enableReplicatedRequestCompression = false

是否启用复制请求压缩。

- waitTimeInMsWhenSyncEmpty=5 * 60 * 1000

当从其他节点同步实例信息为空时等待的时间。

- peerNodeConnectTimeoutMs=200

节点间连接的超时时间。

- peerNodeReadTimeoutMs=200

节点间读取信息的超时时间。

- peerNodeTotalConnections=1000

节点间连接总数。

- peerNodeTotalConnectionsPerHost = 500;

单个节点间连接总数。

- peerNodeConnectionIdleTimeoutSeconds = 30;

节点间连接空闲超时时间。

- retentionTimeInMSInDeltaQueue = 3 * MINUTES;

增量队列的缓存时间。

- deltaRetentionTimerIntervalInMs = 30 * 1000;

清理增量队列中过期的频率。

- evictionIntervalTimerInMs = 60 * 1000;

剔除任务频率。

- responseCacheAutoExpirationInSeconds = 180;

注册列表缓存超时时间（当注册列表没有变化时）

- responseCacheUpdateIntervalMs = 30 * 1000;

注册列表缓存更新频率。

- useReadOnlyResponseCache = true;

是否开启注册列表的二级缓存。

- disableDelta=false。

是否为client提供增量信息。

- maxThreadsForStatusReplication = 1;

状态同步的最大线程数。

- maxElementsInStatusReplicationPool = 10000;

状态同步队列的最大容量。

- syncWhenTimestampDiffers = true;

当时间差异时是否同步。

- registrySyncRetries = 0;

注册信息同步重试次数。

- registrySyncRetryWaitMs = 30 * 1000;

注册信息同步重试期间的时间间隔。

- maxElementsInPeerReplicationPool = 10000;

节点间同步事件的最大容量。

- minThreadsForPeerReplication = 5;

节点间同步的最小线程数。

- maxThreadsForPeerReplication = 20;

节点间同步的最大线程数。

- maxTimeForReplication = 30000;

节点间同步的最大时间，单位为毫秒。

- disableDeltaForRemoteRegions = false；

是否启用远程区域增量。

- remoteRegionConnectTimeoutMs = 1000;

远程区域连接超时时间。

- remoteRegionReadTimeoutMs = 1000;

远程区域读取超时时间。

- remoteRegionTotalConnections = 1000;

远程区域最大连接数

- remoteRegionTotalConnectionsPerHost = 500;

远程区域单机连接数

- remoteRegionConnectionIdleTimeoutSeconds = 30;

远程区域连接空闲超时时间。

- remoteRegionRegistryFetchInterval = 30;

远程区域注册信息拉取频率。

- remoteRegionFetchThreadPoolSize = 20;

远程区域注册信息线程数。

##### ④**Eureka DashBoard注册中心仪表盘配置**

注册中心仪表盘的配置主要是控制注册中心的可视化展示。以eureka.dashboard.xxx的格式配置。

- path="/"

仪表盘访问路径

- enabled=true

是否启用仪表盘

### 2.2. Consul

#### 2.2.1 Consul介绍

+ Consul 是由 HashiCorp 基于 Go 语言开发的，支持多数据中心，分布式高可用的服务发布和注册服务软件。

+ 用于实现分布式系统的服务发现与配置。

+ 使用起来也较为简单。具有天然可移植性(支持Linux、windows和Mac OS X)；安装包仅包含一个可执行文件，方便部署 。

+ 官网地址： [https://www.consul.io](https://www.consul.io/) 

#### 2.2.2 **Consul快速入门**

+ 1.搭建 Provider 和 Consumer 服务。

+ 2.使用 RestTemplate 完成远程调用。

+ 3.将Provider服务注册到Consul中。

+ 4.Consumer 服务 通过从 Consul 中抓取 Provider 地址 完成 远程调用

![](https://img-blog.csdnimg.cn/f9d517caa84f4bd299cfb83b8f591c91.png)

### 2.3. Nacos

#### 2.3.1 Nacos介绍

+ Nacos（Dynamic Naming and Configuration Service） 是阿里巴巴2018年7月开源的项目。

+ 它专注于服务发现和配置管理领域 致力于帮助您发现、配置和管理微服务。Nacos 支持几乎所有主流类型的“服务”的发现、配置和管理。

+ 一句话概括就是Nacos = Spring Cloud注册中心 + Spring Cloud配置中心。

+ 官网：https://nacos.io/

+ 下载地址： https://github.com/alibaba/nacos/releases

#### 2.3.2 Nacos快速入门





## 3. Ribbon 客户端负载均衡

### 3.1 Ribbon 概述

+ Ribbon是 Netflix 提供的一个基于HTTP和TCP的客户端负载均衡工具。

+ Ribbon主要有两个功能：

  + 1.简化远程调用
  + 2.负载均衡

+ 服务端负载均衡

  + 负载均衡算法在服务端

  + 由负载均衡器维护服务地址列表

+ 客户端负载均衡

  + 负载均衡算法在客户端
  + 客户端维护服务地址列表

![Ribbon概述](https://img-blog.csdnimg.cn/e025ba0555d144a2aba6f6334d3e79b8.png)



### 3.2 Ribbon 简化远程调用

+ Ribbon 可以与 简化 RestTemplate 的远程调用

### 3.3 Ribbon 负载均衡

+ Ribbon 负载均衡策略：
  + 随机 ：RandomRule
  + 轮询 ：RoundRobinRule
  + 最小并发：BestAvailableRule
  + 过滤：AvailabilityFilteringRule
  + 响应时间：WeightedResponseTimeRule
  + 轮询重试：RetryRule
  + 性能可用性：ZoneAvoidanceRule

+ 设置负载均衡策略

  + 1.编码
  
    ```java
    package com.wzk.consumer.config;
    
    import com.netflix.loadbalancer.IRule;
    import com.netflix.loadbalancer.RandomRule;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    @Configuration
    public class MyRule {
    
        @Bean
        public IRule rule() {
            return new RandomRule();
        }
    }
    
    ```
  
    ```java
    package com.wzk.consumer;
    
    import com.wzk.consumer.config.MyRule;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
    import org.springframework.cloud.netflix.ribbon.RibbonClient;
    
    /**
     * 启动类
     */
    @SpringBootApplication
    @EnableEurekaClient
    /**
     * 配置Ribbon的负载均衡策略 name
     *  * name：设置 服务提供方的应用名称
     *  * configuration：设置负载均衡Bean
     */
    
    @RibbonClient(name = "EUREKA-PROVIDER",configuration = MyRule.class)
    public class ConsumerApp {
        public static void main(String[] args) {
            SpringApplication.run(ConsumerApp.class,args);
        }
    }
    
    ```
  
    
  
  + 2.配置
  
  ```yaml
  user-service: # 生产者服务名称
  	ribbon:
      	NFloadBalancerRuleClassName: XxxRule # 负载均衡策略类 
  
  ```

## 4. Feign 声明式服务调用

### 4.1 Feign 概述

![](https://img-blog.csdnimg.cn/b8d8a7ac08f249199746c4f32b40e012.png)

+ Feign 是一个声明式的 REST 客户端，它用了基于接口的注解方式，很方便实现客户端配置。

+ Feign 最初由 Netflix 公司提供，但不支持SpringMVC注解，后由 SpringCloud 对其封装，支持了SpringMVC注解，让使用者更易于接受。

### 4.2 Feign 快速入门

![](https://img-blog.csdnimg.cn/b8d8a7ac08f249199746c4f32b40e012.png)

+ 1.在消费端引入 open-feign 依赖

+ 2.编写Feign调用接口

+ 3.在启动类 添加 @EnableFeignClients 注解，开启Feign功能

+ 4.测试调用

### 4.3 Feign 其他功能

#### 4.3.1 超时设置

![](https://img-blog.csdnimg.cn/b8d8a7ac08f249199746c4f32b40e012.png)

+ Feign 底层依赖于 Ribbon 实现负载均衡和远程调用。

+ Ribbon默认1秒超时。

+ 超时配置：

  ```yaml
  ribbon:
  	ConnectTimeout: 1000   #连接超时时间,毫秒
      ReadTimeout: 1000     #逻辑处理超时时间,毫秒
  ```

  

#### 4.3.2 日志记录

+ Feign 只能记录 debug 级别的日志信息。

  ```yaml
  logging:
  	level:
      com.wzk: debug
  ```

+ 定义Feign日志级别Bean

  ```java
  @Bean
  Logger.Level feignLoggerLevel() {
  	return Logger.Level.FULL;
  }
  ```

  

+ 启用该Bean

  ```java
  @FeignClient(configuration = XxxConfig.class)
  ```

## 5. Hystrix 熔断器

### 5.1 Hystrix 概述

+ Hystix 是 Netflix 开源的一个延迟和容错库，用于隔离访问远程服务、第三方库，防止出现级联失败（雪崩）。

+ 雪崩：一个服务失败，导致整条链路的服务都失败的情形。

+ Hystix 主要功能

  + 隔离

    + 1.线程池隔离

    ![](https://img-blog.csdnimg.cn/d5ee98599c724a1c86a4b2d603d92e3d.png)

    ![](https://img-blog.csdnimg.cn/51d75c6ab2a34aa6a569f9697c4064a3.png)

    

    ![](https://img-blog.csdnimg.cn/70afc42bc9f245e3b29bebc254c36649.png)

    ![](https://img-blog.csdnimg.cn/f657afeb85b0444fa92c3cfdf358a441.png)

    + 2.信号量隔离

      ![](https://img-blog.csdnimg.cn/ab01164fd10e448e87e824eb35e816c2.png)

  + 降级：异常，超时

    ![](https://img-blog.csdnimg.cn/33595e5bcba7426a86f47961a29e0e4d.png)

  + 熔断

  + 限流

![](https://img-blog.csdnimg.cn/0c4caa02de05453588cd3507fc45ad04.png)

![](https://img-blog.csdnimg.cn/91e6584393f64dffb12ebfc42d6ba5ba.png)

![](https://img-blog.csdnimg.cn/7765c0870b3a486d97ebd27de34aa263.png)

![](https://img-blog.csdnimg.cn/298fdef20e27495ea13fa90b0e0725ad.png)

### 5.2 Hystrix 降级

#### 5.2.1 Hystrix 降级介绍

+ Hystix 降级：当服务发生异常或调用超时，返回默认数据

![](https://img-blog.csdnimg.cn/33595e5bcba7426a86f47961a29e0e4d.png)



#### 5.2.2 **Hystrix** **降级** **–** **服务提供方**

![](https://img-blog.csdnimg.cn/d5906c2dd4d248eb963d8eb230bb1ebc.png)



+ 1.在服务提供方，引入 hystrix 依赖

+ 2.定义降级方法

+ 3.使用 @HystrixCommand 注解配置降级方法

+ 4.在启动类上开启Hystrix功能：@EnableCircuitBreaker

#### 5.2.3 **Hystrix** **降级** **–** **服务消费方**

+ 1.feign 组件已经集成了 hystrix 组件。

+ 2.定义feign 调用接口实现类，复写方法，即 降级方法

+ 3.在 @FeignClient 注解中使用 fallback 属性设置降级处理类。

+ 4.配置开启 feign.hystrix.enabled = true

![](https://img-blog.csdnimg.cn/2dc8a5a8e74f47739e9f0dc13862a06c.png)



### 5.3 Hystrix 熔断

+ Hystrix 熔断机制，用于监控微服务调用情况，当失败的情况达到预定的阈值（5秒失败20次），会打开断路器，拒绝所有请求，直到服务恢复正常为止。
+ circuitBreaker.sleepWindowInMilliseconds：监控时间
+ circuitBreaker.requestVolumeThreshold：失败次数
+ circuitBreaker.errorThresholdPercentage：失败率

![](https://img-blog.csdnimg.cn/af7ee6440f2f4843bf237a219c56badc.png)

### 5.4 Hystrix 熔断监控

+ Hystrix 提供了 Hystrix-dashboard 功能，用于实时监控微服务运行状态。

+ 但是Hystrix-dashboard只能监控一个微服务。

+ Netflix 还提供了 Turbine ，进行聚合监控。

![](https://img-blog.csdnimg.cn/f2099d8d589c41b388edc11acaefbbe5.png)



![](https://img-blog.csdnimg.cn/ad03dcbab25242abb18c3e1823770382.png)



### 5.5 Turbine聚合监控 

#### 5.5.1 搭建监控模块

**1. 创建监控模块**

创建hystrix-monitor模块，使用Turbine聚合监控多个Hystrix dashboard功能,

**2. 引入Turbine聚合监控起步依赖**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>hystrix-parent</artifactId>
        <groupId>com.wzk</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>hystrix-monitor</artifactId>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-turbine</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

**3. 修改application.yml**

```yml
spring:
  application.name: hystrix-monitor
server:
  port: 8769
turbine:
  combine-host-port: true
  # 配置需要被监控的服务名称列表
  app-config: hystrix-provider,hystrix-consumer
  cluster-name-expression: "'default'"
  aggregator:
    cluster-config: default
  #instanceUrlSuffix: /actuator/hystrix.stream
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/

```

**4. 创建启动类**

```java
@SpringBootApplication
@EnableEurekaClient

@EnableTurbine //开启Turbine 很聚合监控功能
@EnableHystrixDashboard //开启Hystrix仪表盘监控功能
public class HystrixMonitorApp {

    public static void main(String[] args) {
        SpringApplication.run(HystrixMonitorApp.class, args);
    }

}

```

#### 5.5.2 修改被监控模块

需要分别修改 hystrix-provider 和 hystrix-consumer 模块：

**1、导入依赖：**

```xml
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>
```

**2、配置Bean**

此处为了方便，将其配置在启动类中。

```java
@Bean
    public ServletRegistrationBean getServlet() {
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/actuator/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
```

**3、启动类上添加注解@EnableHystrixDashboard**

```java
@EnableDiscoveryClient
@EnableEurekaClient
@SpringBootApplication
@EnableFeignClients 
@EnableHystrixDashboard // 开启Hystrix仪表盘监控功能
public class ConsumerApp {


    public static void main(String[] args) {
        SpringApplication.run(ConsumerApp.class,args);
    }
    @Bean
    public ServletRegistrationBean getServlet() {
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/actuator/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
}

```

#### 5.5.3 启动测试

**1、启动服务：**

- eureka-server

- hystrix-provider

- hystrix-consumer

- hystrix-monitor

**2、访问：**

在浏览器访问http://localhost:8769/hystrix/ 进入Hystrix Dashboard界面

![1585421193757](https://img-blog.csdnimg.cn/4b09a15de7a045478281a166b6f856c3.png)

界面中输入监控的Url地址 http://localhost:8769/turbine.stream，监控时间间隔2000毫秒和title，如下图

![1585421278837](https://img-blog.csdnimg.cn/497c19c05ce445d0bc300224da78bebb.png)



- 实心圆：它有颜色和大小之分，分别代表实例的监控程度和流量大小。如上图所示，它的健康度从绿色、黄色、橙色、红色递减。通过该实心圆的展示，我们就可以在大量的实例中快速的发现故障实例和高压力实例。
- 曲线：用来记录 2 分钟内流量的相对变化，我们可以通过它来观察到流量的上升和下降趋势。

![1585421278837](https://img-blog.csdnimg.cn/f98eadd6fb144671a2c14cf86fd9334e.png)





## 6. Gateway 网关

### 6.1 网关概述

+ 网关旨在为微服务架构提供一种简单而有效的统一的API路由管理方式。

+ 在微服务架构中，不同的微服务可以有不同的网络地址，各个微服务之间通过互相调用完成用户请求，客户端可能通过调用N个微服务的接口完成一个用户请求。

  + 存在的问题：

    + 客户端多次请求不同的微服务，增加客户端的复杂性

    + 认证复杂，每个服务都要进行认证

    + http请求不同服务次数增加，性能不高

+ 网关就是系统的入口，封装了应用程序的内部结构，为客户端提

  供统一服务，一些与业务本身功能无关的公共逻辑可以在这里实现，

  诸如认证、鉴权、监控、缓存、负载均衡、流量管控、路由转发等

+ 在目前的网关解决方案里，有Nginx+ Lua、Netflix Zuul 、Spring Cloud Gateway等等

![](https://img-blog.csdnimg.cn/1c1555819919473ebc5a5b4b42d82cd2.png)



### 6.2 Gateway 网关快速入门

+ 1.搭建网关模块

+ 2.引入依赖：starter-gateway

+ 3.编写启动类

+ 4.编写配置文件

+ 5.启动测试

![](https://img-blog.csdnimg.cn/1c1555819919473ebc5a5b4b42d82cd2.png)





### 6.3 Gateway 网关路由配置

#### 6.3.1 静态路由

![](https://img-blog.csdnimg.cn/1c1555819919473ebc5a5b4b42d82cd2.png)

#### 6.3.2 **动态路由**

+ 引入eureka-client配置

+ 修改uri属性：uri: lb://服务名称

![](https://img-blog.csdnimg.cn/3ca1a803f3ba40ed8e4b50f06e6e1a9d.png)

#### 6.3.4 **微服务名称配置**

```yaml
spring:
	cloud:
		discovery:
		locator:
        enabled: true # 开启微服发现功能
        lower-case-service-id: true # 讲请求路径上的服务名配置为小写
```

![](https://img-blog.csdnimg.cn/1c1555819919473ebc5a5b4b42d82cd2.png)

### 6.4 Gateway 网关过滤器

#### 6.4.1 Gateway 网关过滤器介绍

+ Gateway 支持过滤器功能，对请求或响应进行拦截，完成一些通用操作。

+ Gateway 提供两种过滤器方式：“pre”和“post”

+ pre 过滤器，在转发之前执行，可以做参数校验、权限校验、流量监控、日志输出、协议转换等。

+ post 过滤器，在响应之前执行，可以做响应内容、响应头的修改，日志的输出，流量监控等。

+ Gateway 还提供了两种类型过滤器

+ GatewayFilter：局部过滤器，针对单个路由

+ GlobalFilter ：全局过滤器，针对所有路由

![](https://img-blog.csdnimg.cn/6191f252a17c4eb8a28c196a269b8c74.png)

#### 6.4.2 局部过滤器

+ GatewayFilter 局部过滤器，是针对单个路由的过滤器。

+ 在Spring Cloud Gateway 组件中提供了大量内置的局部过滤器，对请求和响应做过滤操作。

+ 遵循约定大于配置的思想，只需要在配置文件配置局部过滤器名称，并为其指定对应的值，就可以让其生效。

![](https://img-blog.csdnimg.cn/cdbf5b2bc8a342faa2d5a8660b8d4e48.png)

#### 6.4.3 全局过滤器

+ GlobalFilter 全局过滤器，不需要在配置文件中配置，系统初始化时加载，并作用在每个路由上。

+ Spring Cloud Gateway 核心的功能也是通过内置的全局过滤器来完成。

+ 自定义全局过滤器步骤：
  + 1.定义类实现 GlobalFilter 和 Ordered接口
  + 2.复写方法
  + 3.完成逻辑处理

![](https://img-blog.csdnimg.cn/88b0f7ecc71849ad80d49482dab5bd2d.png)

### 6.5 Spring Cloud Gateway 内置的过滤器工厂

#### 内置的过滤器工厂

这里简单将Spring Cloud Gateway内置的所有过滤器工厂整理成了一张表格。如下：

| 过滤器工厂                  | 作用                                                         | 参数                                                         |
| :-------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| AddRequestHeader            | 为原始请求添加Header                                         | Header的名称及值                                             |
| AddRequestParameter         | 为原始请求添加请求参数                                       | 参数名称及值                                                 |
| AddResponseHeader           | 为原始响应添加Header                                         | Header的名称及值                                             |
| DedupeResponseHeader        | 剔除响应头中重复的值                                         | 需要去重的Header名称及去重策略                               |
| Hystrix                     | 为路由引入Hystrix的断路器保护                                | `HystrixCommand`的名称                                       |
| FallbackHeaders             | 为fallbackUri的请求头中添加具体的异常信息                    | Header的名称                                                 |
| PrefixPath                  | 为原始请求路径添加前缀                                       | 前缀路径                                                     |
| PreserveHostHeader          | 为请求添加一个preserveHostHeader=true的属性，路由过滤器会检查该属性以决定是否要发送原始的Host | 无                                                           |
| RequestRateLimiter          | 用于对请求限流，限流算法为令牌桶                             | keyResolver、rateLimiter、statusCode、denyEmptyKey、emptyKeyStatus |
| RedirectTo                  | 将原始请求重定向到指定的URL                                  | http状态码及重定向的url                                      |
| RemoveHopByHopHeadersFilter | 为原始请求删除IETF组织规定的一系列Header                     | 默认就会启用，可以通过配置指定仅删除哪些Header               |
| RemoveRequestHeader         | 为原始请求删除某个Header                                     | Header名称                                                   |
| RemoveResponseHeader        | 为原始响应删除某个Header                                     | Header名称                                                   |
| RewritePath                 | 重写原始的请求路径                                           | 原始路径正则表达式以及重写后路径的正则表达式                 |
| RewriteResponseHeader       | 重写原始响应中的某个Header                                   | Header名称，值的正则表达式，重写后的值                       |
| SaveSession                 | 在转发请求之前，强制执行`WebSession::save`操作               | 无                                                           |
| secureHeaders               | 为原始响应添加一系列起安全作用的响应头                       | 无，支持修改这些安全响应头的值                               |
| SetPath                     | 修改原始的请求路径                                           | 修改后的路径                                                 |
| SetResponseHeader           | 修改原始响应中某个Header的值                                 | Header名称，修改后的值                                       |
| SetStatus                   | 修改原始响应的状态码                                         | HTTP 状态码，可以是数字，也可以是字符串                      |
| StripPrefix                 | 用于截断原始请求的路径                                       | 使用数字表示要截断的路径的数量                               |
| Retry                       | 针对不同的响应进行重试                                       | retries、statuses、methods、series                           |
| RequestSize                 | 设置允许接收最大请求包的大小。如果请求包大小超过设置的值，则返回 `413 Payload Too Large` | 请求包大小，单位为字节，默认值为5M                           |
| ModifyRequestBody           | 在转发请求之前修改原始请求体内容                             | 修改后的请求体内容                                           |
| ModifyResponseBody          | 修改原始响应体的内容                                         | 修改后的响应体内容                                           |
| Default                     | 为所有路由添加过滤器                                         | 过滤器工厂名称及值                                           |

**Tips：**每个过滤器工厂都对应一个实现类，并且这些类的名称必须以`GatewayFilterFactory`结尾，这是Spring Cloud Gateway的一个约定，例如`AddRequestHeader`对应的实现类为`AddRequestHeaderGatewayFilterFactory`。

##### 6.5.1、AddRequestHeader GatewayFilter Factory

为原始请求添加Header，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_header_route
        uri: https://example.org
        filters:
        - AddRequestHeader=X-Request-Foo, Bar
```

为原始请求添加名为 `X-Request-Foo` ，值为 `Bar` 的请求头

##### 6.5.2、AddRequestParameter GatewayFilter Factory

为原始请求添加请求参数及值，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_parameter_route
        uri: https://example.org
        filters:
        - AddRequestParameter=foo, bar
```

为原始请求添加名为foo，值为bar的参数，即：`foo=bar`

##### 6.5.3、AddResponseHeader GatewayFilter Factory

为原始响应添加Header，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: add_response_header_route
        uri: https://example.org
        filters:
        - AddResponseHeader=X-Response-Foo, Bar
```

为原始响应添加名为 `X-Request-Foo` ，值为 `Bar` 的响应头

##### 6.5.4、DedupeResponseHeader GatewayFilter Factory

DedupeResponseHeader可以根据配置的Header名称及去重策略剔除响应头中重复的值，这是Spring Cloud Greenwich SR2提供的新特性，低于这个版本无法使用。

我们在Gateway以及微服务上都设置了CORS（解决跨域）Header的话，如果不做任何配置，那么请求 -> 网关 -> 微服务，获得的CORS Header的值，就将会是这样的：

```bash
Access-Control-Allow-Credentials: true, true
Access-Control-Allow-Origin: https://musk.mars, https://musk.mars
```

可以看到这两个Header的值都重复了，若想把这两个Header的值去重的话，就需要使用到DedupeResponseHeader，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: dedupe_response_header_route
        uri: https://example.org
        filters:
        # 若需要去重的Header有多个，使用空格分隔
        - DedupeResponseHeader=Access-Control-Allow-Credentials Access-Control-Allow-Origin
```

去重策略：

- RETAIN_FIRST：默认值，保留第一个值
- RETAIN_LAST：保留最后一个值
- RETAIN_UNIQUE：保留所有唯一值，以它们第一次出现的顺序保留

若想对该过滤器工厂有个比较全面的了解的话，建议阅读该过滤器工厂的源码，因为源码里有详细的注释及示例，比官方文档写得还好：`org.springframework.cloud.gateway.filter.factory.DedupeResponseHeaderGatewayFilterFactory`

##### 6.5.5、Hystrix GatewayFilter Factory

为路由引入Hystrix的断路器保护，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: hystrix_route
        uri: https://example.org
        filters:
        - Hystrix=myCommandName
```

Hystrix是Spring Cloud第一代容错组件，不过已经进入维护模式，未来Hystrix会被Spring Cloud移除掉，取而代之的是Alibaba Sentinel/Resilience4J。所以本文不做详细介绍了，感兴趣的话可以参考官方文档：

- [Hystrix GatewayFilter Factory](https://cloud.spring.io/spring-cloud-static/Greenwich.SR2/single/spring-cloud.html#hystrix)

##### 6.5.6、FallbackHeaders GatewayFilter Factory

同样是对Hystrix的支持，上一小节所介绍的过滤器工厂支持一个配置参数：`fallbackUri`，该配置用于当发生异常时将请求转发到一个特定的uri上。而`FallbackHeaders`这个过滤工厂可以在转发请求到该uri时添加一个Header，这个Header的值为具体的异常信息。配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: ingredients
        uri: lb://ingredients
        predicates:
        - Path=//ingredients/**
        filters:
        - name: Hystrix
          args:
            name: fetchIngredients
            fallbackUri: forward:/fallback
      - id: ingredients-fallback
        uri: http://localhost:9994
        predicates:
        - Path=/fallback
        filters:
        - name: FallbackHeaders
          args:
            executionExceptionTypeHeaderName: Test-Header
```

这里也不做详细介绍了，感兴趣可以参考官方文档：

- [FallbackHeaders GatewayFilter Factory](https://cloud.spring.io/spring-cloud-static/Greenwich.SR2/single/spring-cloud.html#fallback-headers)

##### 6.5.7、PrefixPath GatewayFilter Factory

为原始的请求路径添加一个前缀路径，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: prefixpath_route
        uri: https://example.org
        filters:
        - PrefixPath=/mypath
```

该配置使访问`${GATEWAY_URL}/hello` 会转发到`https://example.org/mypath/hello`

##### 6.5.8、PreserveHostHeader GatewayFilter Factory

为请求添加一个preserveHostHeader=true的属性，路由过滤器会检查该属性以决定是否要发送原始的Host Header。配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: preserve_host_route
        uri: https://example.org
        filters:
        - PreserveHostHeader
```

如果不设置，那么名为 `Host` 的Header将由Http Client控制

##### 6.5.9、RequestRateLimiter GatewayFilter Factory

用于对请求进行限流，限流算法为令牌桶。配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: requestratelimiter_route
        uri: https://example.org
        filters:
        - name: RequestRateLimiter
          args:
            redis-rate-limiter.replenishRate: 10
            redis-rate-limiter.burstCapacity: 20
```

由于另一篇文章中已经介绍过如何使用该过滤器工厂实现网关限流，所以这里就不再赘述了：

- [Spring Cloud Gateway - 扩展](https://blog.51cto.com/zero01/2430532)

或者参考官方文档：

- [RequestRateLimiter GatewayFilter Factory](https://cloud.spring.io/spring-cloud-static/Greenwich.SR2/single/spring-cloud.html#_requestratelimiter_gatewayfilter_factory)

##### **6.5.10**、RedirectTo GatewayFilter Factory

将原始请求重定向到指定的Url，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: redirect_route
        uri: https://example.org
        filters:
        - RedirectTo=302, https://acme.org
```

该配置使访问 `${GATEWAY_URL}/hello` 会被重定向到 `https://acme.org/hello` ，并且携带一个 `Location:http://acme.org` 的Header，而返回客户端的HTTP状态码为302

注意事项：

- HTTP状态码应为3xx，例如301
- URL必须是合法的URL，该URL会作为`Location` Header的值

##### 6.5.11、RemoveHopByHopHeadersFilter GatewayFilter Factory

为原始请求删除[IETF](https://tools.ietf.org/html/draft-ietf-httpbis-p1-messaging-14#section-7.1.3)组织规定的一系列Header，默认删除的Header如下：

- Connection
- Keep-Alive
- Proxy-Authenticate
- Proxy-Authorization
- TE
- Trailer
- Transfer-Encoding
- Upgrade

可以通过配置去指定仅删除哪些Header，配置示例：

```yml
spring:
  cloud:
    gateway:
      filter:
        remove-hop-by-hop:
          # 多个Header使用逗号（,）分隔
          headers: Connection,Keep-Alive
```

##### 6.5.12、RemoveRequestHeader GatewayFilter Factory

为原始请求删除某个Header，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: removerequestheader_route
        uri: https://example.org
        filters:
        - RemoveRequestHeader=X-Request-Foo
```

删除原始请求中名为 `X-Request-Foo` 的请求头

##### 6.5.13、RemoveResponseHeader GatewayFilter Factory

为原始响应删除某个Header，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: removeresponseheader_route
        uri: https://example.org
        filters:
        - RemoveResponseHeader=X-Response-Foo
```

删除原始响应中名为 `X-Request-Foo` 的响应头

##### 6.5.14、RewritePath GatewayFilter Factory

通过正则表达式重写原始的请求路径，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: rewritepath_route
        uri: https://example.org
        predicates:
        - Path=/foo/**
        filters:
        # 参数1为原始路径的正则表达式，参数2为重写后路径的正则表达式
        - RewritePath=/foo/(?<segment>.*), /$\{segment}

```

该配置使得访问 `/foo/bar` 时，会将路径重写为`/bar` 再进行转发，也就是会转发到 `https://example.org/bar`。需要注意的是：由于YAML语法，需用`$\` 替换 `$`

##### 6.5.15、RewriteResponseHeader GatewayFilter Factory

重写原始响应中的某个Header，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: rewriteresponseheader_route
        uri: https://example.org
        filters:
        # 参数1为Header名称，参数2为值的正则表达式，参数3为重写后的值
        - RewriteResponseHeader=X-Response-Foo, password=[^&]+, password=***

```

该配置的意义在于：如果响应头中 `X-Response-Foo` 的值为`/42?user=ford&password=omg!what&flag=true`，那么就会被按照配置的值重写成`/42?user=ford&password=***&flag=true`，也就是把其中的`password=omg!what`重写成了`password=***`

##### 6.5.16、SaveSession GatewayFilter Factory

在转发请求之前，强制执行`WebSession::save`操作，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: save_session
        uri: https://example.org
        predicates:
        - Path=/foo/**
        filters:
        - SaveSession

```

主要用在那种像 Spring Session 延迟数据存储（数据不是立刻持久化）的，并希望在请求转发前确保session状态保存情况。如果你将Spring Secutiry于Spring Session集成使用，并想确保安全信息都传到下游机器，你就需要配置这个filter。

##### 6.5.17、secureHeaders GatewayFilter Factory

secureHeaders过滤器工厂主要是参考了[这篇博客](https://blog.appcanary.com/2017/http-security-headers.html)中的建议，为原始响应添加了一系列起安全作用的响应头。默认会添加如下Headers（包括值）：

- `X-Xss-Protection:1; mode=block`
- `Strict-Transport-Security:max-age=631138519`
- `X-Frame-Options:DENY`
- `X-Content-Type-Options:nosniff`
- `Referrer-Policy:no-referrer`
- `Content-Security-Policy:default-src 'self' https:; font-src 'self' https: data:; img-src 'self' https: data:; object-src 'none'; script-src https:; style-src 'self' https: 'unsafe-inline'`
- `X-Download-Options:noopen`
- `X-Permitted-Cross-Domain-Policies:none`

如果你想修改这些Header的值，那么就需要使用这些Headers对应的后缀，如下：

- `xss-protection-header`
- `strict-transport-security`
- `frame-options`
- `content-type-options`
- `referrer-policy`
- `content-security-policy`
- `download-options`
- `permitted-cross-domain-policies`

配置示例：

```yml
spring:
  cloud:
    gateway:
      filter:
        secure-headers:
          # 修改 X-Xss-Protection 的值为 2; mode=unblock
          xss-protection-header: 2; mode=unblock
```

如果想禁用某些Header，可使用如下配置：

```yml
spring:
  cloud:
    gateway:
      filter:
        secure-headers:
          # 多个使用逗号（,）分隔
          disable: frame-options,download-options

```

##### 6.5.18、SetPath GatewayFilter Factory

修改原始的请求路径，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: setpath_route
        uri: https://example.org
        predicates:
        - Path=/foo/{segment}
        filters:
        - SetPath=/{segment}

```

该配置使访问 `${GATEWAY_URL}/foo/bar` 时会转发到 `https://example.org/bar` ，也就是原本的`/foo/bar`被修改为了`/bar`

##### **6.5.19**、SetResponseHeader GatewayFilter Factory

修改原始响应中某个Header的值，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: setresponseheader_route
        uri: https://example.org
        filters:
        - SetResponseHeader=X-Response-Foo, Bar

```

将原始响应中 `X-Response-Foo` 的值修改为 `Bar`

##### 6.5.20、SetStatus GatewayFilter Factory

修改原始响应的状态码，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: setstatusstring_route
        uri: https://example.org
        filters:
        # 字符串形式
        - SetStatus=BAD_REQUEST
      - id: setstatusint_route
        uri: https://example.org
        filters:
        # 数字形式
        - SetStatus=401
```

SetStatusd的值可以是数字，也可以是字符串。但一定要是Spring `HttpStatus` 枚举类中的值。上面这两种配置都可以返回401这个HTTP状态码。

##### 6.5.21、StripPrefix GatewayFilter Factory

用于截断原始请求的路径，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: nameRoot
        uri: http://nameservice
        predicates:
        - Path=/name/**
        filters:
        # 数字表示要截断的路径的数量
        - StripPrefix=2

```

如上配置，如果请求的路径为 `/name/bar/foo` ，那么则会截断成`/foo`后进行转发 ，也就是会截断2个路径。

##### 6.5.22、Retry GatewayFilter Factory

针对不同的响应进行重试，例如可以针对HTTP状态码进行重试，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: retry_test
        uri: http://localhost:8080/flakey
        predicates:
        - Host=*.retry.com
        filters:
        - name: Retry
          args:
            retries: 3
            statuses: BAD_GATEWAY
```

可配置如下参数：

- `retries`：重试次数
- `statuses`：需要重试的状态码，取值在 `org.springframework.http.HttpStatus` 中
- `methods`：需要重试的请求方法，取值在 `org.springframework.http.HttpMethod` 中
- `series`：HTTP状态码序列，取值在 `org.springframework.http.HttpStatus.Series` 中

##### 6.5.23、RequestSize GatewayFilter Factory

设置允许接收最大请求包的大小，配置示例：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: request_size_route
      uri: http://localhost:8080/upload
      predicates:
      - Path=/upload
      filters:
      - name: RequestSize
        args:
          # 单位为字节
          maxSize: 5000000

```

如果请求包大小超过设置的值，则会返回 `413 Payload Too Large`以及一个`errorMessage`

##### 6.5.24、Modify Request Body GatewayFilter Factory

在转发请求之前修改原始请求体内容，该过滤器工厂只能通过代码配置，不支持在配置文件中配置。代码示例：

```java
@Bean
public RouteLocator routes(RouteLocatorBuilder builder) {
    return builder.routes()
        .route("rewrite_request_obj", r -> r.host("*.rewriterequestobj.org")
            .filters(f -> f.prefixPath("/httpbin")
                .modifyRequestBody(String.class, Hello.class, MediaType.APPLICATION_JSON_VALUE,
                    (exchange, s) -> return Mono.just(new Hello(s.toUpperCase())))).uri(uri))
        .build();
}
 
static class Hello {
    String message;
 
    public Hello() { }
 
    public Hello(String message) {
        this.message = message;
    }
 
    public String getMessage() {
        return message;
    }
 
    public void setMessage(String message) {
        this.message = message;
    }
}
```

> Tips：该过滤器工厂处于 **BETA** 状态，未来API可能会变化，生产环境请慎用

##### 6.5.25、Modify Response Body GatewayFilter Factory

可用于修改原始响应体的内容，该过滤器工厂同样只能通过代码配置，不支持在配置文件中配置。代码示例：

```java
@Bean
public RouteLocator routes(RouteLocatorBuilder builder) {
    return builder.routes()
        .route("rewrite_response_upper", r -> r.host("*.rewriteresponseupper.org")
            .filters(f -> f.prefixPath("/httpbin")
                .modifyResponseBody(String.class, String.class,
                    (exchange, s) -> Mono.just(s.toUpperCase()))).uri(uri)
        .build();
}

```

> Tips：该过滤器工厂处于 **BETA** 状态，未来API可能会变化，生产环境请慎用

##### **6.5.26**、Default Filters

Default Filters用于为所有路由添加过滤器工厂，也就是说通过Default Filter所配置的过滤器工厂会作用到所有的路由上。配置示例：

```yml
spring:
  cloud:
    gateway:
      default-filters:
      - AddResponseHeader=X-Response-Default-Foo, Default-Bar
      - PrefixPath=/httpbin
```

------

官方文档：

- [GatewayFilter Factories](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.1.0.RELEASE/single/spring-cloud-gateway.html#_gatewayfilter_factories)

## 7. Config 分布式配置中心

### 7.1 Config 概述

+ Spring Cloud Config 解决了在分布式场景下多环境配置文件的管理和维护。

+ 好处：
  + 集中管理配置文件
  + 不同环境不同配置，动态化的配置更新
  + 配置信息改变时，不需要重启即可更新配置信息到服务

![](https://img-blog.csdnimg.cn/70242aaef7964c32877051d8bcf3f5e3.png)



### 7.2 Config 快速入门

+ config server：
  + 1.使用gitee创建远程仓库，上传配置文件
  + 2.搭建 config server 模块
  + 3.导入 config-server 依赖
  + 4.编写配置，设置 gitee 远程仓库地址
  + 5.测试访问远程配置文件

+ config client：
  + 1.导入 starter-config 依赖
  + 2.配置config server 地址，读取配置文件名称等信息
  + 3.获取配置值
  + 4.启动测试

![](https://img-blog.csdnimg.cn/70242aaef7964c32877051d8bcf3f5e3.png)

+ **Config** **客户端刷新**

  + 1.在 config 客户端引入 actuator 依赖

  + 2.获取配置信息类上，添加 @RefreshScope 注解

  + 3.添加配置

    management.endpoints.web.exposure.include: refresh

  + 4.使用curl工具发送post请求（通过cmd）

    ```shell
    curl -X POST http://localhost:8001/actuator/refresh 
    ```
    
    

### 7.3 Config 集成Eureka

```yml
config-client配置：
spring:
	cloud:
    	config:
        	discovery:
            enabled: true
            service-id: config-server
```

```yml
config-server配置:
eureka:
	client:
    	service-url:
        	defaultZone: http://localhost:8761/eureka/
```

![](https://img-blog.csdnimg.cn/6f77b0dcec7a4a64b785ff2dcd643f13.png)



## 8.Bus 消息总线

### 8.1 Bus 概述

+ Spring Cloud Bus 是用轻量的消息中间件将分布式的节点连接起来，可以用于广播配置文件的更改或者服务的监控管理。关键的思想就是，消息总线可以为微服务做监控，也可以实现应用程序之间相通信。

+ Spring Cloud Bus 可选的消息中间件包括 RabbitMQ 和 Kafka 。

![](https://img-blog.csdnimg.cn/1ef95b5b3d244b6691f48d1941018962.png)





### 8.2 RabbitMQ 回顾

![](https://img-blog.csdnimg.cn/68ceb5bcc6734d5996b2270f897a0c61.png)

![](https://img-blog.csdnimg.cn/b514a32ffebb47a28d56f34e540adfba.png)



+ RabbitMQ 提供了 6 种工作模式：简单模式、work queues、Publish/Subscribe 发布与订阅模式、Routing 路由模式、Topics 主题模式、RPC 远程调用模式（远程调用，不太算 MQ；暂不作介绍）。

![](https://img-blog.csdnimg.cn/d1410228689149909962f2d36629a8f2.png)

### 8.3 RabbitMQ Windows 安装

#### 8.3.1、安装Erlang

1. 双击资料中提供的 **otp_win64_22.1.exe** ，选择对应安装目录，一路下一步，完成安装。

2. 设置Erlang环境变量

   （1）新建ERLANG_HOME

![1585755246863](https://img-blog.csdnimg.cn/942d59ae432141f19d023126be402a8c.png)

​	（2）修改环境变量path，增加Erlang变量至path，%ERLANG_HOME%\bin;

![1585755661841](https://img-blog.csdnimg.cn/e61d76086ec9428bba64e1750060a473.png)

​	（3）打开cmd命令框，输入erl，如果能看到版本号，则Erlang安装完成。

![1585755758154](https://img-blog.csdnimg.cn/129ff1632d434cada4ff44a8a3384153.png)

 

 

#### 8.3.2、安装RabbitMQ



1. 双击资料中提供的 **rabbitmq-server-3.7.7.exe** ，选择对应安装目录，一路下一步，完成安装。

2. 设置环境变量

   (1) 新建RABBITMQ_HOME

![1585756035623](https://img-blog.csdnimg.cn/0229d27b6c2d49139a1a583d4afd3bd7.png)

 

 

​	（2）修改环境变量path，增加rabbitmq变量至path，%RABBITMQ_HOME%\sbin

![1585756139616](https://img-blog.csdnimg.cn/d10084950d61436aa05a7edfd739caac.png)

3. 查看信息。打开cmd命令框，切换至D:\Program Files\rabbitmq_server-3.7.7\sbin目录下，输入rabbitmqctl status

![1585756373625](https://img-blog.csdnimg.cn/c961811c40654940b2c565bab62b225b.png)



4. 安装插件，命令：rabbitmq-plugins.bat enable rabbitmq_management。出现下面信息表示插件安装成功。

![1585756546472](https://img-blog.csdnimg.cn/1be4ff8d6b5d4c65877ba0ff52656a23.png)

#### 8.3.3、启动RabbitMQ

1. 启动RabbitMQ：rabbitmq-server -detached 后台启动

2. 停止RabbitMQ：rabbitmqctl stop

3. rabbitmq启动成功，浏览器中[http://localhost:15672](http://localhost:15672/),默认用户名和密码 都是 guest

至此，rabbitMQ安装部署完成。

### 8.4 Bus 快速入门 

![](https://img-blog.csdnimg.cn/1ef95b5b3d244b6691f48d1941018962.png)

+ 1.分别在 config-server 和 config-client中引入 bus依赖：bus-amqp

+ 2.分别在 config-server 和 config-client中配置 RabbitMQ

+ 3.在config-server中设置暴露监控断点：bus-refresh

  ```sh
  management:
    endpoints:
      web:
        exposure:
          include: 'bus-refresh'
  ```

+ 4.启动测试

  ```sh
  curl -X POST http://localhost:9527/actuator/busrefresh
  ```

  

## 9.Stream 消息驱动

### 9.1 Stream 概述

+ Spring Cloud Stream 是一个构建消息驱动微服务应用的框架。

+ Stream 解决了开发人员无感知的使用消息中间件的问题，因为Stream对消息中间件的进一步封装，可以做到代码层面对中间件的无感知，甚至于动态的切换中间件，使得微服务开发的高度解耦，服务可以关注更多自己的业务流程。

+ Spring Cloud Stream目前支持两种消息中间件RabbitMQ和Kafka

![](https://img-blog.csdnimg.cn/877f2545907243518869d01b807937c2.png)





### 9.4 Stream组件

+ Spring Cloud Stream 构建的应用程序与消息中间件之间是通过绑定器 Binder 相关联的。绑定器对于应用程序而言起到了隔离作用， 它使得不同消息中间件的实现细节对应用程序来说是透明的。

+ binding 是我们通过配置把应用和spring cloud stream 的 binder 绑定在一起

+ output：发送消息 Channel，内置 Source接口

+ input：接收消息 Channel，内置 Sink接口

<img src="https://img-blog.csdnimg.cn/363a795255414017a7e66645611b362b.png" style="zoom:175%;" />



### 9.3 Stream 消息生产者

<img src="https://img-blog.csdnimg.cn/363a795255414017a7e66645611b362b.png" style="zoom:175%;" />

+ 1.创建消息生产者模块，引入依赖 starter-stream-rabbit

+ 2.编写配置，定义 binder，和 bingings

+ 3.定义消息发送业务类。添加 @EnableBinding(Source.class)，注入MessageChannel **output** ，完成消息发送

+ 4.编写启动类，测试

### 9.4 Stream 消息消费者

<img src="https://img-blog.csdnimg.cn/363a795255414017a7e66645611b362b.png" style="zoom:175%;" />

+ 1.创建消息消费者模块，引入依赖 starter-stream-rabbit

+ 2.编写配置，定义 binder，和 bingings

+ 3.定义消息接收业务类。添加 @EnableBinding(Sink.class)，使用@StreamListener(Sink.INPUT)，完成消息接收。

+ 4.编写启动类，测试

## 10.Sleuth+Zipkin 链路追踪

### 10.1 概述

+ Spring Cloud Sleuth 其实是一个工具,它在整个分布式系统中能跟踪一个用户请求的过程，捕获这些跟踪数据，就能构建微服务的整个调用链的视图，这是调试和监控微服务的关键工具。

+ 耗时分析

+ 可视化错误

+ 链路优化

+ Zipkin 是 Twitter 的一个开源项目，它致力于收集服务的定时数据，以解决微服务架构中的延迟问题，包括数据的收集、存储、查找和展现。

<img src="https://img-blog.csdnimg.cn/bec4eff9a7a64f3fad160484e8347b6b.png" style="zoom:150%;" />



### 10.2 Sleuth+Zipkin 快速入门

+ 1.安装启动zipkin。 java –jar zipkin.jar

+ 2.访问zipkin web界面。 http://localhost:9411/

+ 3.在服务提供方和消费方分别引入 sleuth 和 zipkin 依赖

+ 4.分别配置服务提供方和消费方。

+ 5.启动，测试









