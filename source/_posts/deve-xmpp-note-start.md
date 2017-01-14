title: XMPP学习笔记——概述篇
date: 2014-11-24 01:17:29
tags: [jQuery,XMPP]
categories: 设计开发
toc: true
---

### XMPP是什么？
XMPP，可扩展消息和出席（存在）协议（eXtensible Messageing and Presence Protocol）。顾名思义，这是一个关于收发消息的规范。

最初研发IMPP（即时信息和出席协议，Instant Messaging and Presence Protocol）是为了创建一种标准化的协议，但是今天，IMPP已经发展成为基本协议单元，定义所有即时通信协议应该支持的核心功能集。

XMPP和SIMPLE（针对即时信息和出席扩展的会话发起协议，Session Initiation Protocol for Instant Messaging and Presence Leveraging Extensions）两种协议是架构，有助于实现IMPP协议所描述的规范。

PRIM（出席和即时信息协议，Presence and Instant Messaging Protocol）最初是基于即时通信的协议，与XMPP 和SIMPLE 类似，但是已经不再使用。
<!--more-->
### XMPP简史

1996年之后，Mirabilis、AOL、Yahoo、微软等互联网公司陆续推出了个人通信产品。但是，这些产品各自绑定到各自专用的协议和网络。也就是说，ICQ的用户不能和MSN的用户交谈。

很多开发人员希望整合客户端，但是屡屡受挫。开放的、去中心化的IM网络和协议思想在这个时候就诞生了，经过发展，后来便有了XMPP。

### XMPP网络
任何XMPP网络都是由若干角色组成的，这些角色可以分为服务器、客户端、组件和服务器插件。

#### 服务器
XMPP服务器是XMPP网络的交通系统，它的任务就是为XMPP节提供路由。

常见的XMPP服务器有Openfire、Ejabberd、Tigase、M-Link、Jabber-XCP等。

#### 客户端
大多数的XMPP实体是客户端，它们通过客户端-服务器协议连接到XMPP服务器。

#### 组件
并不只是客户端能连接到XMPP服务器，大多数服务器还支持外部服务器组件，这些组件通过添加某种新服务来增强服务器的行为。在外界看来，组件就像一个子服务器。
<!--more-->
#### 插件
许多XMPP服务器支持插件扩展，这些插件通常使用与服务器自身相同的语言编写，并在服务器的进程内运行。他们的作用很大程度上与外部组件重叠，但插件还能够访问内部服务器数据结构并改变核心服务器行为。

### XMPP寻址
XMPP网络上每个实体都有一个或多个地址（JID，jabber identifier）。JID有多种不同形式，但它们通常看上去就像是电子邮件。

JID有两种类型，裸JID和完整JID，而裸JID是完整JID去除资源部分后的地址。例如，某个客户端的完整JID是voidking@voidking.lit/library，那么它的裸JID就是voidking@voidking.lit。

### 节
在XMPP中，各项工作都是通过在一个XMPP流上发送和接收XMPP节来完成的。核心XMPP工具集由三种基本节组成，这三种节分别为`<presence>`、`<message>`、`<iq>`。

XMPP流由两份XML文档组成，通信的每个方向均有一份文档。这份文档有一个根元素`<stream:stream>`，这个根元素的子元素由可路由的节以及与流相关的顶级子元素构成。
下面给出一段简短的XMPP会话：
```
<stream:stream>
	<iq type='get'>
		<query xmlns='jabber:iq:roster'/>
	</iq>
	
	<presence/>
	
	<message to='voidking@voidking.lit' from='haojin@gmail.com/ballroom' type='chat'>
		<body>I cannot talk of books in a ball-room; my head is always full of something else.</body>
	</message>
	
	<presence type='unavailable'/>
</stream:stream>
```
简单解释下：
（1）haojin登陆后，将自己的第一节（一个&lt;iq&gt;元素）发送出去，这个&lt;iq&gt;元素请求haojin的联系人列表。
（2）接下来，他使用&lt;presence&gt;节通知服务器他在线并且可以访问。
（3）当haojin注意到voidking也在线时，他发送了一条&lt;message&gt;节给voidking。
（4）最后，haojin发送了一个&lt;presence&gt;节告诉服务器自己不可访问并关闭&lt;stream:stream&gt;元素，然后结束会话。

