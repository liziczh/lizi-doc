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



## Eureka 服务注册

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

EurekaServer 配置：`application.yml`  

```yaml
server:
  port: 8761
spring:
  application:
    name: eureka-server
eureka:
  instance:
    hostname: localhost
  client:
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
    # 是否将自己作为client注册到eureka-server，默认为true
    register-with-eureka: true
    # 是否拉取eureka注册信息，默认为true
    fetch-registry: false
  server:
    # 是否开启自我保护模式，默认为true
    enable-self-preservation: false
    # 续期时间，即扫描失效服务的间隔时间（缺省值为60*1000ms）
    eviction-interval-timer-in-ms: 10000
```

SpringBootApplication：启用EurekaServer

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServerApplication.class, args);
	}
}
```

启动EurekaServer服务，访问 http://localhost:8761/。

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
    # 设置微服务调用地址为IP优先
    prefer-ip-address: true
    # 心跳时间，即服务续约间隔时间，缺省值为30s
    lease-renewal-interval-in-seconds: 30
    # 发呆时间，即服务续约到期时间（缺省为90s）
    lease-expiration-duration-in-seconds: 90
  client:
    service-url:
      # 单机
      defaultZone: http://localhost:8761/eureka/
```

SpringBootApplication：启用EurekaClient

```java
@SpringBootApplication
@EnableEurekaClient
public class EurekaServiceProviderApplication {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServiceProviderApplication.class, args);
	}
}
```

Service Provider Controller 提供服务：

```java
@RestController
@RequestMapping(value = "/provide/")
public class EurekaServiceProviderController {
    @GetMapping(value = "hello")
	public String hello(){
		return "Hello! I'm " + appName + ", My port is " + port;
	}
	@GetMapping(value = "name/{name}")
	public String name(@PathVariable String name){
		return "Hello! My name is " + name;
	}
}
```

Service Consumer Controller 消费服务：

```java
@RestController
@RequestMapping(value = "/consume/")
public class EurekaServiceConsumerController {
	@Bean
	@LoadBalanced
	public RestTemplate restTemplate(){
		return new RestTemplate();
	}

	@Autowired
	private RestTemplate restTemplate;
    
    @GetMapping(value = "hello")
	public String hello(){
		String url = "http://eureka-service-provider:8081/provide/hello";
		return restTemplate.getForObject(url, String.class);
	}
	
	@GetMapping(value = "name/{name}")
	public String get(@PathVariable String name){
		String url = "http://eureka-service-provider:8081/provide/name/"+name;
		return restTemplate.getForObject(url, String.class);
	}
}
```

### Eureka 集群配置

集群：将注册中心分别指向其它注册中心。 

```yaml
# application-peer1.yml
server:
  port: 8001
spring:
  application:
    name: eureka-server
  profiles:
    active: peer1
eureka:
  instance:
    hostname: localhost
  client:
    service-url:
      defaultZone: http://localhost:8002/eureka/,http://localhost:8003/eureka/
```

```yaml
# application-peer2.yml
server:
  port: 8002
spring:
  application:
    name: eureka-server
  profiles:
    active: peer2
eureka:
  instance:
    hostname: localhost
  client:
    service-url:
      defaultZone: http://localhost:8001/eureka/,http://localhost:8003/eureka/
```

```yaml
# application-peer3.yml
server:
  port: 8003
spring:
  application:
    name: eureka-server
  profiles:
    active: peer3
eureka:
  instance:
    hostname: localhost
  client:
    service-url:
      defaultZone: http://localhost:8001/eureka/,http://localhost:8002/eureka/
```

使用IDEA配置三个实例：

```
Program arguments: --spring.profiles.active=peer1
Program arguments: --spring.profiles.active=peer2
Program arguments: --spring.profiles.active=peer3
```

## Feign 远程调用

Feign：服务调用。

ServiceConsumer引入maven依赖：

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-openfeign</artifactId>
    <version>2.2.1.RELEASE</version>
</dependency>
```

FeignClient配置文件：

```yaml
feign:
  client:
    config:
      default:
        connect-timeout: 10000
