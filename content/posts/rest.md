---
title: 深入浅出REST
tag: rest
date: 2018-05-01 09:35:10
---

不知你是否意识到，围绕着什么才是实现异构的应用到应用通信的“正确”方式，一场争论正进行的如火如荼：虽然当前主流的方式明显地集中在基于SOAP、WSDL和WS-*规范的Web Services领域，但也有少数人用细小但洪亮的声音主张说更好的方式是REST，表述性状态转移（REpresentational State Transfer）的简称。在本文中，我不会涉及争论的话题，而是尝试对REST和RESTful HTTP应用集成做实用性的介绍。以我的经验，有些话题一旦触及就会引来众多的讨论，当涉及到这方面话题的时候，我会深入详细地阐述。

## REST关键原则

大部分对REST的介绍是以其正式的定义和背景作为开场的。但这儿且先按下不表，我先提出一个简单扼要的定义：REST定义了应该如何正确地使用（这和大多数人的实际使用方式有很大不同）Web标准，例如HTTP和URI。如果你在设计应用程序时能坚持REST原则，那就预示着你将会得到一个使用了优质Web架构（这将让你受益）的系统。总之，五条关键原则列举如下：

* 为所有“事物”定义ID
* 将所有事物链接在一起
* 使用标准方法
* 资源多重表述
* 无状态通信

下面让我们进一步审视这些原则。

## 为所有“事物”定义ID

在这里我使用了“事物”来代替更正式准确的术语“资源”，因为一条如此简单的原则，不应该被淹没在术语当中。思考一下人们构建的系统，通常会找到一系列值得被标识的关键抽象。每个事物都应该是可标识的，都应该拥有一个明显的ID——在Web中，代表ID的统一概念是：URI。URI构成了一个全局命名空间，使用URI标识你的关键资源意味着它们获得了一个唯一、全局的ID。

对事物使用一致的命名规则（naming scheme）最主要的好处就是你不需要提出自己的规则——而是依靠某个已被定义，在全球范围中几乎完美运行，并且能被绝大多数人所理解的规则。想一下你构建的上一个应用（假设它不是采用RESTful方式构建的）中的任意一个高级对象（high-level object），那就很有可能看到许多从使用唯一标识中受益的用例。比如，如果你的应用中包含一个对顾客的抽象，那么我可以相当肯定，用户会希望将一个指向某个顾客的链接，能通过电子邮件发送到同事那里，或者加入到浏览器的书签中，甚至写到纸上。更透彻地讲：如果在一个类似于Amazon.com的在线商城中，没有用唯一的ID（一个URI）标识它的每一件商品，可想而知这将是多么可怕的业务决策。

当面对这个原则时，许多人惊讶于这是否意味着需要直接向外界暴露数据库记录（或者数据库记录ID）——自从多年以来面向对象的实践告诫我们，要将持久化的信息作为实现细节隐藏起来之后，哪怕是刚有点想法都常会使人惊恐。但是这条原则与隐藏实现细节两者之间并没有任何冲突：通常，值得被URI标识的事物——资源——要比数据库记录抽象的多。例如，一个定单资源可以由定单项、地址以及许多其它方面（可能不希望作为单独标识的资源暴露出来）组成。标识所有值得标识的事物，领会这个观念可以进一步引导你创造出在传统的应用程序设计中不常见的资源：一个流程或者流程步骤、一次销售、一次谈判、一份报价请求——这都是应该被标识的事物的示例。同样，这也会导致创建比非RESTful设计更多的持久化实体。

下面是一些你可能想到的URI的例子：

```
http://example.com/customers/1234 
http://example.com/orders/2007/10/776654 http://example.com/products/4554 
http://example.com/processes/salary-increase-234
```

正如我选择了创建便于阅读的URI——这是个有用的观点，尽管不是RESTful设计所必须的——应该能十分容易地推测出URI的含义：它们明显地标识着单一“数据项”。但是再往下看：

```
http://example.com/orders/2007/11 
http://example.com/products?color=green
```

首先，这两个URI看起来与之前的稍有不同——毕竟，它们不是对一件事物的标识，而是对一类事物集合的标识（假定第一个URI标识了所有在2007年11月份提交的定单，第二个则是绿颜色产品的集合）。但是这些集合自身也是事物（资源），也应该被标识。

注意，使用唯一、全局统一的命名规则的好处，既适用于浏览器中的Web应用，也适用于机对机（machine-to-machine，m2m）通信。

来对第一个原则做下总结：使用URI标识所有值得标识的事物，特别是应用中提供的所有“高级”资源，无论这些资源代表单一数据项、数据项集合、虚拟亦或实际的对象还是计算结果等。

## 将所有事物链接在一起