#### 通用属性
所有三种节都支持一组通用的属性：
（1）from
（2）to
（3）type
（4）id

#### presence节
&lt;presence&gt;节控制并报告实体的可访问性，“在线”、“离线”、“离开”、“请勿打扰”。此外，这个节还用来建立和终止向其他实体发布出席订阅。

##### 普通presence节
普通&lt;presence&gt;节不含type属性，或者type属性值为unavailable或error。type属性没有available值，因为可以通过缺少type属性来指出这种情况。

用户通过发送不带to属性，直接发往服务器的&lt;presence&gt;节来操纵自己的出席状态。

```
<presence/>

<presence type='unavailable'/>

<presence>
	<show>away</show>
	<status>at the ball</status>
<presence/>

<presence>
	<status>touring the countryside</status>
	<priority>10</priority>
</presence>

<presence>
	<priority>10</priority>
</presence>
```
简单解释：
（1）&lt;show&gt;元素用来传达用户的可访问性，只能出现在&lt;presence&gt;节中。该元素的值可以为：away、chat、dnd和xa，分别表示离开、有意聊天、不希望被打扰和长期离开。
（2）&lt;status&gt;元素时一个人类可读的字符串，在接收者的聊天客户端中，这个字符串一般会紧挨着联系人名字显示。
（3）&lt;priority&gt;元素用来指明连接资源的优先级，介于-128~127，默认值为0。

##### 扩展presence节
开发人员希望能够扩展&lt;presence&gt;节以包含更详细的信息，比如用户当前听的歌或个人的情绪。
因为&lt;presence&gt;节会广播给所有联系人，并且在XMPP网络流量中占据了很大的份额，所以不鼓励这种做法。这类扩展应该交给额外信息传送协议来处理。

##### 出席订阅
用户的服务器会自动地将出席信息广播给那些订阅该用户出席信息的联系人。类似的，用户从所有他已经出席订阅的联系人那里接收到出席更新信息。
与一些社交网络和IM系统不同，在XMPP中，出席订阅是有方向的。我订阅了你的出席信息，但是你并不一定订阅了我的出席信息。
可以通过设置type的值来识别出席订阅节：subscribe、unsubscribe、subscribed、unsubscribed。

##### 定向出席
定向出席是一种直接发给另一个用户或其他实体的普通&lt;presence&gt;节，这种节用来向那些没有进行出席订阅（通常因为只是临时需要出席信息）的实体传达出席状态信息。

#### message节
&lt;message&gt;节用来从一个实体向另一个实体发送消息。这些可以是简单的聊天信息，也可以是任何结构化信息。例如，绘制指令、游戏状态和新游戏变动状况。

&lt;message&gt;属于发送后不管的类型，没有内在的可靠性，就像电子邮件一样。在有些情况下（比如向不存在的服务器发送信息），发送者可能会收到一个错误提示节，从中了解出现的问题。可以通过在应用程序协议中增加确认机制来实现可靠传送。
##### 消息类型
&lt;message&gt;节有几种不同的类型，这些类型由type属性指出，可取的值有：chat、error、normal、groupchat和headline。该属性是可选的，如果没有指定type值，默认为normal。

##### 消息内容
尽管&lt;message&gt;节可以包含任意扩展元素，但&lt;body&gt;和&lt;thread&gt;元素是为向消息中添加内容提供的正常机制。这两种子元素均是可选的。