```

SpringBootApplication：ServiceConsumer启用FeignClients

```java
@EnableEurekaClient
@EnableFeignClients
@SpringBootApplication
public class EurekaServiceConsumerApplication {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServiceConsumerApplication.class, args);
	}
}
```

**FeginClient**：

```java
@Component
@FeignClient(name = "EUREKA-SERVICE-PROVIDER")
public interface FeignService {
	@GetMapping(value = "/provide/hello")
	String hello();
	@GetMapping(value = "/provide/name/{name}")
	String name(@PathVariable String name);
}
```

> 默认情况下，feign只能通过@RequestBody传对象参数，建议service-provider将为浏览器服务的Controller和为消费者服务的Controller分开。

FeignClient注解属性：

- name：服务名称，同value。

- value：服务名称，同name。

- path： 类似于requestMapping。
-  fallback：熔断机制，调用失败时的回调方法。底层依赖hystrix，@EnableHystrix。

### Feign原理

扫包@FeignClient注解类，注入IOC容器。

feign接口被调用时，通过JDK动态代理生成RequestTemplate（包含请求参数、url等），RequestTemplate生成Request交给client处理。client默认为 HTTPUrlConnection，也可以是OKhttp、Apache的HTTPClient等。

最后client封装成LoadBaLanceClient，结合Ribbon负载均衡地发起调用。 

## Ribbon 负载均衡

Ribbon：服务端负载均衡

ServiceProvider引入maven依赖：

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
	<version>2.2.1.RELEASE</version>
</dependency>
```

Ribbon配置类：

```java
@Configuration
public class RibbonConfig {
	@Bean
	public IRule ribbonRule(){
		return new RandomRule(); // 随机选择一个server。
//		return new BestAvailableRule(); // 逐个检查，选择并发请求最小的server。
//		return new RoundRobinRule(); // 轮询选择server。
//		return new WeightedResponseTimeRule(); // 根据响应时间分配权重，权重选择。
//		return new ZoneAvoidanceRule(); // 根据server性能和server可用性选择。
//		return new RetryRule(); // 对选定的负载均衡策略机上重试机制，在一个配置时间段内当选择server不成功，则一直尝试使用subRule的方式选择一个可用的server
	}

	@Bean
	public IPing ribbonPing(){
		return new PingUrl();
	}

	@Bean
	public ServerListSubsetFilter serverListSubsetFilter(){
		ServerListSubsetFilter filter = new ServerListSubsetFilter();
		return filter;
	}
}
```

SpringBootApplication：ServiceProvider添加注解@RibbonClient

```java
@EnableEurekaClient
@RibbonClient(name = "RibbonClient", configuration = RibbonConfig.class)
@SpringBootApplication
public class EurekaServiceProviderApplication {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServiceProviderApplication.class, args);
	}
}
```

ServiceProvider测试：

```java
@RestController
@RequestMapping(value = "/provide/")
public class EurekaServiceProviderController {
	@Value("${spring.application.name}")
	private String appName;
	@Value("${server.port}")
	private String port;

	@GetMapping(value = "hello")
	public String hello(){
		return "Hello! I'm " + appName + ", My port is " + port;
	}
}
```

ServiceConsumer测试：

```java
@Component
@FeignClient(name = "EUREKA-SERVICE-PROVIDER", fallback = FeignServiceFallback.class)
public interface FeignService {
	@GetMapping(value = "/provide/hello")
	String hello();
}
```

ServiceProvider启动两个段端口不同的实例，ServiceConsumer调用，可以看到每次调用port值不同。

## Hystrix 熔断机制

熔断器：消费端监控服务故障，连续多次失败调用通过Fallback进行熔断保护。

ServiceConsumer引入Maven依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
    <version>2.2.1.RELEASE</version>
</dependency>
```

ServiceConsumer配置文件：

```yaml
feign:
  hystrix:
    enabled: true
```

FeignClient：

```java
@Component
@FeignClient(name = "EUREKA-SERVICE-PROVIDER", fallback = FeignServiceFallback.class)
public interface FeignService {
	@GetMapping(value = "/provide/hello")
	String hello();
	@GetMapping(value = "/provide/name/{name}")
	String name(@PathVariable String name);
}
```

FeignClienFallback：

```java
@Component
public class FeignServiceFallback implements FeignService {
	@Override
	public String name(String name) {
		return "Name Error";
	}
	@Override
	public String hello() {
		return "Hello Error";
	}
}
```

## Config 统一配置

**ConfigServer**：

引入maven依赖：

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-config-server</artifactId>
    <version>2.2.1.RELEASE</version>
</dependency>
```

ConfigServer配置文件：`application.yml`  

```yaml
spring:
  application:
    name: config-server
  cloud:
    config:
      server:
        git:
          # git仓库地址
          uri: https://github.com/liziczh/lizi-springcloud
          # git分支
          default-label: master
          # 配置文件根目录
          search-paths: springcloud-config/config
          username: liziczh  # 公开项目，无需配置
          password: xxxxxx   # 公开项目，无需配置
```

