**JsonFormat与DataTimeFormat**

JsonFormat：后台到前台的时间格式

```
@JsonFormat(pattern="yyyy-MM-dd",timezone = "GMT+8")
```

DataTimeFormat：前台到后台的时间格式

```
@DateTimeFormat(pattern = "yyyy-MM-dd")
```

**Tomcat8中文控制台日志乱码问题？**

找到`..\apache-tomcat-8.5.51\conf\logging.properties`文件修改`java.util.logging.ConsoleHandler.encoding = UTF-8`为`GBK`。

**Maven：Could not find artifact ？**

maven多模块项目构建时，先将parent项目要先install一回，之后子项目才可以运行mvn compile命令,否则就会报如上异常。 

**jar包与war包区别？**

JAR（ Java Archive ）Java归档文件，对类进行打包，引入项目可直接使用jar中的类。

WAR包是web应用程序，直接将war包放入Tomcat的webapps目录下，启动tomcat，自动解压war包为程序文件夹。

```
|- WEB-INF
    |- web.xml
    |- classes
        |- servlet&jsp
    |- lib
        |- 依赖Jar包
```

**sign算法**：

1.客户端已知参数：appKey，**appSercret**，timestamp，param1，value1，param2，value2...

2.通过key&secret生成签名：sign=md5(appKey&appSercret&param1=value1&param2=value2&timestamp)

3.客户端发送请求传参：appKey，timestamp，sign，param1，value1，param2，value2...

4.服务端根据appKey匹配相应的appSercret，根据相同的规则生成签名sign校验一致性。

5.服务端查询redis是否已存在该sign，若已存在，则拒绝请求；若不存在，则缓存sign一段时间。

**包装类 valueOf() 与 parseLong()的区别：**

parseLong(String s)：转为基本数据类型 int、long等

valueOf(String s)：转为基本类型的包装类，由parseLong()包装而来，结果是Integer、Long等包装类。



java8接口可以写默认方法。

bigDecimal会丢失精度嘛?