接下来要讨论的原则有一个有点令人害怕的正式描述：“超媒体被当作应用状态引擎（Hypermedia as the engine of application state）”，有时简写为HATEOAS。（严格地说，这不是我说的。）这个描述的核心是 **超媒体** 概念，换句话说：是 **链接** 的思想。链接是我们在HTML中常见的概念，但是它的用处绝不局限于此（用于人们网络浏览）。考虑一下下面这个虚构的XML片段：

```
<order self="http://example.com/customers/1234"> 
<amount>23</amount> 
<product ref="http://example.com/products/4554"> 
<customer ref="http://example.com/customers/1234"> 
</customer> </product></order>
```

如果你观察文档中product和customer的链接，就可以很容易地想象到，应用程序（已经检索过文档）如何“跟随”链接检索更多的信息。当然，如果使用一个遵守专用命名规范的简单“id”属性作为链接，也是可行的—— **但是仅限于应用环境之内** 。使用URI表示链接的优雅之处在于，链接可以指向由不同应用、不同服务器甚至位于另一个大陆上的不同公司提供的资源——因为URI命名规范是全球标准，构成Web的所有资源都可以互联互通。

超媒体原则还有一个更重要的方面——应用“状态”。简而言之，实际上服务器端（如果你愿意，也可以叫服务提供者）为客户端（服务消费者）提供一组链接，使客户端能通过链接将应用从一个状态改变为另一个状态。稍后我们会在另一篇文章中探究这个方面的影响；目前，只需要记住：链接是构成动态应用的非常有效的方式。

对此原则总结如下：任何可能的情况下，使用链接指引可以被标识的事物（资源）。也正是超链接造就了现在的Web。

## 使用标准方法

在前两个原则的讨论中暗含着一个假设：接收URI的应用程序可以通过URI明确地 **做** 一些有意义的事情。如果你在公共汽车上看到一个URI，你可以将它输入浏览器的地址栏中并回车——但是你的浏览器如何知道需要对这个URI做些什么呢？

它知道如何去处理URI的原因在于所有的资源都支持同样的接口，一套同样的方法（只要你乐意，也可以称为操作）集合。在HTTP中这被叫做动词（verb），除了两个大家熟知的（GET和POST）之外，标准方法集合中还包含PUT、DELETE、HEAD和OPTIONS。这些方法的含义连同行为许诺都一起定义在HTTP规范之中。如果你是一名OO开发人员，就可以想象到RESTful HTTP方案中的所有资源都继承自类似于这样的一个类（采用类Java、C # 的伪语法描述，请注意关键的方法）：

```
class Resource { 
Resource(URI u); 
Response get(); 
Response post(Request r); 
Response put(Request r); 
Response delete(); 
}
```

由于所有资源使用了同样的接口，你可以依此使用GET方法检索一个 **表述** （representation）——也就是对资源的描述。因为规范中定义了GET的语义，所以可以肯定当你调用它的时候不需要对后果负责——这就是为什么可以“安全”地调用此方法。GET方法支持非常高效、成熟的缓存，所以在很多情况下，你甚至不需要向服务器发送请求。还可以肯定的是，GET方法具有 **幂等性** [译注：指多个相同请求返回相同的结果]——如果你发送了一个GET请求没有得到结果，你可能不知道原因是请求未能到达目的地，还是响应在反馈的途中丢失了。幂等性保证了你可以简单地再发送一次请求解决问题。幂等性同样适用于PUT（基本的含义是“更新资源数据，如果资源不存在的话，则根据此URI创建一个新的资源”）和DELETE（你完全可以一遍又一遍地操作它，直到得出结论——删除不存在的东西没有任何问题）方法。POST方法，通常表示“创建一个新资源”，也能被用于调用任意过程，因而它既不安全也不具有幂等性。

如果你采用RESTful的方式暴露应用功能（如果你乐意，也可以称为服务功能）， **那这条原则和它的约束同样也适用于你** 。如果你已经习惯于另外的设计方式，则很难去接受这条原则——毕竟，你很可能认为你的应用包含了超出这些操作表达范围的逻辑。请允许我花费一些时间来让你相信不存在这样的情况。

来看下面这个简单的采购方案例子：

[image:79A10003-EA20-46E5-BCAE-B2EB238B3C25-98444-0000E8FFDC1EC4AC/figure1.jpg]
可以看到，例子中定义了两个服务程序（没有包含任何实现细节）。这些服务程序的接口都是为了完成任务（正是我们讨论的OrderManagement和CustomerManagement服务）而定制的。如果客户端程序试图使用这些服务，那它必须针对这些特定接口进行编码——不可能在这些接口定义之前，使用客户程序去有目的地和接口协作。这些接口定义了服务程序的应用协议（application protocol）。

