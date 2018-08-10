# SpringBoot环境搭建

## 1.搭建前准备

### 1.1拷贝setting.xml文件

将maven的setting.xml文件拷贝至用户目录的.m2文件夹下，例如：**C:\Users\Administrator\.m2**



### 1.2setting.xml配置信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd"> 
	
	<!--Maven下载的Jar包存放位置-->
	<localRepository>D:\\repository</localRepository>

    <mirrors>  
	  <!-- 使用阿里云镜像代理Maven中央仓库，提升下载Jar包速度 -->
      <mirror>  
        <id>alimaven</id>  
        <name>aliyun maven</name>  
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
        <mirrorOf>central</mirrorOf>          
      </mirror>  
    </mirrors>
  
</settings>

```



## 2.创建SpringBoot工程

### 2.1 pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>guyang</groupId>
	<artifactId>microservice-simple-provider-user</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>microservice-simple-provider-user</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.3.RELEASE</version>
		<relativePath/> 
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.2</version>
		</dependency>

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>

```



### 2.2  application.yml

```yaml
# Tomcat端口设置
server:
  port: 80
  
# Mysql数据源配置
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/coolfuck
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver

# Mybatis配置
mybatis:
  mapper-locations: classpath:mapper/*.xml
```



## 3. Mybatis相关

### 3.1 mapper 接口

```
@Mapper
public interface UserMapper {

	public List<User> getList();
	
}
```

**记住！接口上必须加@Mapper注解,不用SpringBoot整合可以不加,但是SpringBoot整合必须家加**

### 3.2 Mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mapper.UserMapper">

    <select id="getList" resultType="com.entity.User">
        SELECT * FROM user;
    </select>

</mapper>
```







## 4.Junit测试

### 4.1测试

```
@SpringBootTest(classes= {ProviderUserApp.class})
@RunWith(SpringRunner.class)
@ComponentScan(basePackages= {"com.mapper"})
public class AppTest {

	@Autowired
	private UserMapper userMapper;
	
	@Test
	public void test() {
		List<User> list = userMapper.getList();
		System.out.println(list.toString());
	}
	
}
```



