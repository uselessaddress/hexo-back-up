title: "Java API概览"
toc: true
date: 2015-07-19 16:52:32
tags:
- java
categories: 设计开发
---

# 前言

今天，在开发Android的时候，发现自己对Android缺乏全局掌控的感觉。为什么这么说呢？比如，要实现的功能是帧动画，我根本不知道Android提供了一个AnimationDrawable类。

是因为Java没学好？不是，但是Java确实还有很大的进步空间。借此机会，捋一捋Java各个包和类的作用！

<!--more-->

# java
java类库是java发布之初就确定了的基础库，是核心包。

## applet

提供创建 applet 所必需的类和 applet 用来与其 applet 上下文通信的类。 

applet 框架包括两种实体：applet 和 applet 上下文。applet 是一种可嵌入的窗体（参见 Panel 类），它带有几个 applet 上下文用来初始化、启动和终止 applet 的额外方法。 

applet 上下文是负责加载和运行 applet 的应用程序。例如，applet 上下文可能是 Web 浏览器或 applet 开发环境。 

## awt
包含用于创建用户界面和绘制图形图像的所有类。在 AWT 术语中，诸如按钮或滚动条之类的用户界面对象称为组件。Component 类是所有 AWT 组件的根。有关所有 AWT 组件的公共属性的详细描述，请参见 Component。 

当用户与组件交互时，一些组件会激发事件。AWTEvent 类及其子类用于表示 AWT 组件能够激发的事件。有关 AWT 事件模型的描述，请参见 AWTEvent。 

容器是一个可以包含组件和其他容器的组件。容器还可以具有布局管理器，用来控制容器中组件的可视化布局。AWT 包带有几个布局管理器类和一个接口，此接口可用于构建自己的布局管理器。有关更多信息，请参见 Container 和 LayoutManager。

### color
### dnd
### datatransfer
### event
### font
### geom
### im
### image
### print
### doc-files

## beans
包含与开发 beans 有关的类，即基于 JavaBeans™ 架构的组件。少数类可由 bean 使用，也能以应用程序的形式运行。例如，event 类由激发属性和禁止更改事件的 bean 使用（参见 PropertyChangeEvent）。不过，此包中的大多数类由 bean 编辑器（即自定义 bean 并将其汇集起来以创建应用程序的开发环境）使用。特别要指出的是，这些类帮助 bean 编辑器创建用户可以用来自定义 bean 的用户界面。例如，bean 可能包含 bean 编辑器也许不知道如何处理的特殊类型的属性。通过使用 PropertyEditor 接口，bean 开发人员可以为此特殊类型提供一个编辑器。 

为了最大限度地减少 bean 使用的资源，只在要编辑 bean 时加载 bean 编辑器使用的类。当 bean 以应用程序的形式运行时，不需要这些类，所以不用加载它们。此信息在称为 bean-info 的类中（参见 BeanInfo）。 

除非显式声明，否则 null 值或空 String 对于此包中的方法是无效参数。如果使用这些参数，可能将引发异常。

### beancontext

## io
通过数据流、序列化和文件系统提供系统输入和输出。 除非另有说明，否则向此包的任何类或接口中的构造方法或方法传递 null 参数时，都将抛出 NullPointerException。 

## lang
提供利用 Java 编程语言进行程序设计的基础类。最重要的类是 Object（它是类层次结构的根）和 Class（它的实例表示正在运行的应用程序中的类）。 

把基本类型的值当成一个对象来表示通常很有必要。包装器类 Boolean、Character、Integer、Long、Float 和 Double 就是用于这个目的。例如，一个 Double 类型的对象包含了一个类型为 double 的字段，这表示如果引用某个值，则可以将该值存储在引用类型的变量中。这些类还提供了大量用于转换基值的方法，并支持一些标准方法，比如 equals 和 hashCode。Void 类是一个非实例化的类，它保持一个对表示基本类型 void 的 Class 对象的引用。 

