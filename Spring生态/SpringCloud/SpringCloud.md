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











