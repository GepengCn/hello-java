- Spring有哪些模块
spring core
spring context
spring web
spring bean
spring data
spring mvc
- 谈谈自己对于 Spring IoC 的了解

依赖注入，将创建bean的过程交给spring容器来做

- 什么是 Spring Bean？

由spring容器管理生命周期的bean

- 将一个类声明为 Bean 的注解有哪些?

@Component
@Service
@Resposity
@Controller
@RestController

- @Component 和 @Bean 的区别是什么？

@Component修饰类
@Bean修饰方法，更灵活，可以在代码里组装bean，适用于引入一些第三方的组件

- 注入 Bean 的注解有哪些？

@Autowired
@Resource

- Bean 的作用域有哪些?

@Singleton
@ProtoType
@Request
@Session
@Websocket

- Bean 的生命周期了解么?

1. 扫描配置，查询bean的定义
2. 通过反射创建bean
3. 调用set方法，设置bean的属性值
4. 实现Bean*Aware的接口，会执行对应的回调接口，set*方法，比如设置bean名称，类加载器，bean工厂等等
5. 包含BeanPostProcessor接口的类，会执行对应的before和after方法
6. 实现InitialingBean接口的，会执行afterPropertiesSet接口
7. 配置了init或者destroy的会执行里面的方法
8. 实现了DisposableBean接口的会执行对应的destroy方法

- 谈谈自己对于 AOP 的了解

将于业务不直接相关的，通用的业务逻辑抽象出来，比如日志统计，入参校验，出参转换等等
将通用业务与直接业务解耦，更容易扩展

- AOP通知类型有哪些？

@Before
@After
@AfterReturning
@AfterThrowing
@Around

- 多个切面的执行顺序如何控制？

不出错

@Before
@Around
@AfterReturning
@After

出错
@Before
@Around
@Afterthrowing


- 说说自己对于 Spring MVC 了解?

一种设计的模式，为了将我们开发web程序的各个组件解耦，分层，让各个组件独立的扩展和开发

- Spring MVC 的核心组件有哪些？

DispatchServlet
HandlerMapping
HandlerAdapter
Handler
Model
View
ViewResolver

- SpringMVC 工作原理了解吗?

客户端发送请求到服务器
DispatchServelt收到请求，调HandlerMapping解析uri获取对应的Handler
然后使用HandlerAdapter适配+执行Handler
Handler就是实际的Controller，执行完后，返回ModelAndView，Model就是返回的数据，View是个逻辑视图
通过ViewResolver找到真正的视图，通过Model将数据渲染到View，然后返回给客户端

- 统一异常处理怎么做?

@ControllerAdvise+@ExceptionCaught

- Spring 框架中用到了哪些设计模式？

工厂模式BeanFactory
单例模式Singleton
模版模式 JdbcTemplate RedisTemplate
适配器模式HandlerAdapter


- Spring 管理事务的方式有几种？

注解@Transcation
TranscationManager

- Spring 事务中哪几种事务传播行为?



- Spring 事务中的隔离级别有哪几种?

默认：mysql是可重复度 oracle 读取已提交

- @Transactional(rollbackFor = Exception.class)注解了解吗？

不加的话，默认是运行时异常才会回滚