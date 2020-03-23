## Swagger2

### 引入相关依赖

```xml
<dependency>
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger2</artifactId>
   <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

### Swagger2 配置

**SpringBoot 整合 Swagger2**：

Swagger2Config.java：

```java
/**
 * @Description Swagger2 配置
 * @Author lizi
 * @Date 2018/9/29
 */
@EnableSwagger2 //启用Swagger2功能注解
@Configuration //SpringBoot配置注解
public class Swagger2Config {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2) //文档类型：DocumentationType.SWAGGER_2
                .apiInfo(apiInfo()) //api信息
                .select() //构建api选择器
                .apis(RequestHandlerSelectors.basePackage("com.example")) //api选择器选择api的包
                .paths(PathSelectors.any()) //api选择器选择包路径下任何api显示在文档中
                .build(); //构建文档
    }
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Swagger RESTful API") //页面标题
                .version("1.0") //版本号
                .description("API-描述") //描述
                .build();
    }
}
```

### Swagger2 相关注解

**@Api** - 标注 Controller；

| 属性名称    | 说明                     |
| ----------- | ------------------------ |
| value       | Controller的注解         |
| description | API资源详细描述          |
| hidden      | 配置为true，在文档中隐藏 |

```java
@Api(value = "用户服务",description = "用户相关接口")
```

**@ApiOperation** - 标注 Controller 具体方法；

| 属性名称 | 说明                                        |
| -------- | ------------------------------------------- |
| value    | 接口的名称                                  |
| notes    | 接口的注释                                  |
| response | 接口的返回类型，如：response = String.class |
| hidden   | 配置为true，在文档中隐藏                    |

```java
@ApiOperation(value = "查找用户",notes = "根据用户ID查找用户",response = User.class)
```

**@ApiParam** - 标注 Controller 方法的参数；

| 属性名称     | 说明                |
| ------------ | ------------------- |
| name         | 参数名称            |
| value        | 参数值              |
| required     | 是否必须，默认false |
| defaultValue | 参数默认值          |
| type         | 参数类型            |
| hidden       | 隐藏该参数          |

```java
@ApiParam(name = "用户",value = "实体类User",required = true) User user
```

**@ApiResponses / @ApiResponse** - 标注 Controller 方法的 http 返回状态；

| 属性名称 | 说明                      |
| -------- | ------------------------- |
| code     | http 状态码               |
| message  | 状态的描述信息            |
| response | 状态相应，默认响应类 Void |

```java
@ApiResponses({
    @ApiResponse(code = 200,message = "成功！"),
    @ApiResponse(code = 401,message = "未授权！"),
    @ApiResponse(code = 404,message = "页面未找到！"),
    @ApiResponse(code = 403,message = "出错了！")
})
```