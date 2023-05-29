- Spring有哪些模块

spring core
spring bean
spring mvc
spring data
spring aop
spring context

- 谈谈自己对于 Spring IoC 的了解

依赖注入，创建对象交给spring容器来管理，什么时候创建，什么时候销毁，都由spring来管理

- 什么是 Spring Bean？

交给spring容器管理生命周期的对象

- 将一个类声明为 Bean 的注解有哪些?

@Component
@Service
@Controller
@Bean

- @Component 和 @Bean 的区别是什么？

@Component声明到这个类交给spring托管

@Bean只能声明到方法上，比较灵活，创建类的实现和定制内容交给开发者，只有bean的生命周期还是归spring管理，一般用于加载一些第三方的组件，中间件用它

- 注入 Bean 的注解有哪些？
@Component
@Service
@Controller
@Bean


- Bean 的作用域有哪些?

@Singleton
@ProteType
@Request
@Session
@Websocket

- Bean 的生命周期了解么?

先从配置文件获取bean的定义
反射创建bean实例
如果实现了各种Bean*Aware接口，会在bean创建过程中，执行对应的回调方法，比如创建BeanFactory，BeanName，BeanClassLoader
如果实现了InitialBean接口，会执行afterProperties回调方法
如果在xml里配置了init，会在bean初始化时候执行
同理配置了destroy，会在销毁时执行


- 谈谈自己对于 AOP 的了解

面相切面，就是把与业务不直接相关的通用的逻辑抽象出来，比如监控，日志这些
在方法的执行前后，做一些通用的处理

- AOP通知类型有哪些？

@Before @AfterReturning @AfterThrowing @Around @After

- 多个切面的执行顺序如何控制？

@Order(1)越小，优先级越高
或者实现Ordered接口

- 说说自己对于 Spring MVC 了解?

将web的开发流程分层，让每层专注于自己的内容，互相独立，更好的扩展和理解

- Spring MVC 的核心组件有哪些？

DispatcherServlet 请求处理、分发，响应都靠它
HandlerMapping 通过uri找到对应的Handler
HandlerAdapter 将Handler适配并执行
Handler 对应的Controller，处理真正的业务请求，返回ModelAndView
ViewResolver 解析View，找到真正的视图，通过Model渲染页面

- SpringMVC 工作原理了解吗?

客户端/浏览器发送http请求
DispatcherServlet收到请求，转给HandlerMapping
HandlerMapping通过uri找到对应的Handler
然后HandlerAdapter将Handler封装并执行
Handler里面是Controller里真正的业务逻辑，开始执行
返回ModelAndView，Model是数据 View是逻辑视图
ViewResolver解析View，拿到真正的View，并通过Model渲染页面，最后返回客户端/浏览器

- 统一异常处理怎么做?

@ControllerAdvise + @ExceptionHandler

- Spring 框架中用到了哪些设计模式？

- 工厂模式 BeanFactory
- 代理模式 AOP
- 模版模式 jdbcTemplate
- 适配器模式 SpringMVC
- 单例模式 Spring Bean

- Spring 管理事务的方式有几种？

注解
TrancationManager

- Spring 事务中哪几种事务传播行为?

默认mysql是可重复度 oracle读取已提交

- Spring 事务中的隔离级别有哪几种?

读取未提交 脏读、幻读、
读取已提交
可重复度
串行

- @Transactional(rollbackFor = Exception.class)注解了解吗？

不加的话，只有运行时异常回滚