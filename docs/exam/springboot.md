# SpringBoot

## 问题

- 为什么要有SpringBoot
- SpringBoot的主要优点
- 什么是Spring Boot Starters
- Spring Boot支持哪些内嵌的容器
- 介绍下@SpringBootApplication注解
- Spring Boot 自动装配是如何实现的？
- 开发web服务常用注解有哪些?
- Spring Boot 常用读取配置文件的方法有哪些？
- Spring Boot 加载配置文件的优先级了解吗？
- 常用的Bean映射工具有哪些？

## 答案

### 为什么要有SpringBoot

!!! tip "为什么要有SpringBoot"

    简化spring开发，减少配置文件，开箱即用

### SpringBoot的主要优点

!!! tip "SpringBoot的主要优点"

    1. 不需要编写大量样板代码、XML配置
    2. 很容易与Spring生态集成，Spring JDBC、Spring MVC等
    3. 内置http服务器
    4. 多种插件集成，比如maven、Graddle，管理项目更容易


### 什么是Spring Boot Starters

!!! tip "什么是Spring Boot Starters"

    一系列依赖关系的集合，比如Spring-boot-starter-web，依赖好了必备的库，比如Spring MVC、Tomcat、Jackson等

### Spring Boot支持哪些内嵌的容器

!!! tip "Spring Boot支持哪些内嵌的容器"

    Tomcat,Jetty,Undertow

### 介绍下@SpringBootApplication注解

!!! tip "介绍下@SpringBootApplication注解"

    可以看做是`@Configuration`、`@EnableAutoConfiguration`、`@ComponentScan`注解的集合
    
    - `@EnableAutoConfiguration`：启用Springboot自动装配
    - `@ComponentScan`：扫描被`@Component`、`@Service`、`@Controller`注解的bean，默认扫描该类所在包下所有的类
    - `@Configuration`：指明这个类，里面有@bean方法，可以被Spring容器管理


### Spring Boot 自动装配是如何实现的？

!!! tip "Spring Boot 自动装配是如何实现的？"

    由@SpringBootApplication来实现的


### 开发web服务常用注解有哪些?

!!! tip "开发web服务常用注解有哪些?"

    - @Autowired
    - @RestController、@Controller
    - @Component
    - @Service
    - @GetMapping、@PostMapping
    - @RequestParam、@Pathvariable、@RequestBody

### Spring Boot 常用读取配置文件的方法有哪些？

!!! tip "Spring Boot 常用读取配置文件的方法有哪些？"

    - @Value读取
    - @ConfigurationProperties+@Component+@Autowired(引用)
    - @ConfigurationProperties+@EnableConfigurationProperties(ProfileProperties.class)修饰Application
    

### Spring Boot 加载配置文件的优先级了解吗？

!!! tip "Spring Boot 加载配置文件的优先级了解吗？"

    1. classpath下的/config目录
    2. classpath根目录
    3. resources下的/config目录
    4. resources根目录

### 常用的Bean映射工具有哪些？

!!! tip "常用的Bean映射工具有哪些？"

    - Spring BeanUtils
    - Apache BeanUtils
    - MapStruct
    - Hutool