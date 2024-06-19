## Disable default security

This is done in the primary Spring application class' annotation

```java
@SpringBootApplication(exclude = SecurityAutoConfiguration.class)  
public class SpringlesApplication {  
  
    public static void main(String[] args) {  
       SpringApplication.run(SpringlesApplication.class, args);  
    }  
  
}
```

add `exclude` directive to `SpringBootApplication` annotation.

## [[Authentication|Authentication]]