类 Math 提供了常用的数学函数，比如正弦、余弦和平方根。类似地，类 String 和 StringBuffer 提供了常用的字符串操作。 

类 ClassLoader、Process、Runtime、SecurityManager 和 System 提供了管理类的动态加载、外部进程创建、主机环境查询（比如时间）和安全策略实施等“系统操作”。 

类 Throwable 包含了可能由 throw 语句抛出的对象(§14.16)。Throwable 的子类表示错误和异常。

### annotation
### instrument
### management
### ref
### reflect

## math
提供用于执行任意精度整数算法 (BigInteger) 和任意精度小数算法 (BigDecimal) 的类。BigInteger 除提供任意精度之外，它类似于 Java 的基本整数类型，因此在 BigInteger 上执行的操作不产生溢出，也不会丢失精度。除标准算法操作外，BigInteger 还提供模 (modular) 算法、GCD 计算、基本 (primality) 测试、素数生成、位处理以及一些其他操作。 BigDecimal 提供适用于货币计算和类似计算的任意精度的有符号十进制数字。BigDecimal 允许用户对舍入行为进行完全控制，并允许用户选择所有八个舍入模式。 


## net

为实现网络应用程序提供类。 

java.net 包可以大致分为两个部分：

1、低级 API，用于处理以下抽象：

- 地址，也就是网络标识符，如 IP 地址。

- 套接字，也就是基本双向数据通信机制。

- 接口，用于描述网络接口。 

2、高级 API，用于处理以下抽象：

- URI，表示统一资源标识符。

- URL，表示统一资源定位符。

- 连接，表示到 URL 所指向资源的连接。


## nio
定义作为数据容器的缓冲区，并提供其他 NIO 包的概述。 

NIO API 的集中抽象为： 

- 缓冲区，它们是数据容器； 

- 字符集及其相关解码器和编码器，它们在字节和 Unicode 字符之间进行转换；

- 各种类型的通道，它们表示到能够执行IO操作的实体的连接；以及选择器和选择键，它们与可选择信道 一起定义了多路的、无阻塞的 I/O 设施。 

### channel
### charset

## rmi
提供 RMI 包。RMI 指的是远程方法调用 (Remote Method Invocation)。它是一种机制，能够让在某个 Java 虚拟机上的对象调用另一个 Java 虚拟机中的对象上的方法。可以用此方法调用的任何对象必须实现该远程接口。调用这样一个对象时，其参数为 "marshalled" 并将其从本地虚拟机发送到远程虚拟机（该远程虚拟机的参数为 "unmarshalled"）上。该方法终止时，将编组来自远程机的结果并将结果发送到调用方的虚拟机。如果方法调用导致抛出异常，则该异常将指示给调用方。 

### activation
### dgc
### registry
### server

## security
为安全框架提供类和接口。包括那些实现了可方便配置的、细粒度的访问控制安全架构的类。此包也支持密码公钥对的生成和存储，以及包括信息摘要和签名生成在内的可输出密码操作。最后，此包提供支持 signed/guarded 对象和安全随机数生成的对象。此包中提供的许多类（特别是密码和安全随机数生成器类）是基于提供商的。该类本身定义了应用程序可以写入的编程接口。实现本身可由独立的第三方厂商来写，可以根据需要进行无缝的插入。因此，应用程序开发人员可以利用任何数量的基于提供商的实现而不必添加或重写代码。 

### acl
### cert
### interfaces
### spec

## sql
提供使用Java™编程语言访问并处理存储在数据源（通常是一个关系数据库）中的数据的 API。此 API 包括一个框架，凭借此框架可以动态地安装不同驱动程序来访问不同数据源。尽管 JDBC™ API 主要用于将 SQL 语句传递给数据库，但它还可以用于以表格方式从任何数据源中读写数据。通过接口的 javax.sql.RowSet 组可以使用的 reader/writer 实用程序，可以被定制以使用和更新来自电子表格、纯文本文件或其他任何表格式数据源的数据。

