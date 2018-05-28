通用日志输出：在每个类中都要添加具体的日志输出代码  
面向接口日志输出：实现了业务逻辑与日志代码的分离，但还是要依赖具体的接口
Java代理日志输出：真正实现对日志代码重用，且不依赖具体的接口

### 通用日志输出

``` java
package xyz.log.gen;
public class TestHelloWorld {
public static void main(String[] args) {
        TimeBook timeBook = new TimeBook();
        timeBook.doAuditing("张三");
    }
}
```

``` java
package xyz.log.gen;
import org.apache.log4j.Level;
import org.apache.log4j.Logger;
public class TimeBook {
    private Logger logger = Logger.getLogger(this.getClass().getName());
    //审核数据相关程序
    public void doAuditing(String name){
        logger.log(Level.INFO ,name + "开始审核数据");
        //审核数据相关程序
        logger.log(Level.INFO ,name + "审核数据结束");
    }
}
```

### 面向编程接口进行日志输出
**接口**
``` java
package xyz.log.impl;

public interface TimeBookInterface {
    public void doAuditing(String name);
}

```
**实现接口**

``` java
package xyz.log.impl;

public class TimeBook implements TimeBookInterface {
    @Override
    public void doAuditing(String name) {
        //审核数据相关程序
    }
}
```
**代理类**

``` java
package xyz.log.impl;

import org.apache.log4j.Level;
import org.apache.log4j.Logger;

/**
 * Java代理类
 * 用来实现日志输出
 */
public class TimeBookProxy {
    private Logger logger = Logger.getLogger(this.getClass().getName());

    private TimeBookInterface timeBookInterface;
    //在该类中针对前面的接口编程，而不针对具体的类
    public TimeBookProxy(TimeBookInterface timeBookInterface){
        this.timeBookInterface = timeBookInterface;
    }
    //实际业务的处理
    public  void  doAuditing(String name){
        logger.log(Level.INFO,name+"开始审核数据");
        timeBookInterface.doAuditing(name);
        logger.log(Level.INFO,name+"审核数据结束");
    }

}

```
**测试类**

``` java
package xyz.log.impl;

public class TimeBook implements TimeBookInterface {
    @Override
    public void doAuditing(String name) {
        //审核数据相关程序
    }
}
```

### Java代理机制实现日志输出
**代理类**

``` java
package xyz.log.java;

import org.apache.log4j.Level;
import org.apache.log4j.Logger;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;


public class LogProxy implements InvocationHandler {
    private Logger logger = Logger.getLogger(this.getClass().getName());
    private Object delegate;
    //绑定代理对象
    public Object bind(Object delegate){
        this.delegate = delegate;
        return Proxy.newProxyInstance(delegate.getClass().getClassLoader(),delegate.getClass().getInterfaces(),this);
    }
    //针对接口编程
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object result = null;
        //方法调用前进行日志输出
        logger.log(Level.INFO,args[0] + "开始审核数据");
        result = method.invoke(delegate,args);
        logger.log(Level.INFO,args[0] + "审核数据结束");
        return result;
    }
}

```
**测试**

``` java
package xyz.log.java;




public class TestHelloWorld {
    public static void main(String[] args) {
        //实现对日志的重用
        LogProxy logProxy = new LogProxy();
        TimeBookInterface timeBookProxy = (TimeBookInterface) logProxy.bind(new TimeBook());
        timeBookProxy.doAuditing("张三");
    }
}

```







