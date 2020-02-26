**Tomcat8中文控制台日志乱码问题？**

找到apache-tomcat-8.5.51\conf\logging.properties文件修改java.util.logging.ConsoleHandler.encoding = UTF-8为GBK。

