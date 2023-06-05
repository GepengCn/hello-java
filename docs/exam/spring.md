# Spring

## 问题

- Spring有哪些模块
- 谈谈自己对于 Spring IoC 的了解
- 什么是 Spring Bean？
- 将一个类声明为 Bean 的注解有哪些?
- @Component 和 @Bean 的区别是什么？
- 注入 Bean 的注解有哪些？
- Bean 的作用域有哪些?
- Bean 的生命周期了解么?
- 谈谈自己对于 AOP 的了解
- AOP通知类型有哪些？
- 多个切面的执行顺序如何控制？
- 说说自己对于 Spring MVC 了解?
- Spring MVC 的核心组件有哪些？
- SpringMVC 工作原理了解吗?
- 统一异常处理怎么做?
- Spring 框架中用到了哪些设计模式？
- Spring 管理事务的方式有几种？
- Spring 事务中哪几种事务传播行为?
- Spring 事务中的隔离级别有哪几种?
- @Transactional(rollbackFor = Exception.class)注解了解吗？

## 答案

### Spring有哪些模块


!!! tip "Spring有哪些模块"

    - [x] 核心模块
    
    - spring-core:核心工具类
    - spring-beans:bean的创建、管理
    - spring-context:资源加载、事件传播、国际化
    - spring-expression:el表达式
    
    - [x] AOP
    
    - spring-aspects: 集成AspectJ
    - spring-aop:面向切面的编程实现
    
    - [x] 数据连接层
    
    - spring-jdbc:数据库访问
    - spring-tx:事务
    - spring-orm:对orm框架支持
    
    - [x] Spring Web
    
    - spring-web:web实现
    - spring-webmvc:Spring MVC
    - spring-websocket:websocket支持
    
    - [x] Spring Test
    
    继承了各种常用的测试框架
    
    

### 谈谈自己对于 Spring IoC 的了解

!!! tip "谈谈自己对于 Spring IoC 的了解"

    控制反转，创建对象不是我们自己来做，还是交给spring容器来创建、管理依赖关系。



### 什么是 Spring Bean？

!!! tip "什么是 Spring Bean？"

    IoC 容器所管理的对象。

### 将一个类声明为 Bean 的注解有哪些?

!!! tip "将一个类声明为 Bean 的注解有哪些?"

    `@Component`,`@Repository`,`@Service`,`@Controller`

### @Component 和 @Bean 的区别是什么？

!!! tip "@Component 和 @Bean 的区别是什么？"


    - @Component 注解作用于类，而@Bean注解作用于方法。
    - 当我们引用第三方库中的类需要装配到 Spring容器时，则只能通过 @Bean来实现

### 注入 Bean 的注解有哪些？

!!! tip "注入 Bean 的注解有哪些？"

    - @Autowired: Spring 内置默认byType，接口多个实现类可能会冲突，需要@Qualifier来区分name
    - @Resource：jdk提供的，默认byName，无法通过ByName才会byTime

### Bean 的作用域有哪些?

!!! tip "Bean 的作用域有哪些?"

    - singleton，单例，默认
    - prototype，新创建实例
    - request, session, websocket都是特定周期内创建，不在周期内销毁

### Bean 的生命周期了解么?

!!! tip "Bean 的生命周期了解么?"

    - 根据配置找bean的定义
    - 反射创建bean的实例
    - set bean的属性值
    - 如果实现了Bean**Aware接口，回调对应的set方法，在里面传入一些自定义的，比如beanName，类加载器，bean工厂
    - 如果有实现BeanPostProcessor接口的对象使用，会执行里面的before和after方法
    - 如果实现InitializingBean会执行afterPropertiesSet
    - 如果配置文件里配置了 init-method和destroy，会执行对应方法，destroy会在bean销毁时执行
    - 如果实现了DisposableBean接口，销毁bean的时候会执行destroy方法


### 谈谈自己对于 AOP 的了解

!!! tip "谈谈自己对于 AOP 的了解"

    与业务不直接相关的，通用的逻辑抽象出来，减少重复的代码，降低耦合，不影响业务的情况下，也容易扩展

    如果有实现接口，使用jdk的代理，没有实现的用cglib

    也可以用AspectJ，AspectJ是编译时增强，Spring AOP是运行时增强

### AOP通知类型有哪些？

!!! tip "AOP通知类型有哪些？"

    - 前置，调用方法前
    - 后置，调用方法后
    - 正常返回,返回结果后触发
    - 异常
    - 环绕，拦截方法前后

    程序执行正常：

    1、环绕通知前
    2、@Before通知
    3、程序逻辑
    4、@AfterReturning通知
    5、@After通知
    6、环绕通知后

    程序执行异常：

    1、环绕通知前
    2、@Before通知
    3、@AfterThrowing异常通知
    4、@After通知

### 多个切面的执行顺序如何控制？

!!! tip "多个切面的执行顺序如何控制？"

    - @Order(3)注解，越小越早
    - 实现Ordered 接口重写 getOrder 方法。


### 说说自己对于 Spring MVC 了解?

!!! tip "说说自己对于 Spring MVC 了解?"

    将业务的逻辑、数据、显示分离，可以说是一种设计模式，简化web开发，甚至springmvc，我们分为service、dao、entity、controller层

### Spring MVC 的核心组件有哪些？

!!! tip "Spring MVC 的核心组件有哪些？"

    - DispatcherServlet:接收请求、分发，响应客户端
    - HandlerMapping:根据uri匹配Handler
    - HandlerAdapter:适配执行对应的 Handler
    - Handler:实际处理，就是controller
    - ViewResolver:根据handler返回的逻辑视图，找到并渲染真正的视图，并响应客户端

### SpringMVC 工作原理了解吗?

!!! tip "SpringMVC 工作原理了解吗?"

    1. 客户端/浏览器发送请求
    2. DispatcherServlet 调用HandlerMapping匹配handler
    3. DispatcherServlet调用HandlerAdapter适配执行handler
    4. Handler处理完返回ModelAndView给DispatcherServlet,model是数据，view是逻辑视图
    5. ViewResolver根据view找到实际的View
    6. DispaterServlet将Model给View进行视图渲染
    7. 响应客户端

### 统一异常处理怎么做

!!! tip "统一异常处理怎么做"

    @ControllerAdvice + @ExceptionHandler

    - @ControllerAdvice：指定controller
    - @ExceptionHandler：指定方法拦截什么异常

### Spring 框架中用到了哪些设计模式？

!!! tip "Spring 框架中用到了哪些设计模式？"

    - 工厂:BeanFactory
    - 代理:Spring AOP
    - 单例:bean
    - 模板方法:jdbcTemplate
    - 适配器模式:spring mvc


### Spring 管理事务的方式有几种？

!!! tip "Spring 管理事务的方式有几种？"

    - 硬编码：TransactionTemplate或TransactionManager
    - 注解:@Transactional


### Spring 事务中哪几种事务传播行为?

!!! tip "Spring 事务中哪几种事务传播行为?"

    1. 存在事务，加入，不存在创建新的
    2. 创建新的，如果已存在，则挂起存在，相互独立
    3. 存在，创建嵌套事务，不存在创建事务
    4. 存在则加入，不存在抛异常

### Spring 事务中的隔离级别有哪几种?

!!! tip "Spring 事务中的隔离级别有哪几种?"

    多了一个默认，mysql采用REPEATABLE_READ,oracle采用READ_COMMITTED

### @Transactional(rollbackFor = Exception.class)注解了解吗？

!!! tip "@Transactional(rollbackFor = Exception.class)注解了解吗？"

    不加rollback，只有运行时异常回滚