# Maven

Maven 项目构建工具



## Maven 配置

（1）下载 apache-maven-x.x.x-bin；

（2）配置 maven 环境变量：

- MAVEN_HOME：`D:/Code/Java/Maven/maven`；

- Path：`%MAVEN_HOME%/bin`；

（3）**maven/conf/settings.xml**：

- 本地 maven 仓库地址：

```xml
<localRepository>D:\Code\Java\Maven\maven-repo</localRepository>
```

- 阿里云 maven 镜像：

```xml
<mirror>
   <id>nexus-aliyun</id>
   <mirrorOf>*</mirrorOf>
   <name>Nexus aliyun</name>
   <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror> 
```
## Maven 依赖

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```