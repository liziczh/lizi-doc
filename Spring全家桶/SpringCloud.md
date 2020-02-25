# SpringCloud

SpringCloud 是一系列框架的有序集合。

服务注册与发现：Eureka，Zookeeper，Cloud Foundry，Consul。

服务调用：Ribbon，Feign。

熔断器：Hystrix。

路由网关：Zuul，Gateway。

配置中心：Config。

消息总线：Bus。

服务链路追踪：Sleuth。

Spring Cloud Netflix Eureka：基于 REST 的服务注册与发现中心。

Spring Cloud Netflix Hystrix：熔断器。

Spring Cloud Netflix Zuul：服务网关。

Spring Cloud Config：配置中心。

Spring Cloud Bus：消息总线。

Spring Cloud Consul：分布式服务配置与注册中心。

常用：

服务注册与发现：Eureka

服务调用：Fegin。

负载均衡：Ribbon。

配置中心：Config。

服务网关：Gateway。



## Eureka

Eureka，服务中心 / 注册中心，管理各种服务功能包括服务的注册、发现、熔断、负载、降级等。

添加依赖

```xml
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-netflix-eureka-server</artifactId>
   <version>2.0.0.RELEASE</version>
</dependency>
```

**Eureka 配置**：

```yaml
eureka:
  instance:
    hostname: localhost  # eureka实例的主机名
  client：
    register-with-eureka: false # 是否向服务注册中心注册自己
    fetch-registry: false # 是否抓取服务注册表信息
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```



## Hystrix 熔断器

熔断器原理：快速失败。

```
feign.hystrix.enabled=true
```

Config Server：

```
@EnableDiscoveryClient
@EnableConfigServer
```

配置文件（Server）：

```yaml
server:
server:
  port: 8001
spring:
  application:
    name: spring-cloud-config-server
  cloud:
    config:
      server:
        git:
          uri: https://github.com/ityouknow/spring-cloud-starter/  # 配置git仓库的地址
          search-paths: config-repo     # git仓库地址下的相对地址，可以配置多个，用,分割。
          username: username            # git仓库的账号
          password: password            # git仓库的密码
```

Config Client：

```
@EnableDiscoveryClient
```

配置文件（client）：

```properties
spring.application.name=spring-cloud-config-client
server.port=8002

spring.cloud.config.name=neo-config
spring.cloud.config.profile=dev
spring.cloud.config.label=master
spring.cloud.config.discovery.enabled=true
spring.cloud.config.discovery.serviceId=spring-cloud-config-server

eureka.client.serviceUrl.defaultZone=http://localhost:8000/eureka/
```