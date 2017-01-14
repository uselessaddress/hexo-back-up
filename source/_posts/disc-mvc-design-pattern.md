title: 为什么MVC不是一种设计模式？
date: 2014-10-25 22:45:18
tags: 
- 设计模式
- MVC
categories: 点滴发现
---

GoF (Gang of Four，四人组，《Design Patterns: Elements of Reusable Object-Oriented Software》/《设计模式》一书的作者：Erich Gamma、Richard Helm、Ralph Johnson、John Vlissides)并没有把MVC提及为一种设计模式，而是把它当做“一组用于构建用户界面的类集合”。

在他们看来，它其实是其它三个经典的设计模式的演变：观察者模式(Observer)(Pub/Sub),策略模式(Strategy)和组合模式(Composite)。

根据MVC在框架中的实现不同可能还会用到工厂模式(Factory)和装饰器(Decorator)模式。

models表示应用的数据，而views处理屏幕上展现给用户的内容。为此，MVC在核心通讯上基于推送/订阅模型(惊讶的是在很多关于MVC的文章中并没有提及到)。当一个model变化时它对应用其它模块发出更新通知(“publishes”)，订阅者(subscriber)——通常是一个Controller，然后更新对应的view。观察者——这种自然的观察关系促进了多个view关联到同一个 model。

对于感兴趣的开发人员想更多的了解解耦性的MVC(根据不同的实现)，这种模式的目标之一就是在一个主题和它的观察者之间建立一对多的关系。当这个主题改变的时候，它的观察者也会得到更新。Views和controllers的关系稍微有点不同。Controllers帮助views对不同用户的输 入做不同的响应，是一个非常好的策略模式列子。


摘自文档：http://damoqiongqiu.iteye.com/blog/1949256