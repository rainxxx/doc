SpringCloud学习笔记

## 1.项目搭建

SpringCloud需要建立在Spring Boot基础上

### 1.1添加以下依赖

```xml
<!-- Spring Cloud -->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Camden.SR4</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```



## 2.服务调用

### 2.1 RestTemplate

```java
@Autowired
private RestTemplate restTemplate;

@GetMapping("getUserList")
	public Object getUserList() {													  
		return restTemplate.getForObject("http://localhost:7000/provider/getUserList", List.class);//这里一定要写对类型
	}
```

### 2.2 服务监控端点

在工程pom.xml中添加如下依赖

```XML
<!-- 为服务添加监控端点 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```



## 3.Eureka

### 3.1 Eureka Server

**所有SpringCloud组件都需要建立在SpringCloud的那个依赖下,详见1.1**

```
<!-- Eureka Server -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka-server</artifactId>
</dependency>
```

**启动类中添加@EnableEurekaServer注解**

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApp {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServerApp.class, args);
	}
}
```

**application.yml配置如下**

```yaml
server:
  port: 8761
  
eureka:
  client:
    register-with-eureka: false #是否将自己注册到Eureka Server中默认为true
    fetch-registry: false #是否从Eureka Server获取注册信息默认为true
    service-url:
      default-zone: http://localhost:8761/eureka/ #与Eureka Server交互的地址,多个地址逗号隔开
```



### 3.2 Eureka Client

**将服务注册到Eureka中**

```xml
<!-- Eureka 客户端 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```

**application.yml**

```yaml
server:
  port: 7000
  
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/coolfuck
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
  application:
    name: microservice-provider-user
    
mybatis:
  mapper-locations: classpath:mapper/*.xml
  
eureka:
  client:
    registerWithEureka: true
    fetchRegistry: true
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
  instance:
    prefer-ip-address: true #表示将自己的ip注册到Eureka Server中
```



**启动程序中添加@EnableDiscoveryClient注解**

```java
@SpringBootApplication
@ComponentScan(basePackages = { "com" })
@MapperScan(basePackages = { "com.mapper" })
@EnableDiscoveryClient
public class SimpleProviderApp {
	public static void main(String[] args) {
		SpringApplication.run(SimpleProviderApp.class, args);
	}
}
```









































