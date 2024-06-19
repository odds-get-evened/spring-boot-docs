This document will give the bare minimum basics for setting Spring Boot up

## Required dependencies

```xml
...
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-data-jpa</artifactId>  
</dependency>  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-security</artifactId>  
</dependency>  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-thymeleaf</artifactId>  
</dependency>  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-web</artifactId>  
</dependency>  
<dependency>  
    <groupId>org.thymeleaf.extras</groupId>  
    <artifactId>thymeleaf-extras-springsecurity6</artifactId>  
</dependency>  
  
<!-- MariaDB connector --> 
<!--
<dependency>  
    <groupId>org.mariadb.jdbc</groupId>  
    <artifactId>mariadb-java-client</artifactId>  
    <version>2.1.2</version>  
</dependency>
-->  
  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-test</artifactId>  
    <scope>test</scope>  
</dependency>  
<dependency>  
    <groupId>org.springframework.security</groupId>  
    <artifactId>spring-security-test</artifactId>  
    <scope>test</scope>  
</dependency>
...
```

## Application settings

There are two ways to set this up using either a `.properties` file, or preferred, a `.yml` file.

```yaml
spring:  
  application:  
    name: springles 
  # using a MariaDB connection here (not required for initial setup) 
  datasource:  
    url: jdbc:mariadb://localhost:3306/spring_practice  
    username: springuser  
    password: springuser  
    driver-class-name: org.mariadb.jdbc.Driver  
  jpa:  
    hibernate:  
      ddl-auto: validate
```

## [[Security|Security stuff]]

## Database Stuff

### [[Respositories|Repositories]]

### [[Entities|Entities]]

## [[Templates|Thymeleaf/Templating/HTML]]