## text
提供以与自然语言无关的方式来处理文本、日期、数字和消息的类和接口。这意味着所编写的主程序或 applet 是与语言无关的，并且它可以依靠独立的、动态链接的本地化资源。这实现了随时为新本地化添加本地化的灵活性。 

这些类能够格式化日期、数字和消息、解析、搜索和排序字符串，以及迭代字符、单词、语句和换行符。此包包含类和接口的三大主要组： 

- 用于迭代文本的类 
- 用于格式化和分析的类 
- 用于整理字符串的类 
### spi

## util
包含 collection 框架、遗留的 collection 类、事件模型、日期和时间设施、国际化和各种实用工具类（字符串标记生成器、随机数生成器和位数组）。 

### concurrent
### jar
### logging
### prefs
### regex
### spi
### zip

# javax
javax的x是extension的意思，也就是扩展包。
javax类库是在java类库上面增加的一层东西，就是为了保持版本兼容要保存原来的，但有些东西有了更好的解决方案，所以，就加上些，典型的就是awt(Abstract Windowing ToolKit) 和swing。


## accessibility
定义了用户界面组件与提供对这些组件进行访问的辅助技术之间的协定。如果 Java 应用程序完全支持 Java Accessibility API，则它应该与屏幕读取器、屏幕放大器这样的辅助技术保持兼容和友好。使用完全支持 Java Accessibility API 的 Java 应用程序，将不再需要离屏模型的屏幕读取器 ，因为该 API 提供了离屏模型中通常所包含的所有信息。 

## activation
### processing

## activity
包含解组期间通过 ORB 机制抛出的与 Activity 服务有关的异常。 

## annotation

## crypto
为加密操作提供类和接口。在此包中定义的加密操作包括加密、密钥生成和密钥协商，以及消息验证码（Message Authentication Code，MAC）生成。 

加密支持包括对称密码、不对称密码、块密码和流密码。此包还支持安全流和密封的对象。 

此包中提供的许多类都是基于提供者的。该类本身定义可以写入应用程序的编程接口。然后可由独立的第三方供应商编写实现本身，并根据需要无缝嵌入。因此，应用程序开发人员可以利用任意数量的基于提供者的实现，而无需添加或重写代码。 

### interfaces
### spec

## imageio
Java Image I/O API 的主要包。 

使用 ImageIO 类的静态方法可以执行许多常见的图像 I/O 操作。 

此包包含一些基本类和接口，有的用来描述图像文件内容（包括元数据和缩略图）(IIOImage)；有的用来控制图像读取过程（ImageReader、ImageReadParam 和 ImageTypeSpecifier）和图像写入过程（ImageWriter 和 ImageWriteParam）；还有的用来执行格式之间的代码转换 (ImageTranscoder) 和报告错误 (IIOException)。 

### envent
### metadata
### plugins
### spi
### stream

## jws
### soap

## lang
### model

## management
提供 Java Management Extensions 的核心类。

Java Management Extensions (JMXTM) API 是一个用于管理和监视的标准 API。典型用途包括：

- 查询并更改应用程序配置 
- 累积有关应用程序行为的统计数据并使其可用 
- 通知状态更改及错误状况。
 
JMX API 还可以作为解决方案的一部分来管理系统、网络等。

API 包括远程访问，因此，远程管理程序可以基于这些目的与正在运行的应用程序进行交互。

### loading
### monitor
### timer
### relation
### openmbean
### modelmbean
### remote

## naming
为访问命名服务提供类和接口。 

此包定义 Java Naming and Directory InterfaceTM (JNDI) 的命名操作。  JNDI 向使用 Java 编程语言编写的应用程序提供命名和目录功能。它被设计成与任何特定的命名或目录服务实现无关。因此可以使用共同的方式对多种服务（新的、新出现的及已经部署的服务）进行访问。

### directory
### event
### ldap
### spi