在RESTful HTTP方式中，你将通过组成 **HTTP应用协议** 的通用接口访问服务程序。你可能会想出像这样的方式：

[image:01E1B47D-5FEF-420D-82D1-5D952990DAF9-98444-0000E901C3490CA3/figure2.jpg]
可以看到，服务程序中的特定操作被映射成为标准的HTTP方法——为了消除歧义，我创建了一组全新的资源。“这是骗人的把戏”，我听见你叫嚷着。不，这不是欺骗。标识一个顾客的URI上的GET方法正好相当于getCustomerDetails操作。有人用三角形形象化地说明了这一点：

[image:2CDF0860-3C8A-4E20-A110-9E4D8F7EA0DD-98444-0000E902291203B7/figure3.jpg]
把三个顶点想象为你可以调节的按钮。可以看到在第一种方法中，你拥有许多操作，许多种类的数据以及固定数量的“实例”（本质上和你拥有的服务程序数量一致）。在第二种方法中，你拥有固定数量的操作，许多种类的数据和许多调用固定方法的对象。它的意义在于，证明了通过这两种方式，你基本上可以表示任何你喜欢的事情。

为什么使用标准方法如此重要？从根本上说，它使你的应用成为Web的一部分——应用程序为Web变成Internet上最成功的应用所做的贡献，与它添加到Web中的资源数量成比例。采用RESTful方式，一个应用可能会向Web中添加数以百万计的客户URI；如果采用CORBA技术并维持应用的原有设计方式，那它的贡献大抵只是一个“端点（endpoint）”——就好比一个非常小的门，仅仅允许有钥匙的人进入其中的资源域。

统一接口也使得所有理解HTTP应用协议的组件能与你的应用交互。通用客户程序（generic client）就是从中受益的组件的例子，例如curl、wget、代理、缓存、HTTP服务器、网关还有Google、Yahoo!、MSN等等。

总结如下：为使客户端程序能与你的资源相互协作，资源应该正确地实现默认的应用协议（HTTP），也就是使用标准的GET、PUT、POST和DELETE方法。

## 资源多重表述

到目前为止我们一直忽略了一个稍微复杂的问题：客户程序如何知道该怎样处理检索到的数据，比如作为GET或者POST请求的结果？原因是，HTTP采取的方式是允许数据处理和操作调用之间关系分离的。换句话说，如果客户程序知道如何处理一种特定的数据格式，那就可以与所有提供这种表述格式的资源交互。让我们再用一个例子来阐明这个观点。利用HTTP内容协商（content negotiation），客户程序可以请求一种特定格式的表述：

```
GET /customers/1234 HTTP/1.1 
Host: example.com 
Accept: application/vnd.mycompany.customer+xml
```

请求的结果可能是一些由公司专有的XML格式表述的客户信息。假设客户程序发送另外一个不同的请求，就如下面这样：

```
GET /customers/1234 HTTP/1.1 
Host: example.com 
Accept: text/x-vcard
```

结果则可能是VCard格式的客户地址。（在这里我没有展示响应的内容，在其HTTP Content-type头中应该包含着关于数据类型的元数据。）这说明为什么理想的情况下，资源表述应该采用标准格式——如果客户程序对HTTP应用协议和一组数据格式都有所“了解”，那么它就可以用一种有意义的方式 **与世界上任意一个RESTful HTTP应用交互** 。不幸的是，我们不可能拿到所有东西的标准格式，但是，或许我们可以想到在公司或者一些合作伙伴中使用标准格式来营造一个小环境。当然以上情况不仅适用于从服务器端到客户端的数据，反之既然——倘若从客户端传来的数据符合应用协议，那么服务器端就可以使用特定的格式处理数据，而不去关心客户端的类型。

在实践中，资源多重表述还有着其它重要的好处：如果你为你的资源提供HTML和XML两种表述方式，那这些资源不仅可以被你的应用所用，还可以被任意标准Web浏览器所用——也就是说，你的应用信息可以被所有会使用Web的人获取到。

资源多重表述还有另外一种使用方式：你可以将应用的Web UI纳入到Web API中——毕竟，API的设计通常是由UI可以提供的功能驱动的，而UI也是通过API执行动作的。将这两个任务合二为一带来了令人惊讶的好处，这使得使用者和调用程序都能得到更好的Web接口。

总结：针对不同的需求提供资源多重表述。

## 无状态通信

