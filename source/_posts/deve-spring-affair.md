title: Spring事务支持
date: 2014-02-22 14:10:24
tags:
- Spring
- JavaEE
categories: 设计开发
toc: true
---

### 概述
Spring的事务管理不需要与任何特定的事务API耦合。Spring同时支持编程式事务策略和声明式事务策略，大部分时候都采用声明式事务策略。声明式事务管理的优势非常明显：代码中无需关注事务逻辑，让Spring声明式事务管理负责事务逻辑，声明式事务管理无需与具体的事务逻辑耦合，可以方便地在不同事务逻辑之间切换。

声明式事务逻辑的配置方式，通常有以下4中：
1、使用TransactionProxyFactoryBean为目标Bean生成事务代理的配置。此方式是最传统、配置文件最臃肿、最难以阅读的方式。
2、采用Bean继承的事务代理配置方式，比较简洁，但仍然是增量式配置。
3、采用BeanNameAutoProxyCreator，根据Bean Name自动生成事务代理的方式。这是直接利用Spring的AOP框架配置事务代理的方式，需要对Spring的AOP框架有所理解。但这种方式避免了增量式方式，效果非常不错。
4、采用DefaultAdvisorAutoProxyCreator，直接利用Spring的AOP框架配置事务代理的方式，效果非常不错，只是这种配置方式的可读性不如第3种方式。

<!--more-->
### 参考文档
《Java EE基础实用教程》，郑阿奇主编

