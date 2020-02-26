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