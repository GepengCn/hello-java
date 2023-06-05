- 为什么要有SpringBoot

1. 减少传统服务xml配置的繁琐
2. 开发web服务更容易
3. 更轻量

- SpringBoot的主要优点

1. 自动装配
2. 集成了tomcat等web容器
3. 与Spring全家桶集成更方便
4. 使用Maven或者gradle管理项目，构建项目更简单
5. 简化了web服务的构建流程，更轻量

- 什么是Spring Boot Starters

一些列组件依赖的集成，通过引用它，可以简化引入某个组件的繁琐过程，因为它已经把这些依赖引用好了

- Spring Boot支持哪些内嵌的容器

tomcat、jetty、undertow

- 介绍下@SpringBootApplication注解

@EnableAutoConfig：启用自动装配
@ComponentScan


- Spring Boot 自动装配是如何实现的？

使用注解@SpringBootApplication

- 开发web服务常用注解有哪些?

@Component
@Service
@Resposity
@Entity
@Bean
@Controller
@RestController
@RequestMapping、@GetMapping
@RequestParam
@RequestBody
@ResponseBody
@PathVariable
@Value
@Configuration

- Spring Boot 常用读取配置文件的方法有哪些？

@Value


- Spring Boot 加载配置文件的优先级了解吗？

resouces/config
resource/
classpath/config
classpath

- 常用的Bean映射工具有哪些？

BeanUtils BeanUtil

