- 为什么要有SpringBoot

部署服务更简单了，更轻量，也没有那些xml配置了，集成spring组件甚至其他的组件也更加的简单，兼容性更好，与maven和Graddle这个项目构建工具集成也更好，各种注解、配置式的开发，更加的便利

- SpringBoot的主要优点

构建web服务更加的简单
自动装配+maven构建，让集成各种第三方组件更容易兼容性也更好，配置也简单多了
与Spring全家桶集成更好
注解+配置，让开发更容易
SpringBoot也让服务更清量，从选型到集成第三方，到最后构建非常快捷方便

- 什么是Spring Boot Starters

一系列依赖的合集

- Spring Boot支持哪些内嵌的容器

tomcat
jetty
undertow

- 介绍下@SpringBootApplication注解

三个注解构成的
@EnableAutoConfig 启用自动装配
@ComponentScan 扫描该类包下的所有bean，交给Spring容器来做ioc


- Spring Boot 自动装配是如何实现的？
@SpringBootApplication注解


- 开发web服务常用注解有哪些?

@Component @Service @Respository @Controller @RestController @Bean

@RequestMapping @GetMapping

@RequestParam @ResponseBody @Pathvariable

@Autowised
@Qualified
@Conditional
@Configuration

- Spring Boot 常用读取配置文件的方法有哪些？

@Value
@Configuration+@Component


- Spring Boot 加载配置文件的优先级了解吗？
根目录/config
优先根目录
resources/config
resources /

- 常用的Bean映射工具有哪些？

Spring.BeanUtils
MapStruct
Hutool BeanUtil