#### IQ节
&lt;iq&gt;节表示的是Info/Query，它为XMPP通信提供请求和响应机制。它与HTTP协议的基本工作原理非常相似，允许获取和设置查询，与HTTP的GET和POST动作类似。
&lt;iq&gt;节有四种，通过type属性区分，其中两种请求get和set，两种响应result和error。每一个IQ-get或IQ-set节均必须接收响应的IQ-result和IQ-error节。此外，每一对&lt;iq&gt;必须匹配id属性。

#### error节
所有三种XMPP节都有一个error类型，而且错误提示节的每种类型的内容都是按照同一模式排列。
错误提示节具有定义明确的的结构，通常包含原节（肇事节）的内容、通用错误信息以及应用程序特有的错误条件和信息（可选）。

### 连接生命周期
通过三种基本节，实际上可以完成XMPP中任何任务，但发送XMPP节通常需要建立一个经过身份验证的XMPP会话。

#### 连接
在发送任何节之前，需要建立XMPP流。在XMPP流存在之前，必须建立通往XMPP服务器的连接。
#### 流的建立

一旦建立通往XMPP服务器的连接，XMPP流就启动了。通过向服务器发送起始元素&lt;stream:stream&gt;，就可打开XMPP流。服务器通过发送响应流起始标记&lt;stream:stream&gt;进行相应。
一旦双向建立XMPP流，就可以来回发送各种元素。

#### 身份验证
XMPP允许进行TLS（Transport Layer Security，传输层安全）加密，而且大多数客户端默认使用该功能。一旦服务器通告TLS支持后，客户端就启动TLS连接并将当前套接字升级为一个加密套接字而不断开连接。一旦TLS加密确立，就会创建一对新的XMPP流。

#### 连接断开
当用户结束XMPP会话时，会终止会话并断开连接。一般终止会话方式是，首先发送无效出席信息，然后关闭&lt;stream:stream&gt;元素。

### XMPP通信流程
(1)节点连接到服务器；
(2)服务器利用本地目录系统中的证书对其认证；
(3)节点指定目标地址，让服务器告知目标状态；
(4)服务器查找、连接并进行相互认证；
(5)节点之间进行交互．

### XMPP优势
与HTTP相比，XMPP具有如下的优势：
（1）能够“推送”数据，而不是“拉”。
（2）防火墙友好
（3）牢固的身份验证和安全机制
（4）为许多不同的问题提供大量即开即用的工具

### XMPP不足
每种协议都有各自的优缺点，在许多场合中XMPP并不是完成任务的最佳工具或受到某种限制。
（1）有状态协议
（2）社区和部署不及HTTP广泛
（3）对于简单的请求，其开销比HTTP大
（4）仍然需要专门的实现

### 桥接XMPP和Web
虽然有几款浏览器正在试验一些功能以利用XMPP，但主流浏览器目前还没有内置XMPP协议支持。但通过某种巧妙的编程和一些服务器端的帮助，我们可以在HTTP连接之上建立高效的XMPP会话通道。

是这种高效通道成为可能的是一项名为HTTP长轮询的技术。通过联合使用一个简单的基于HTTP的管理协议以及XMPP连接管理器，我们可以将XMPP（以及它的所有功能）带入到HTTP应用程序中。

#### 长轮询
如果读者曾在一个专横的老板下面工作，他会不停地问：“软件还没有写完吗？”这其实就是轮询。

尽管服务器不会被激怒，但是如果太多客户端过快地轮询，服务器也可能变得缓慢。如果不停地被打扰，那么完成的工作量就会减少。但是为了获取快速更新，轮询的间隔必须相当短，最低的延迟就是轮询间隔的长度。

轮询的另一个问题是大多数轮询请求并没有接收到新数据，就像给专横雇主的答复一样，服务器对“软件还没有写完吗？”问题的答案总是“还没有”。

一些聪明的家伙，发明了一种巧妙的技术来解决这个问题。它们并不立即响应该请求，而是在新数据没有准备好时将其挂起一段时间。