**无状态通信** 是我要讲到的最后一个原则。首先，需要着重强调的是，虽然REST包含无状态性（statelessness）的观念，但这并不是说暴露功能的应用不能有状态——
事实上，在大部分情况下这会导致整个做法没有任何用处。REST要求状态要么被放入资源状态中，要么保存在客户端上。或者换句话说，服务器端不能保持除了单次请求之外的，任何与其通信的客户端的通信状态。这样做的最直接的理由就是可伸缩性—— 如果服务器需要保持客户端状态，那么大量的客户端交互会严重影响服务器的内存可用空间（footprint）。（注意，要做到无状态通信往往需要需要一些重新设计——不能简单地将一些session状态绑缚在URI上，然后就宣称这个应用是RESTful。）

但除此以外，其它方面可能显得更为重要：无状态约束使服务器的变化对客户端是不可见的，因为在两次连续的请求中，客户端并不依赖于同一台服务器。一个客户端从某台服务器上收到一份包含链接的文档，当它要做一些处理时，这台服务器宕掉了，可能是硬盘坏掉而被拿去修理，可能是软件需要升级重启——如果这个客户端访问了从这台服务器接收的链接，它不会察觉到后台的服务器已经改变了。

## 理论上的REST

我承认：以上我所说的REST不是真正的REST，而且我可能有点过多地热衷于简单化。但因为我想有一个与众不同的开场，所以没有在一开始就介绍其正式的定义和背景。现在就让我们稍微简要地介绍一下这方面的内容。

首先，先前我并没有明确地区分HTTP、RESTful HTTP和REST。要理解这些不同方面之间的关系，我们要先来看看REST的历史。

[Roy T. Fielding](http://www.ics.uci.edu/%7Efielding/) 在他的 [博士学位论文](http://www.ics.uci.edu/%7Efielding/pubs/dissertation/top.htm) （实际上你应该访问这个链接——至少对于一篇学术论文来说，它是相当易读的。此论文已被翻译成 [中文](http://www.redsaga.com/opendoc/REST_cn.pdf) ）中定义了术语REST。Roy曾是许多基本Web协议的主要设计者，其中包括HTTP和URIs，并且他在论文中对这些协议提出了很多想法。（这篇论文被誉为“REST圣经”，这是恰当的——毕竟，是作者发明了这个术语，所以在定义上，他写的任何内容都被认为是权威的。）在论文中，Roy首先定义一种方法论来谈论 **架构风格** ——高级、抽象的模式，来表达架构方法背后的核心理念。每一个架构风格由一系列的 **约束** （constraints）定义形成。架构风格的例子包括“没有风格”（根本没有任何约束）、管道和过滤器（pipe and filter）、客户端/服务器、分布式对象以及——你猜到它了——REST。

如果对你来说这些听起来都太抽象了，那就对了——REST在本质上是一个可以被许多不同技术实现的高层次的风格，而且可以被实例化——通过为它的抽象特性赋上不同的值。比如，REST中包含资源和统一接口的概念——也就是说，所有资源都应该对这些相同的方法作出反应。但是REST并没有说明是哪些方法，或者有多少方法。

REST风格的一个“化身”便是HTTP（以及一套相关的一套标准，比如URI），或者稍微抽象一些：Web架构自身。接着上面的例子，HTTP使用HTTP动词作为REST统一接口的“实例”。由于Fielding是在Web已经（或者至少是大部分）“完善”了之后才定义的REST风格，有人可能会争论两者是不是100%的匹配。但是无论如何，整体上来说Web、HTTP和URI仅仅是REST风格的一个主要实现。不过，由于Roy Fielding即是REST论文的作者，又对Web架构设计有过深远的影响，两者相似也在情理之中。

最后，我在前面一次又一次地使用着术语“RESTful HTTP”，原因很简单：许多使用HTTP的应用因为一些理由并没有遵循REST原则，有人会说使用HTTP而不遵循REST原则就等同于滥用HTTP。当然这听起来有点狂热——事实上违反REST约束的原因通常是，仅仅因为每个约束带来的设计权衡可能不适合于一些特殊情况。但通常，违背REST约束的原因可归咎于对其好处认知的缺乏。来看一个明显的反面案例：使用HTTP GET调用类似于删除对象的操作，这违反了REST的安全约束和一般性常识（客户程序不应为此负责，服务器端开发人员大概不是有意而为之）。但在随后的文章中，我会提及更多这样或那样的对HTTP的滥用。

## 总结

本文试图对REST（Web架构）背后的概念提供快速的介绍。RESTful HTTP暴露功能的方式与RPC、分布式对象以及Web Services是不相同的；要真正理解这些不同是需要一些心态的转变。不管你构建的应用是仅仅想暴露Web UI还是想把API变成Web的一份子，了解下REST的原则还是有好处的。


**查看英文原文** ： [A Brief Introduction to REST](http://www.infoq.com/articles/rest-introduction)


> [呼呼呼山]()(http://code4fun.me)
> 2019-05-01 09:35:10