SpringBootApplication：

```java
@EnableConfigServer
@SpringBootApplication
public class ConfigServerApplication {
	public static void main(String[] args) {
		SpringApplication.run(ConfigServerApplication.class, args);
	}
}
```

**SpringCloud Config的URL与配置文件的映射关系**：

```yaml
/{application}/{profile}[/{label}]
/{application}-{profile}.yml
/{label}/{application}-{profile}.yml
/{application}-{profile}.properties
/{label}/{application}-{profile}.properties
```

启动项目，按照URL映射关系即可查看配置文件。

**ConfigClient**：

引入maven依赖：

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-config</artifactId>
    <version>2.2.1.RELEASE</version>
</dependency>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

ConfigClient配置文件：`bootstrap.yml` 

> 注意配置文件的名称为bootstrap.yml，bootstrap比application加载早。

```yaml
spring:
  application:
    name: config-client
  cloud:
    config:
      # 配置中心地址
      uri: http://localhost:9001/
      # git分支
      label: master
      # 配置文件的名称
      name: config-client
      # 获取配置的策略
      profile: pro
      # 配置发现
      discovery:
        enabled: true
        # 指定配置中心的service-id
        service-id: config-server
```

ConfigController：

```java
@RestController
@RequestMapping("/config/")
@RefreshScope
public class SpringCloudConfigController {
	@Value(value = "${db.username:admin}")
	private String username;
	@GetMapping(value = "test")
	public String test() {
		return "username:" + username;
	}
}
```

## Gateway 服务网关

Route（路由）：由一个RouteID、目标URL、一组断言、一组过滤器定义。如果断言为真，则路由匹配。

引入maven依赖：

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-gateway</artifactId>
	<version>2.2.1.RELEASE</version>
</dependency>
```

配置文件：`application.yml` 

```yaml
spring:
  application:
    name: gateway-service-a
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true  # 启用注册中心定位网关服务，启用后可直接使用应用名程调用服务
      routes:
          # 路由标识
        - id: gateway-service-a
          # 目标服务URL
          uri: https://www.baidu.com
          # 断言
          predicates:
            - After=2019-01-01T00:00:00+08:00[Asia/Shanghai]
#            - Before=2049-01-01T00:00:00+08:00[Asia/Shanghai]
#            - Between=2019-01-01T00:00:00+08:00[Asia/Shanghai], 2049-01-01T00:00:00+08:00[Asia/Shanghai]
#            - Cookie=sessionId, test
#            - Header=X-Request-Id, \d+
#            - Host=**.baidu.com
#            - Method=GET
#            - Path=/a
#            - Query=name
#            - RemoteAddr=192.168.1.1/24
#          filter:
#            # 截取Path位数
#            - StripPrefix=1
```

 Predicate：匹配成功跳转目标URL。

1.在某个时间之后的请求都进行转发

```yaml
- After=2019-01-01T00:00:00+08:00[Asia/Shanghai]
```

2.在某个时间之前的请求都进行转发

```yaml
- Before=2019-01-01T00:00:00+08:00[Asia/Shanghai]
```

3.在某个时间段的请求都进行转发

```yaml
- Between=2019-01-01T00:00:00+08:00[Asia/Shanghai], 2019-07-01T00:00:00+08:00[Asia/Shanghai]
```

4.Cookie匹配：Cookie Route Predicate

```yaml
- Cookie=sessionId, test
```

测试：`curl http://localhost:8080 --cookie "sessionId=test"`  

5.请求头匹配：Header Route Predicate：

```yaml
- Header=X-Request-Id, \d+
```

测试：`curl http://localhost:8080  -H "X-Request-Id:88"`  

6.Host匹配：Host Route Predicate

```yaml
- Host=**.baidu.com
```

测试：` curl http://localhost:8080  -H "Host: www.baidu.com" `  

7.请求方式匹配：

```yaml
- Method=GET
```

测试：` curl -X GET http://localhost:8080 `

8.请求路径匹配：Path Route Predicate 

```
- Path=/hello
```

测试：` curl http://localhost:8080/hello `  

9.请求参数匹配：

```
- Query=name
```

测试：` curl localhost:8080?name=tom&id=2 `   

10.请求IP地址匹配：

```
- RemoteAddr=192.168.1.1/24
```

测试：` curl localhost:8080 `  

