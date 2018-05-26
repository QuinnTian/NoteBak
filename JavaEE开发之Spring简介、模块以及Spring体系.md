### 什么是Spring 

 - Sping 是一个轻量级企业开发的一站式解决方案，用于解决Java EE开发中的所有问题。Spring框架主要提供IOC容器、AOP、数据访问、WEB开发、消息、测试相关技术的支持
 - Spring 采用简单的POJO来进行企业级开发。
 - POJO：Plain Ordinary Java Object简单的Java对象，实际就是普通JavaBeans，是为了避免和EJB混淆所创造的简称。
 - Java对象->Bean<-IOC容器来初始化，提供依赖管理和对象使用。
### Spring中的模块
![Spring中各个模块](./images/1527353485719.jpg)

**核心容器**  
 - Spring-Core：核心工具类  
 - Spring-Beans：Spring定义bean的支持  
 - Spring-Context：运行时Spring容器  
 - Spring-Context：Spring容器对第三方包的集成支持  
 - Spring-Expression：使用表达式语言在进行查询时和操作的对象
**AOP**  
 - Spring-AOP：基于代理的AspectJ的AOP支持  
 - Spring-Aspects：基于AspectJ的AOP支持  
**消息**    
 - Spring-messaging：对消息架构和协议的支持  
**Web** 
 - Spring-Web：基础Web集成功能，Web项目的容器
 - Spring-Webmvc：基于Servlet的SpringMVC
 - Spring-WebSocket：提供Websocket功能
 - Spring-Webmvc-Portlet：提供Porlet环境支持  
**数据访问/集成** 
 - Spring-JDBC：提供以JDBC访问数据库的支持
 - Spring-TX：提供编程式和声明式的事务支持  
 - Spring-ORM：对象-关系映射支持  
 - Spring-OXM：对象-XML映射支持  
 - Spring-JMS：提供JMS支持
   
### Spring中的模块

 - Spring Framework(Core)： Spring的核心项目,其中包含了一系列的IOC容器的设计，提供了依赖注入的实现；同时,还集成了AOP,提供了面向切面编程的实现;当然还有MVC、JDBC、事务处理模块的实现。目前官网最高版本4.3.0
 - Spring Boot :提供了快速构建Spring应用,提供开发效率,达到 开箱即用---- 快速开始需求开发而不被其他方面影响 “即时运行”。
 - Spring Batch:提供构建批处理应用和自动化操作的框架，专门用于离线分析程序,数据批处理等场景。
 - Spring Data:提供使用非关系型数据的能力,比如当基础数据并非存储在关系数据库中,或MapReduce中的分布式存储、云计算存储环境等 
 - Spring Security:用户认证、授权、安全服务等工具,最先前在Spring社区中的名字是Acegi框架。
 - Spring Security OAuth:OAuth是一个第三方的模块,提供一个开放的协议的实现,通过这个协议前端桌面应用可以对web应用进行简单而标准的安全调用
 - Spring Web Flow:Web工作流引擎,定义了一种特定的语言来描述工作流,同时高级的工作流控制器引擎可以管理会话状态。
 - Spring BlazeDS Integration :提供Spring与Adobe Flex技术集成的模块。
 - Spring Dynamic Modules:提供Spring 应用运行在OSGi平台上 OSGi面向java的动态模型系统,Eclipse就是构建在OSGi平台上的。
 - Spring Intergration:为企业的数据集成提供了解决方案,
 - Spring AMQP:高级消息队列协议,支持java 和.NET两个版本。SpringSoruce旗下的Rabbit MQ就是一个开源的AMQP的消息服务器,Rabbit MQ 是用Erlang语言开发的。
 - Spring .NET：为.NET提供Spring相关的技术支持,如IOC容器、AOP等。
 - Spring Android:为Android终端开发应用提供Sring支持。
 - Spring Mobile:为移动终端的服务器应用开发提供支持
 - Spring Social：Spring框架的扩展,提供了SNS服务,如FaceBook和Twitter服务