## net
提供用于网络应用程序的类。这些类包括用于创建套接字的工厂。使用套接字工厂可以封装套接字的创建和配置行为。 
### ssl

## print
为 Java™ Print Service API 提供了主要类和接口。Java Print Service API 允许客户端和服务器应用程序具备如下功能： 

- 根据其性能发现并选择 PrintService。 
- 指定打印数据的格式。 
- 向支持所打印文档类型的服务提交 PrintJob。 

### attribute
### event


## rmi
包含 RMI-IIOP 的用户 API。这些 API 供 RMI-IIOP 应用程序使用，并在 IIOP 或 JRMP 上运行时提等效的语法。另请参阅 javax.rmi.CORBA 包。 

### CORBA
### ssl

## script
脚本 API 由定义 Java™ Scripting Engines 的接口和类组成，并为它们在 Java 应用程序中的使用提供框架。此 API 供那些希望在其 Java 应用程序中执行用脚本语言编写的程序的应用程序编程人员使用。脚本语言程序通常由应用程序的终端用户提供。 



## security
### auth
### cert
### sasl

## sound
### sampled
### midi

## sql
为通过 Java™ 编程语言进行服务器端数据源访问和处理提供 API。此包补充了 java.sql 包，它从 1.4 版本开始包含在 Java 平台、标准版 (Java SETM) 中。它保留了 Java 平台、企业版 (Java EETM) 中的精华部分。 

java.sql 包中提供以下内容： 

- DataSource 接口，用于建立到数据源的连接，是 DriverManager 的替代项。 
- 连接池和语句池 
- 分布式事务 
- Rowset 

应用程序直接使用 DataSource 和 RowSet API，但连接池和分布式事务 API 只能由中间层基础设施在内部使用。

### rowset

## swing
提供一组“轻量级”（全部是 Java 语言）组件，尽量让这些组件在所有平台上的工作方式都相同。有关使用这些组件的程序员指南，请参阅 Creating a GUI with JFC/Swing，该内容在 The Java Tutorial 的结尾处。有关其他参考资料，请参阅相关文档。

### border
### colorchooser
### filechooser
### event
### table
### text
### tree
### undo
### plaf

## tools
为能够从程序（例如，编译器）中调用的工具提供接口。 

要求这些接口和类作为 Java™ Platform, Standard Edition (Java SE) 的一部分，但是不要求提供任何实现它们的工具。 

除非明确允许，否则只要给定 null 参数或给定包含 null 元素的列表或集合，此包中的所有方法都将抛出 NullPointerException。类似地，除非明确允许，否则所有方法都不可以返回 null。 

此包是 Java 编程语言编译器框架的主要部分。此框架允许框架的客户端查找并运行程序中的编译器。该框架还为结构化访问诊断（DiagnosticListener）提供服务提供者接口（SPI），为重写文件访问提供文件抽象（JavaFileManager 和 JavaFileObject）。有关使用 SPI 的详细信息，请参阅 JavaCompiler。 



## transaction
包含解组期间通过 ORB 机制抛出的三个异常。 

### xa

## xml
根据 XML 规范定义核心 XML 常量和功能。
### parsers
### bind
### soap
### ws
### transform
### crypto
### datatype
### validation
### namespace
### xpath
### stream


# org
Java的org包是由企业或者组织提供的java类库。
集成到jdk中但大部分不是sun公司的，可以直接使用。
其中比较常用的是w3c提供的对XML、网页、服务器的类和接口。

## ietf
### igss

## omg
### CORBA
### stub
### CORBA_2_3
### CosNaming
### SendingContext
### PortableServer
### POrtableInterceptor
### Messaging
### IOP
### Dynamic
### DynamicAny

## w3c
### dom

## xml
### sax


# 后记
只展开到了第二级，今后如果有需要，再往下展开。

附上Android API概览：
http://www.android-doc.com/reference/packages.html

# 参考文档
《JDK API 1.6.0 中文版》