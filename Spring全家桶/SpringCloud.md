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

### EurekaServer

添加Maven依赖

```xml
<dependencies>
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
		<version>2.2.1.RELEASE</version>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-test</artifactId>
		<scope>test</scope>
		<exclusions>
			<exclusion>
				<groupId>org.junit.vintage</groupId>
				<artifactId>junit-vintage-engine</artifactId>
			</exclusion>
		</exclusions>
	</dependency>
</dependencies>
<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-dependencies</artifactId>
			<version>Hoxton.SR1</version>
			<type>pom</type>
			<scope>runtime</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
```

> 注意springboot和springcloud的版本对应关系，本处使用的是Springboot parent 2.2.4 和 SpringCloud Hoxton.SR1。

Eureka Server 配置：

```yaml
server:
  port: 8761
spring:
  application:
    name: eureka-server
eureka:
  instance:
    hostname: localhost
  server:
    enable-self-preservation: false # 是否开启自我保护模式，默认为true
    eviction-interval-timer-in-ms: 10000 # 续期时间，即扫描失效服务的间隔时间（缺省值为60*1000ms）
  client:
    register-with-eureka: false # 是否将自己作为client注册到eureka-server，默认为true
    fetch-registry: false # 是否拉取eureka注册信息，默认为true
    service-url:
      default-zone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

启用EurekaServer

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServerApplication.class, args);
	}
}
```

### Service

引入maven依赖：

```xml
<dependencies>
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		<version>2.2.1.RELEASE</version>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-test</artifactId>
		<scope>test</scope>
		<exclusions>
			<exclusion>
				<groupId>org.junit.vintage</groupId>
				<artifactId>junit-vintage-engine</artifactId>
			</exclusion>
		</exclusions>
	</dependency>
</dependencies>
<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-dependencies</artifactId>
			<version>Hoxton.SR1</version>
			<type>pom</type>
			<scope>runtime</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
```

Service配置：

```yaml
server:
  port: 8081
spring:
  application:
    name: eureka-service-provider
eureka:
  instance:
    instance-id: ${spring.application.name}:${server.port}
    prefer-ip-address: true # 设置微服务调用地址为IP优先
    lease-renewal-interval-in-seconds: 30 # 心跳时间，即服务续约间隔时间，缺省值为30s
    lease-expiration-duration-in-seconds: 90 # 发呆时间，即服务续约到期时间（缺省为90s）
  client:
    service-url:
      default-zone: http://localhost:8761/eureka/
```

启用EurekaClient

```java
@SpringBootApplication
@EnableEurekaClient
public class EurekaServiceProviderApplication {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServiceProviderApplication.class, args);
	}
}
```

Service Provider：

```java
@RestController
public class EurekaServiceProviderController {

	@GetMapping(value = "/out/{value}")
	public String provide(@PathVariable String value){
		return "EurekaServiceProvider::" + value;
	}
}
```

Service Provider：

```java
@RestController
public class EurekaServiceConsumerController {
	@Bean
	@LoadBalanced
	public RestTemplate restTemplate(){
		return new RestTemplate();
	}

	@Autowired
	private RestTemplate restTemplate;

	@GetMapping(value = "/get/{value}")
	public String get(@PathVariable String value){
		String url = "http://eureka-service-provider:8001/out/"+value;
		return restTemplate.getForObject(url, String.class);
	}
}
```

### 集群配置

集群：将注册中心分别指向其它注册中心。 

```yaml
server:
  port: 8001
spring:
  application:
    name: eureka-server
  profiles: peer1
eureka:
  instance:
    hostname: peer1
  client:
    service-url:
      default-zone: http://peer2:8002/eureka/,http://peer3:8003/eureka/
---
 server:
  port: 8002
spring:
  application:
    name: eureka-server
  profiles: peer2
eureka:
  instance:
    hostname: peer2
  client:
    service-url:
      default-zone: http://peer1:8001/eureka/,http://peer3:8003/eureka/
---
 server:
  port: 8003
spring:
  application:
    name: eureka-server
  profiles: peer3
eureka:
  instance:
    hostname: peer3
  client:
    service-url:
      default-zone: http://peer1:8001/eureka/,http://peer2:8002/eureka/
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