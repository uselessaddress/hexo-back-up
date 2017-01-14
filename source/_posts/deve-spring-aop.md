title: "Spring AOP"
date: 2014-02-22 12:10:24
tags:
- Spring
- AOP
- JavaEE
categories: 设计开发
toc: true
---

### 从代理机制初探AOP
```
package com.voidking.aop;

import java.util.logging.*;

public class HelloSpeaker {
	private Logger logger = Logger.getLogger(this.getClass().getName());
	public void hello(String name)
	{
		logger.log(Level.INFO, "hello method starts...");
		System.out.println("hello,"+name);
		logger.log(Level.INFO, "hello method ends...");
	}
}

```
在HelloSpeaker类中，当执行hello()方法时，程序员希望该方法执行开始与执行完毕时都留下日志。最简单的做法是上面的程序设计，在方法执行前后加上日志动作。

然而对于HelloSpeaker类来说，日志的这种动作并不属于HelloSpeaker的逻辑，这使得HelloSpeaker增加了额外的职责。
<!--more-->
如果程序中这种日志动作到处都有，以上的写法势必造成程序员必须到处撰写这些日志动作的代码。这将使得维护日志代码的困难加大。如果需要的服务不只是日志动作，有一些非类本身职责的相关动作也混入到类中，如权限检查、事务管理等，会使得类的负担加重，甚至混淆类本身的职责。

另一方面，使用以上的写法，如果有一天不再需要日志（或权限检查、交易管理等）服务，将需要修改所有留下日志动作的程序，无法简单地将这些相关服务从现有的程序中移除。

可以使用代理（Proxy）机制来解决这个问题，有两种代理方式：静态代理（static proxy）和动态代理（dynamic proxy）。

在静态代理的实现中，代理类与被代理的类必须实现同一个接口。在代理类中可以实现记录等相关服务，并在需要的时候再呼叫被代理类。这样被代理类就可以仅仅保留业务相关的职责了。

举个简单的例子，首先定义一个IHello接口：
```
package com.voidking.aop;

public interface IHello {
	public void hello(String name);
}

```

然后让实现业务逻辑的HelloSpeaker2类实现IHello接口：
```
package com.voidking.aop;


public class HelloSpeaker2 implements IHello {
	
	@Override
	public void hello(String name) {
		// TODO Auto-generated method stub
		System.out.println("hello,"+name);
	}

}

```

代理类HelloProxy同样要实现IHello接口：
```
package com.voidking.aop;

import java.util.logging.Level;
import java.util.logging.Logger;

public class HelloProxy implements IHello {

	private Logger logger = Logger.getLogger(this.getClass().getName());
	private IHello helloObject;
	public  HelloProxy(IHello helloObject) {
		// TODO Auto-generated constructor stub
		this.helloObject = helloObject;
	}
	
	
	@Override
	public void hello(String name) {
		// TODO Auto-generated method stub
		log("hello method starts...");
		helloObject.hello(name);
		log("hello method ends...");
	}
	
	private void log(String msg)
	{
		logger.log(Level.INFO, msg);
	}

}

```
写一个测试程序来看看效果：
```
package com.voidking.aop;

public class ProxyDemo {
	public static void main(String[] args) {
		IHello proxy = new HelloProxy(new HelloSpeaker2());
		proxy.hello("Justin");
	}
}

```

这是静态代理的基本示例，但是可以看到，代理类的一个接口只能服务于一种类型的类，而且如果要代理的方法很多，势必要为每个方法进行代理。静态代理在程序规模稍大时必定无法胜任。

### 动态代理
JDK1.3之后加入了可协助开发动态代理功能的API等相关类别，不需要为特定类和方法编写特定代理类，使用动态代理。使用动态代理可以使一个处理者（Handler）为各个类服务。

IHello.java、HelloSpeaker.java、LogHandler.java、ProxyDemo.java代码分别如下：
```
package com.voidking.aop2;

public interface IHello {
	public void hello(String name);
}

```

```
package com.voidking.aop2;

public class HelloSpeaker implements IHello {

	@Override
	public void hello(String name) {
		// TODO Auto-generated method stub
		System.out.println("Hello,"+name);
	}

}

```

```
package com.voidking.aop2;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class LogHandler implements InvocationHandler {

	private Object sub;
	public LogHandler() {
		// TODO Auto-generated constructor stub
	}
	public LogHandler(Object obj){
		sub=obj;
	}
	@Override
	public Object invoke(Object proxy, Method method, Object[] args)
			throws Throwable {
		// TODO Auto-generated method stub
		System.out.println("before you do thing");
		method.invoke(sub, args);
		System.out.println("after you do thing");
		return null;
	}

}

```

```
package com.voidking.aop2;

import java.lang.reflect.Proxy;

public class ProxyDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		HelloSpeaker helloSpeaker = new HelloSpeaker();
		LogHandler logHandler = new LogHandler(helloSpeaker);
		Class cls = helloSpeaker.getClass();
		IHello iHello = (IHello)Proxy.newProxyInstance(cls.getClassLoader(), cls.getInterfaces(), logHandler);
		iHello.hello("Justin");
	}

}

```
HelloSpeaker本身的职责是显示文字，却必须插入日志动作，这是的HelloSpeaker的职责加重。日志的程序代码横切（cross-cutting）到HelloSpeaker的程序执行流程中，日志这样的动作在AOP术语中被称为横切关注点（cross-cutting concerns）。

使用代理类将记录与业务逻辑无关的动作提取出来，设计为一个服务类，如同前面的范例HelloProxy或者LogHandler，这样的类称为切面（Aspect）。

将日志等动作（cross-cutting concerns）设计为通用，不介入特定业务类的一个职责清楚的Aspect类，这就是所谓的Aspect-Oriented Programming，AOP。


### 通知Advice
Spring提供了5种通知（Advice）类型：
- Interception Around Advice：在目标对象的方法执行前后被调用。
- Before Advice：在目标对象的方法执行前被调用。
- After Returning Advice：在目标对象的方法执行后被调用。
- Throw Advice：在目标对象的方法抛出异常时被调用。
- Introduction Advice：一种特殊类型的拦截通知，只有在目标对象的方法调用完毕后执行。

### 切入点Pointcut
Pointcut定义了Advice应用的时机。
在Spring中，使用PointcutAdvisor把Pointcut与Advice结合为一个对象。Spring中大部分内建的Pointcut都有对应的PointAdvisor。
静态切入点只限于给定的方法和目标类，而不考虑方法的参数。动态切入点与静态切入点的区别是，动态切入点不仅限定于给定的方法和类，还可以指定方法的参数。大多数切入点，可以使用静态切入点，很少有机会创建动态切入点。

### 源代码分享
https://github.com/voidking/aop.git

### 小结
Advice和Pointcut到底干嘛用的？书上根本没有讲清楚哇！不去查资料了，需要的时候再去深入学习。


### 参考文档
《Java EE基础实用教程》，郑阿奇主编

