# HTTP服务器

Spring Boot是一个快速构建Java应用程序的框架，它提供了内置的HTTP服务器功能。使用Spring Boot可以轻松地创建RESTful API和Web应用程序。它提供了丰富的功能和配置选项，使您能够快速启动一个HTTP服务器。

Spring Boot支持多种HTTP服务器，其中一些常见的包括：

1.  **Apache Tomcat**：Spring Boot默认的内嵌Servlet容器就是Apache Tomcat。您可以直接使用Spring Boot创建的应用程序内嵌Tomcat，而无需单独安装和配置Tomcat服务器。
2.  **Jetty**：Spring Boot也支持使用Jetty作为内嵌的Servlet容器。您可以通过相应的依赖和配置来切换到Jetty作为应用程序的HTTP服务器。
3.  **Undertow**：Undertow是另一个受欢迎的Servlet容器和Web服务器，Spring Boot也对其提供了支持。您可以将Undertow配置为Spring Boot应用程序的内嵌服务器。

版本选择

> JDK1.8=springboot2.7.8
> JDK17=3.0.2

pom.xml

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>3.1.2</version>
        <dependency>
           
```

spring boot配置文件src/main/resouces

```ini
server.port=8080
```

简易的http请求处理器

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloWorldController {

    @GetMapping("/")
    public String helloWorld() {
        return "Hello, World!";
    }
}
```

启动类

```java
@SpringBootApplication
public class YourApplication {

    public static void main(String[] args) {
        SpringApplication.run(YourApplication.class, args);
    }
}
```

[获取请求参数](获取请求参数/获取请求参数.md "获取请求参数")

[application.properties](application.properties/application.properties.md "application.properties")

[获取请求的数据](获取请求的数据/获取请求的数据.md "获取请求的数据")
