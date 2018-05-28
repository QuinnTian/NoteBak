![](./images/timg.jpg)
##  有关AOP的三个名词
|  名称   | 中文    |  例子   |     
| --- | --- | --- | --- |
|   PointCut  |  切入点   | 处理逻辑    |     
|   Joint Point  | 连接点    | 连接点的集合是切入点    |     
|	Adivice |通知|输出日志的代码|
|	Advisor |配置器|将通知注入切入点|

## Spring AOP实现输出日志
### 采用Interceptor Around通知
接口
TimeBookInerface
``` java
package xyz.log.aop.impl;

public interface TimeBookInterface {
    /**
     *
     * 具体执行业务的代码
     */
    public void doAuditing(String name);
}

```
FinanceInterface    
``` java
package xyz.log.aop.impl;

public interface FinanceInterface {
    public void doCheck(String name);
}

```
impl

``` java
package xyz.log.aop.action;

import xyz.log.aop.impl.TimeBookInterface;

public class TimeBook implements TimeBookInterface {
    @Override
    public void doAuditing(String name) {
        //审核数据相关代码
    }
}

```
  
  

``` java
package xyz.log.aop.action;

import xyz.log.aop.impl.FinanceInterface;

public class Finance implements FinanceInterface {
    @Override
    public void doCheck(String name) {

    }
}
```
  
  

``` java
package xyz.log.aop.log;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;
import org.apache.log4j.Level;
import org.apache.log4j.Logger;

/**
 * interceptor around通知会在joint point 前后运行
 */
public class LogAround implements MethodInterceptor {
    private Logger logger = Logger.getLogger(this.getClass().getName());

    /**
     * 负责输出日志的代码
     * @param methodInvocation 可以获得方法名称、程序传入的参数Object[]
     * @return
     * @throws Throwable
     */
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
            logger.log(Level.INFO, methodInvocation.getArguments()[0] + "开始审核数据");
        try {
            //.proceed()可以执行被调用的方法
            Object result = methodInvocation.proceed();
            //返回值是被调用方法的返回值
            return result;

        } finally {
            logger.log(Level.INFO, methodInvocation.getArguments()[0] + "审核数据结束");
        }


    }
}

```  
xml配置  

``` xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--使用Spring aop 实现日志输出的Bean-->
    <!--注册日志bean组件-->
    <bean id="log" class="xyz.log.aop.log.LogAround"/>
    <!--审核程序-->
    <bean id="timeBook" class="xyz.log.aop.action.TimeBook"/>
    <!--使用Spring ProxyFactoryBean来实现代理-->
    <bean id="logProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="proxyInterfaces" >
            <value>xyz.log.aop.impl.TimeBookInterface</value>
        </property>
        <property name="target" >
            <ref bean="timeBook"/>
        </property>
        <!--指定要代理的类-->
        <property name="interceptorNames" >
            <list>
                <value>log</value>
            </list>
        </property>
    </bean>
    <!--财务审核-->
    <bean id="finance" class="xyz.log.aop.action.Finance"/>
    <bean id="logProxy1" class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="proxyInterfaces" >
            <value>xyz.log.aop.impl.FinanceInterface</value>
        </property>
        <property name="target" >
            <ref bean="finance"/>
        </property>
        <!--指定要代理的类-->
        <property name="interceptorNames" >
            <list>
                <value>log</value>
            </list>
        </property>
    </bean>
        

</beans>
```
test

``` java
package xyz.log.aop.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import xyz.log.aop.impl.FinanceInterface;
import xyz.log.aop.impl.TimeBookInterface;

public class TestHelloWorld {
    public static void main(String[] args) {
        //通过application context 获取XML
        ApplicationContext actx = new ClassPathXmlApplicationContext("classpath:SpringConfig.xml");
        TimeBookInterface timeBookProxy = (TimeBookInterface) actx.getBean("logProxy");
        FinanceInterface financeProxy = (FinanceInterface) actx.getBean("logProxy1");
        timeBookProxy.doAuditing("张三");
        financeProxy.doCheck("李四");
    }
}

```