例如，如果服务器上新数据已经准备就绪，那么服务器会立即应答。如果尚无新数据，那么服务器将保持连接的打开状态，并持有所有应答。一旦新数据到达，它最终会响应该请求。如果在一定时间内没有新数据到达，服务器可以发回一个空的响应，这样就不会一次性锁住过多打开的连接。一旦一个请求返回，客户端就会立即发送一个新的请求，整个过程重新开始。

因为每个轮询请求都可能打开较长的时间，所以这种技术被称为长轮询。

轮询和长轮询的唯一真正改变之处是，不让客户端等待重新发送请求，而是让服务器等待直到其有新数据需要通告才响应请求。

该技术的一个缺点是服务器需要更聪明才可以处理这些长轮询请求，而这正是连接管理器的作用所在。

#### 管理连接
XMPP连接可以持续任意长的时间，但HTTP请求却相当短命。连接管理器负责维护第三方的XMPP连接并通过HTTP长轮询技术来提供对连接的访问。

浏览器和连接管理器使用一种名为BOSH的简单协议通过HTTP进行通信。实际上，BOSH帮助HTTP客户端建立一个新的XMPP会话，然后将XMPP节包装到一个特殊的&lt;body&gt;元素中通过HTTP来回传送。它还提供了一些安全功能以确保XMPP会话不会轻易地被劫持。连接管理器和XMPP服务器通信就像它是一个普通的客户端一样。

这样一来，HTTP应用程序就能够控制一个真正的XMPP会话。由于长轮询及时提供的高效率和低延迟，因此它的性能相当好，足矣匹敌原生连接。

#### 让Javascript理解XMPP协议

利用HTTP长轮询，我们就拥有了从服务器获取低延迟数据更新的技术。将该技术与连接管理器组合起来，我们就能够通过一系列HTTP请求来发送和接受XMPP数据。最后，我们还需要简化该技术在Web的原生编程语言Javascript中的实现。

Strophe库的创建宗旨，就是为了让使用Javascript编写XMPP应用程序，能够想采用任何其他语言一样简单，将托管连接的底层细节全部隐藏起来。就Strophe的用户而言，它看上去就跟在任何其他环境中所使用的原生XMPP连接一样。

### 构建XMPP应用程序
#### 浏览器平台
Web浏览器可能是有史以来部署最广泛、使用最多的应用程序平台。XMPP为Web应用程序带来了一套新技术和抽象概念，同时带来的是实时、交互式和协作式应用程序的巨大潜力。

对于XMPP开发人员来说，将Web浏览器定位为目标平台有着巨大的意义。Web应用程序跨平台、易于部署，而且有着巨大的用户基础。此外，Web技术大量使用HTML，而用于HTML的工具通常也都能很好地用于XML，因而也能用于XMPP。

#### 基础设施
就像Web通常需要一个Web服务器和应用程序服务器或框架，XMPP应用程序也需要一些基础设施。

XMPP连接要求有一台XMPP服务器，而且通常在该服务器上会有一个账户。XMPP应用程序还需要与连接管理器进行通信，这是因为浏览器目前还无法原生地理解XMPP协议。最后，应用程序使用的任何服务也必须由XMPP服务器提供。

#### 协议设计
如果不是在创建现有XMPP服务（比如多人聊天或传统IM功能），那么用户可能在进行某种协议涉及来实现自己的梦想。XMPP提供了大量的工具可作为工作基础，而且通常，将这些工具进行简单组合，就足够满足大多数应用程序的需要。
下面有几条指南：
（1）组合现有协议。
（2）保持简洁。
（3）避免扩展出席。
（4）参与社区。

### 参考文档
《XMPP高级编程（作者Jack Moffitt）》
《XMPP The Definitive Guide（作者Peter Saint-Andre, Kevin Smith, and Remko Tron?on）》
前辈们的技术博客……