---
title: 面试内容整理--iOS
tags: jobs
categories: 校招
date: 11 Jan 2018 10:46 PM
---

## iOS方面
### 有时间看一下唐巧的技术博客面试题目
### 线程创建的方式
使用NSThread
[http://www.cnblogs.com/tonge/p/5038721.html]()
### [http://www.jianshu.com/p/b4e1e57fa137]()
### 线程安全
当一段代码被多个线程执行，执行后的结果和多个线程依次执行后的结果一致，那么这段代码就是线程安全的。
答案很明显：为了执行效率，大多数的时候不需要并行的执行一段代码，而加上锁，递归锁之类的东西，执行效率会降低很多。需要线程安全的时候，开发者自己维护就可以了。
### 线程同步锁
### 多线程创建方法
多线程   是一个应用程序内多个代码的执行路径，执行线程，同时在同一时间里执行不同的任务。
三种：
1、NSTread
相对最简单，需要自己管理线程的生命周期和线程同步（加锁会有一定的系统开销）
（1）直接创建线程并且自动运行线程
（2）先创建一个线程对象，然后手动运行线程，在运行线程操作之前可以设置线程的优先级等线程信息。
线程同步方法通过锁来实现，每个线程都只用一个锁，这个锁与一个特定的线程关联。用NSLock 或者NSCondition 锁定资源

2、Cocoa NSOperation (NSOperation,NSOperationQueue)
NSOperation 两种方式
一  直接用定义好的子类 NSInvocationOperation   NSBlockOperation
后台建立一个线程
Queue队列中可以加入很多个operation 可以把它看做是一个线程池子，可以设置线程池中的线程数，即并发操作数。
默认是：-1，-1表示没有限制，同时运行队列中的全部操作。
二  继承NSOperation   
把子类的对象放到NSOperationQueue队列中 ，一旦加入，就处理这个，直到操作完成，队列被释放。
3、GrandCentralDispatch:GCD
底层也是用线程实现，queue可以接受任务，先到先执行顺序来执行，也可以并发或串行执行。
同步或异步
优点多多：基于C，融入面向对象，Block,易用，性能上自动根据系统负载来增减线程数量，无需加锁。

### 内存泄露与内存溢出比较
内存溢出 out of memory，是指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory；比如申请了一个integer,但给它存了long才能存下的数，那就是内存溢出。
内存泄露 memory leak，是指程序在申请内存后，无法释放已申请的内存空间，一次内存泄露危害可以忽略，但内存泄露堆积后果很严重，无论多少内存,迟早会被占光。
memory leak会最终会导致out of memory！
### 检测内存泄露的方法
[http://blog.csdn.net/lifengzhong/article/details/7739507]()
### 了解设计模式吗？为什么要有设计模式？考了一些具体的设计模式。你在实际项目中有用到哪些设计模式？
MVC
单例
工厂模式
### 响应者链是什么和原理；；移动应用安全。iOS性能调优。
### Core Data、数据存储的内存，问了下闭包block和block在内存中位置，同函数指针的区别。
### 如何理解MVC设计模式？
MVC是一种架构设计，M表示数据模型，V表示视图，C表示控制器。
数据模型：负责存储、定义、操作数据。
视图：用来展示数据给用户，和用户进行操作交互。
控制器：它是M与V的协调者，控制器获取数据，将数据交给视图去展示。
### MVC的优劣，如果设计松耦合的模块，组件化模块化
MVC是一个架构，或者说是一个设计模式，它就是强制性使应用程序的输入，处理和输出分开。将一个应用程序分为三个部分：Model，View，Controller。
具体的话就是：
视图
　　视图就是负责跟用户交互的界面。一般就是由HTML，css元素组成的界面，当然现在还有一些像js，ajax，flex一些也都属于视图层。在视图层里没有真正的处理发生，之负责数据输出，并允许用户操纵的方式。MVC一个大的好处是它能为你的应用程序处理很多不同的视图。
模型
Model代表一些应用数据和业务逻辑（一般通过通过javaBean，EJB之间实现）在MVC的三个部件中，模型拥有最多的处理任务。例如它可能用象EJBs和javabean这样的构件对象来处理数据库。被模型返回的数据是中立的，就是说模型与数据格式无关，这样一个模型能为多个视图提供数据。由于应用于模型的代码只需写一次就可以被多个视图重用，所以减少了代码的重复性。
控制器
　　控制器接受用户的输入并调用模型和视图去完成用户的需求。所以当单击Web页面中的超链接和发送HTML表单时，控制器本身不输出任何东西和做任何处理。它只是接收请求并决定调用哪个模型构件去处理请求，然后用确定用哪个视图来显示模型处理返回的数据。

这样强制性的分开之后会有很多的优点：
第一个：分工明确：使用MVC可以把数据库开发，程序业务逻辑开发，页面开发分开，每一层都具有相同的特征，方便以后的代码维护。它使程序员（Java开发人员）集中精力于业务逻辑，界面程序员（HTML和JSP开发人员）集中精力于表现形式上。

第二个：就是松耦合，视图层和业务层分离，这样就允许更改视图层代码而不用重新编译模型和控制器代码，同样，一个应用的业务流程或者业务规则的改变只需要改动MVC的模型层即可。因为模型与控制器和视图相分离，所以很容易改变应用程序的数据层和业务规则。
这一点我感觉比较重要，像实际应用的时候，我们还会把model模块细分为：数据库抽象层，数据操作层，业务逻辑层。这些也都是出于一个送耦合的考虑，改动其中一部分，不会影响到另一部分。

第三个：重用性高。像多个视图能够共享一个模型，不论你视图层是用flash界面或是wap界面，用一个模型就能处理他们。将数据和业务规则从表示层分开，就可以最大化从用代码。

用简单的jsp servlet 和javabean来举个注册的例子来说的话：首先是视图层注册的jsp文件，用户填写完用户信息后，提交一个请求，这个请求同过配置文件找到相应的servlet，servlet就相当于控制器，根据不同的用户请求类型，进而调用不用的业务逻辑层进行处理。处理完成之后又将结构信息反馈给jsp文件也就是视图层文件。最最简单的mvc大体上就是这个流程了。

不过MVC也有一定的缺点：
　MVC的缺点是由于它没有明确的定义，所以完全理解MVC并不是很容易。使用MVC需要精心的计划，由于它的内部原理比较复杂，所以需要花费一些时间去思考。
   你将不得不花费相当可观的时间去考虑如何将MVC运用到你的应用程序，同时由于模型和视图要严格的分离，这样也给调试应用程序带来了一定的困难。每个构件在使用之前都需要经过彻底的测试。一旦你的构件经过了测试，你就可以毫无顾忌的重用它们了。
　　根据开发者经验，由于开发者将一个应用程序分成了三个部件，所以使用MVC同时也意味着你将要管理比以前更多的文件，这一点是显而易见的。这样好像我们的工作量增加了，但是请记住这比起它所能带给我们的好处是不值一提。
　　MVC并不适合小型甚至中等规模的应用程序，花费大量时间将MVC应用到规模并不是很大的应用程序通常会得不偿失。
   总之，不要为了MVC而MVC就行了。
### iOS runtime

### 对于block的看法，

### 简述类目优缺点，如果覆盖本类或者父类的方法会出现什么问题？
优点：不需要通过增加子类而增加现有类的行为（方法），且类目中的方法与原始类方法基本没有区别。
缺点：无法向类目添加实例变量。覆盖原始类方法后，原始类的方法没办法调用。
### 简述内存管理基本原则？
如果使用alloc、copy（mutableCopy）或者retain一个对象时，你就有义务向它发送一条release或autorelease消息。其他方法创建的对象，不需要由你来管理内存。
### 什么是ARC？
ARC是automatic reference counting自动引用计数，在程序编译时自动加入retain/release。在对象被创建时retain count+1，在对象被release时count-1，当count=0时，销毁对象。程序中加入autoreleasepool对象会由系统自动加上autorelease方法，如果该对象引用计数为0，则销毁。那么ARC是为了解决MRC手动管理内存存在的一些而诞生的。
MRC下内存管理的缺点：
1）释放一个堆内存时，首先要确定指向这个堆空间的指针都被release了。(避免提前释放)
2）释放指针指向的堆空间，首先要确定哪些指向同一个堆，这些指针只能释放一次。(避免释放多次，造成内存泄露)
3）模块化操作时，对象可能被多个模块创建和使用，不能确定最后由谁释放。
4）多线程操作时，不确定哪个线程最后使用完毕。
虽然ARC给我们编程带来的很多好多，但也可能出现内存泄露。如下面两种情况：
1）循环参照：A有个属性参照B，B有个属性参照A，如果都是strong参照的话，两个对象都无法释放。
2）死循环：如果有个ViewController中有无限循环，也会导致即使ViewController对应的view消失了，ViewController也不能释放。
### ARC有什么特点？
ARC是编译器特性，iOS5.0新添加的特性，使用ARC开发者不需要再retain、release、autorelease，因为编译器会在合适的地方自动插入retain、release。
ARC不会因少release而导致内存泄漏，过度使用release导致程序崩溃，
ARC可以产生更简洁的代码和更健壮的应用。
### 什么是ARC技术？与GC是否相同？
ARC是Automatic Reference Counting的简称，我们称之为自动引用计数，是iOS5.0之后推出的内存管理的新特性。本质上还是使用引用计数来管理对象，只是我们在编写代码时，不需要向对象发送release或autorelease方法，也不可以调用dealloc方法，编译器会在合适的位置自动给用户生成release（autorelease）消息。GC是Garbage Collection，内存垃圾回收机制，ARC比GC性能好。

### 深、浅复制的基本概念以及区别？
浅复制：只复制对象本身，不对里面的属性复制。
深复制：不仅复制对象本身，对象持有的属性对象也做复制。

### 用户自定义了一个对象，如何实现拷贝（可变和不可变拷贝）？
必须实现copying和mutableCopying协议，表示返回一个不可变和可变的对象。否则，程序将会出现异常。
### 定义属性时，什么时候用assign、retain、copy和nonatomic？
assign：普通赋值，一般常用于基本数据类型，常见委托设计模式，以此来防止循环引用。
retain：保留计数，获得了对象的所有权。引用计数在原有基础上加1。
copy：用来复制对象，一般字符串使用，Foundation中的不可变对象使用，效果相当于retain，只是引用计数加1。
nonatomic：非原子性访问，不加同步，多线程并发访问会提高性能。
### strong和weak，`_unsafe_unretained`与weak的区别？
strong：强引用，在ARC中告诉编译器帮助我们自动插入retain。
weak：弱引用，是普通赋值，相当于手动管理内存的assign。
`_unsafe_unretained`：与weak功能一致，区别在于当指向的对象销毁后，weak会将变量置为nil，防止调用野指针。
### ARC存在内存泄露吗？
ARC中如果内存管理不当的话，同样会存在内存泄露。例如，ARC中也会循环引用导致内存泄露；Objective-C对象与CoreFoundation类之间桥接时，管理不当也会产生内存泄露。
### 当我们释放对象时，为什么需要调用[super dealloc]()方法？
子类是继承自父类，那么子类中有一些实例变量（对象），是继承自父类的，因此，我们需要调用父类方法，将父类所拥有的实例进行释放。
### 自动释放池是什么，如何工作？
自动释放池是NSAutorelease类的一个实例，当向一个对象发送autorelease消息时，该对象会自动入池，待池销毁时，将会向池中所有对象发送一条release消息，释放对象。
### 为什么delegate（代理）属性都是assign而不是retain的？
防止循环引用，以至对象无法得到正确的释放。
### iOS开发中数据持久性有哪几种？
plist文件写入，对象归档，sqlite3数据库，CoreData。
### 什么是KVC和KVO？它们之间的关系是什么？
KVC：键值编码，是一种间接访问对象实例变量的机制，该机制可以不通过存取方法就可以访问对象的实例变量。
KVO：键值观察，是一种能使得对象获取到其他对象属性变化的通知机制。
实现KVO键值观察模式，被观察的对象必须使用KVC键值编码来修改它的实例变量，这样才能被观察者观察到。因此，KVC是KVO的基础或者说KVO的实现是建立在KVC的基础之上的。
### 简述常见的设计模式？
单例模式、代理设计、观察者（通知）、工厂方法、模板方法。
### 内存管理在dealloc方法中用release方法与self.xx=nil哪个好？
使用self.xx=nil更好，因为先调用了release方法，而且还将变量设置为nil，这样就更安全的释放对象，防止野指针调用。
### Objective-C语言的优缺点？
优点：类目、动态识别、支持C语言、Objective-C与C++可以混编。
缺点：不支持命名空间、不支持运算符重载、不支持多重继承。
### 代理delegate、通知Notification与block的使用区别？
delegate和block一般是用于两个对象一对一之间的通信交互，delegate需要定义协议方法，代理对象实现协议方法，并且需要建立代理关系才可以实现通信。block更加简洁，不需要定义繁琐的协议方法，但是如果通信事件比较多的话，建议使用delegate。
Notification主要用于一对多情况下通信，而且，通信对象之间不需要建立关系，但是使用通知，代码可读性差。
### 控制器ViewController的loadView、viewDidLoad、viewWillApear和viewDidUnload分别是在什么时候调用？
loadView：当控制器的根视图view为空，且此view被访问时调用。
viewDidLoad：loadView调用之后被调用。
viewWillApear：当控制器根视图view被添加到父视图上时调用。
viewDidUnload：iOS6.0之前，当内存警告时，先卸载视图，再调用
viewDidUnload来释放内存。
### @synthesize和@dynamic的区别？
@synthesize：系统自动生成getter和setter属性声明。
@dynamic：告诉编译器，属性的获取与赋值方法由用户自己实现，不自动生成。
### 事件响应者链的概念？
响应者链表示一系列的响应者对象。事件被交由第一响应者对象处理，如果第一响应者不处理，事件被沿着响应者链向上传递，交给下一个响应者。一般来说，第一响应者是个视图对象或者其子类对象，当其被触摸后事件交由它处理，如果它不处理，事件会被传递给它的视图控制器对象（如果存在），然后是它的父视图对象（如果存在），以此类推，直到顶层视图。接下来会沿着顶层视图到窗口（UIWindow对象），再到程序（UIApplication对象）。如果整个过程都没有响应这个事件，该事件被丢弃。一般情况下，在响应者链中只要有对象处理事件，事件就停止传递。但有时候可以在视图的响应方法中根据一些条件判断来决定是否需要继续传递事件。
### 什么是动态绑定（多态）？
动态绑定是面向对象特性之一多态的体现，声明一个变量，在运行的时候可以绑定不同的对象，比如在方法的参数中声明这样一个变量`UIView *view`，运行的时候，我们可以根据不同的应用场景给view变量传入不同的对象，可以传递一个UIButton对象，也可以传入UIImageView对象。
### 如何理解delegate？
delegate，又称为委托或代理，它是一种设计模式。delegate主要用于两个对象之间的通信交互，并且解除两个通信对象的耦合性，iOS中大量使用了delegate设计，主要用于视图与使用对象之间的通信交互。
### block与函数有何异同？block有何优点？
1）block类似于C里面的函数指针，都可以作为参数进行传递，用于回调。但是block的实现可以定义在方法中，函数则不可以。
2）block语法简洁，可以在方法中定义实现，这样可以访问方法中的局部变量，使代码更加的紧凑，结构化。
### app的内存和性能优化相关的...(我扯了下内存出问题的原因是可能内存泄露或溢出...，然后举了几个例子；接着解决方法分别从布局优化、系统优化、代码优化，还有使用一些工具监测等方面扯了一大堆)
### Android发送网络请求的过程？

### Android的Dalvik虚拟机与Java虚拟机之间的差异？
### Android内存回收机制？和Java比？
### 是如何解决OOM的？
### ListView的优化？
### 平时如何处理抛出的异常？
### 异常处理？
### 当用户用App的时候发生异常咋办？
### 对异步的理解？　　　　　　　　　//——因为单线程怎么怎么样，才用到多线程，blabla神马！你把异步看作是多线程？对啊，怎么了？。。。
### Cache缓存机制？　　　　　　　//——LruCache缓存啦弱引用啦
### 网络怎么异步加载？　　　　　　//——我对服务端交互不熟悉……（唉，C++和网络编程都是我的硬伤啊）
### 谈谈对线程池的理解？　　　　　//——blabla有没有自定义过线程池？噗…
• 　　我队列有100条下载线程，线程池是510个，应该怎么调度？　　//——blabla回去再看看这方面的知识吧。噗

### 网络编程一定要加强！
### 异步编程一定要加强！
### Android常用的布局
### 进程间如何通讯
### java有没有内存泄露  

### 举一个内存泄露的例子
### 匿名内部类能不能使用外部类变量(这个答错，我理解成局部变量了)
### 数据库升级怎么解决
### AsyncTask
### 用过什么第三方图片加载库
### CocoaTouch框架？
UIKit、Foundation、CoreGraphic、CoreAnimation
1）音频和视频：Core Audio、OpenAL、Media Library、AV Foundation
2）数据管理：Core Data、SQLite
3）图形和动画：Core Animation、OpenGL ES、Quartz 2D、Core Graphic
4）用户应用：Address Book、Core Location、Map Kit、Store Kit

### Objective-C中有无多重继承？如何实现多重继承？
Objective-C是没有多重继承的。
Cocoa中所有的类都是NSObject的子类，多继承在这里是用protocol委托代理来实现的。
面向对象语言大部分都是单继承，例如：Java、C#。
C\++支持多继承。

### 什么是类目与延展？
类目：为已知的类增加新的方法。
延展：通知在本类的定义里使用类目来声明私有方法。

### 在一个对象的方法里self.name=”object”和name=”object”有什么不同？
前者调用的是设置器方法，后者是普通赋值。
### 单例设计模式的实现
	static File 
	*instance = nil;
	@implementation File
	//获取单例的方法
	+(id)shareInstance 
	{
	@synthesize (self) 
	{
	if(instance == nil) 
	{
	instance 
	= [[File alloc] init];
	}
	}
	return instance;
	}
//覆写allocWithZone、copyWithZone、retain、release和autorelease方法，目的是限制这个类只创建一个对象
### 为什么使用单例设计？
1）单例设计是用来限制一个类只能创建一个对象，那么此对象中的属性可以存储全局共享的数据，所有的类都可以访问、设置此单例对象中的属性数据。
2）如果一个类创建的时候非常的耗费性能，那么此类如果能满足要求，可以设置为单例节约性能。
### 层CALayer和UIView的区别是什么？
两者最大的区别是，图层不会直接渲染到屏幕上。UIView是iOS系统中界面元素的基础，所有的界面元素都是继承自它，它本身是由CoreAnimation来实现的，它真正绘图的部分是由一个CALayer类来管理的。UIView本身更像是一个CALayer的管理器。一个UIView上可以有多个CALayer，每个CALayer显示一种东西，增强UIView的展现能力。
### 什么是GCD？
GCD是Apple开发的一个多核编程的较新的解决方法。在Mac OS X 10.6雪豹中首次推出，并引入到iOS4.0。GCD是一个替代诸如NSThread等技术的很高效和强大的技术。GCD完全可以处理诸如数据锁定和资源泄漏等复杂的异步编程问题。

### frame和bounds的区别？
frame指的是：该view在父view坐标系统中的位置大小（参照点是父视图的坐标系统）。
bounds指的是：该view在本身坐标系统中的位置和大小（参照点是本身的坐标系统）。

### 控制器的xib是怎么加载的？
当UIViewController的loadView被调用时，在此方法中，通过NSBundle加载xib，先通过控制器的类名作为xib的文件名加载此xib文件。如果找到此xib文件，则加载为view对象作为控制器的根视图，如果没有xib文件，则通过alloc创建一个view对象作为根视图。
### 控制器如何处理系统内存不足警告？
内存不足时，系统会调用控制器didReceiveMemoryWaring方法通知控制器内存不足。iOS6.0与6.0之前的处理方式不一样。
1）iOS6.0之前：调用didReceiveMemoryWaring后，将self.view设置为nil，并且再调用viewDidUnload方法，在此方法中我们应该释放子视图。
2）iOS6.0之后：调用didReceiveMemoryWaring后，不再调用viewDidUnload方法，则应该在didReceiveMemoryWaring方法中手动将self.view=nil，并且释放子视图。
## iOS中对象间有哪些通信方式？
代理delegate、block、通知和KVO。
### block在内存管理上的特点？需要注意循环引用，如何解决循环引用？
1）block中使用了局部对象，则会将此对象retain，引用了当前对象的属性或者方法，则会将当前对象retain。
2）解决循环引用：将当前对象赋值给一个局部变量，并且使用`__block`关键字修饰该局部变量，使用该变量访问当前对象的属性和方法。
### Objective-C中有线程池（线程队列）吗？NSOperationQueue是如何管理线程的？
NSOperationQueue是Objective-C的线程池，线程池中可以存放多个线程。
NSOperationQueue可以管理线程的并发数，还可以管理线程间的优先级。
### timer的间隔周期准吗？为什么？怎样实现一个精准的timer？
定时器timer一般都是准确的，但是当主线程有些时候难免会出现堵塞情况，这样就有可能导致定时器timer会延迟从而不准确。我们可以开发一个多线程，在此多线程上运行定时器，这样多线程只运行定时器，不会因堵塞而导致误差。
### tableView是如何复用的？
如果屏幕上能显示10个单元格，则tableView只会创建11个单元格，也就是n+1，当滑到第12个单元格时就会复用第1个单元格对象。tableView中有个单元格池的概念，tableView调用协议方法获取单元格时，先从池子中查找是否有可复用的单元格，如果有则复用，如果没有则创建一个单元格对象。
### 如何优化tableView的滑动速度？
1）复用单元格。
2）使用不透明的视图，单元格中尽量少使用动画。
3）图片加载使用异步加载，并且设置图片加载的并发数。
4）滑动时不加载图片，停止滑动开始加载。
5）文字、图片可直接drawInRect绘制。
6）如非必要，减少reloadData全部cell，只reloadRowsAtIndexPaths。
7）如果cell是动态行高度，计算出高度后缓存。
8）cell高度固定的话直接用cell.rowHeight设置高度。
### 谈谈对Runloop的理解？
Run loops是线程相关的基础框架的一部分。一个run loop就是一个事件处理的循环，用来不停的调度工作以及处理输入事件。使用run loop的目的是让你的线程在有工作的时候忙于工作，而没有工作的时候处于休眠状态。
### 如何调用drawRect方法与layoutSubView方法？这两个方法的作用？
通过setNeedsLayout方法异步调用layoutSubView方法。
通过setNeedsDisplay方法异步调用drawRect方法。
drawRect方法用于绘图，layoutSubView方法用于布局子视图。
### UIView与UIViewController的关系？
每个控制器都有一个根视图，控制器UIViewController用来管理此视图的加载和显示。
### iOS中有哪些手势？
轻击、捏合、平移、轻扫、旋转和长按。

### storyboard有什么特点？
storyboard是iOS5新增的特性，是对xib的升级版本，引入了一个容器用于管理多个xib文件，和它们之间的跳转交互。
优点：不用再为每个控制器创建xib文件；可以使用静态cell，当cell固定且不多时，使用起来比较方便。
缺点：storyboard单个文件，不利于团队协作开发。
### 如何打包静态库？
新建一个Framework&Library的项目，编译的时候会将项目中的代码文件打包成一个.a静态库文件。

### SVN、Git协作开发，怎么防止代码文件冲突？
防止代码冲突：不要多人同时修改同一个文件。例如，A、B都修改同一个文件，先让A修改，然后提交到服务器，然后B更新下来，再进行修改。
服务器上的项目文件xcodeproj，仅让一个人管理提交，其他人只更新。防止此文件产生冲突。

### block一般用哪个关键字修饰，为什么？
block一般使用copy关键之进行修饰，block使用copy是从MRC遗留下来的“传统”，在MRC中，方法内容的block是在栈区的，使用copy可以把它放到堆区。但在ARC中写不写都行：编译器自动对block进行了copy操作。
### 用@property声明的NSString（或NSArray，NSDictionary）经常
使用copy关键字，为什么？如果改用strong关键字，可能造成什么问题？
用@property声明 NSString、NSArray、NSDictionary 经常使用copy关键字，是因为他们有对应的可变类型：NSMutableString、NSMutableArray、NSMutableDictionary，他们之间可能进行赋值操作，为确保对象中的字符串值不会无意间变动，应该在设置新属性值时拷贝一份。
如果我们使用是strong,那么这个属性就有可能指向一个可变对象,如果这个可变对象在外部被修改了,那么会影响该属性。
copy此特质所表达的所属关系与strong类似。然而设置方法并不保留新值，而是将其“拷贝” (copy)。 当属性类型为NSString时，经常用此特质来保护其封装性，因为传递给设置方法的新值有可能指向一个NSMutableString类的实例。这个类是NSString的子类，表示一种可修改其值的字符串，此时若是不拷贝字符串，那么设置完属性之后，字符串的值就可能会在对象不知情的情况下遭人更改。所以，这时就要拷贝一份“不可变” (immutable)的字符串，确保对象中的字符串值不会无意间变动。只要实现属性所用的对象是“可变的” (mutable)，就应该在设置新属性值时拷贝一份。
### runloop、autorelease pool以及线程之间的关系？
每个线程(包含主线程)都有一个Runloop。对于每一个Runloop，系统会隐式创建一个Autorelease pool，这样所有的release pool会构成一个像callstack一样的一个栈式结构，在每一个Runloop结束时，当前栈顶的Autorelease pool会被销毁，这样这个pool里的每个Object会被release。
### @property 的本质是什么？ivar、getter、setter 是如何生成并添加到这个类中的？
“属性”(property)有两大概念：ivar(实例变量)、存取方法(access method=getter)，即@property = ivar + getter + setter。
例如下面的这个类：
	@interface 
	WBTextView :UITextView 
	@property (nonatomic,copy)NSString *placehold; 
	
	@property 
	(nonatomic,copy)UIColor *placeholdColor; 
	
	@end
类完成属性的定以后，编译器会自动编写访问这些属性的方法(自动合成autosynthesis)，上述代码写出来的类等效与下面的代码：

	@interface 
	WBTextView :UITextView 
	(NSString *)placehold; 
	
	-(void)setPlacehold:(NSString *)placehold; 
	
	-(UIColor 
	*)placeholdColor; 
	-(void)setPlaceholdColor:(UIColor *)placeholdColor; 
	
	@end
详细介绍见：http://blog.csdn.net/jasonjwl/article/details/49427377
### 分别写一个setter方法用于完成`@property (nonatomic,retain)NSString *name`和`@property (nonatomic,copy) NSString *name`？
retain属性的setter方法是保留新值并释放旧值，然后更新实例变量，令其指向新值。顺序很重要。假如还未保留新值就先把旧值释放了，而且两个值又指向同一个对象，先执行的release操作就可能导致系统将此对象永久回收。
	-(void)setName:(NSString 
	*)name{
	
	[name retain];
	
	[_name release];
	
	_name = name;
	}
	-(void)setName:(NSString *)name{ 
	
	 [_name 
	release];
	 _name = 
	[name copy];
	}
### 说说assign和weak，`_block`和 `_weak`的区别？
assign适用于基本数据类型，weak是适用于NSObject对象，并且是一个弱引用。
assign其实也可以用来修饰对象，那么为什么不用它呢？因为被assign修饰的对象在释放之后，指针的地址还是存在的，也就是说指针并没有被置为nil。如果在后续内存分配中，刚巧分到了这块地址，程序就会崩溃掉。而weak修饰的对象在释放之后，指针地址会被置为nil。
`_block`是用来修饰一个变量，这个变量就可以在block中被修改。
`_block`:使用`_block`修饰的变量在block代码块中会被retain(ARC下，MRC下不会retain)。
`_weak`:使用`_weak`修饰的变量不会在block代码块中被retain。
### 请说出下面代码是否有问题，如果有问题请修改？

	@autoreleasepool 
	{
	 for (int 
	i=0; i<largeNumber; i++) 
	{
	 Person 
	*per = [[Person alloc] init];
	 [per autorelease];
	 }
	
	}
内存管理的原则：如果对一个对象使用了alloc、copy、retain，那么你必须使用相应的release或者autorelease。咋一看，这道题目有alloc，也有autorelease，两者对应起来，应该没问题。但autorelease虽然会使引用计数减一，但是它并不是立即减一，它的本质功能只是把对象放到离他最近的自动释放池里。当自动释放池销毁了，才会向自动释放池中的每一个对象发送release消息。这道题的问题就在autorelease。因为largeNumber是一个很大的数，autorelease又不能使引用计数立即减一，所以在循环结束前会造成内存溢出的问题。
解决方案如下：

	@autoreleasepool 
	{
	 for (int 
	i=0; i<100000; i++) { 
	
	 @autoreleasepool 
	{
	 Person 
	*per = [[Person alloc] init];
	 [per autorelease];
	 }
	 }
	
	}
在循环内部再加一个自动释放池，这样就能保证每创建一个对象就能及时释放。
### 请问下面代码是否有问题，如有问题请修改？

	@autoreleasepool 
	{
	 NSString 
	*str = [[NSString alloc] init];
	 [str retain];
	 [str retain];
	 str = 
	@"jxl";
	 [str release];
	 [str release];
	 [str release];
	}
这道题跟第8题一样存在内存泄露问题，
1）内存泄露 ；
2）指向常量区的对象不能release。
指针变量str原本指向一块开辟的堆区空间，但是经过重新给str赋值，str的指向发生了变化，由原来指向堆区空间，到指向常量区。常量区的变量根本不需要释放，这就导致了原来开辟的堆区空间没有释放，造成内存泄露。
### 什么情况下使用weak关键字，相比assign有什么不同？什么情况使用weak关键字？
1）在ARC中，在有可能出现循环引用的时候，往往要通过让其中一端使用weak来解决。比如delegate代理。
2）自身已经对它进行一次强引用，没有必要再强引用一次，此时也会使用weak，自定义控件属性一般也使用weak。
不同点：
1）weak此特性表明该属性定义了一种“非拥有关系”。为这种属性设置新值时，设置方法既不保留新值，也不释放旧值。此特性与assign一样，然而在属性所指的对象遭到销毁时，属性值也会清空。而assign的“设置方法”只会执行针对“纯量类型” (scalar type，例如 CGFloat 或 NSlnteger 等)的简单赋值操作。
2）assign可以用非OC对象，而weak必须用于OC对象。
### 内存管理语义(assign、strong、weak等的区别)?
1）assign “设置方法” 只会执行针对“纯量”的简单赋值操作。
2）strong 此特性表明该属性定义了一种“拥有关系”。为这种属性设置新值时，设置方法会先保留新值，并释放旧值，然后再将新值设置上去。
3）weak 此特性表明该属性定义了一种“非拥有关系”。为这种属性设置新值时，设置方法既不保留新值，也不释放旧值。此特性同assign类似，然而在属性所指的对象遭到销毁时，属性值也会清空。
4）`unsafe_unretained `此特性的语义和assign相同，但是它适用于“对象类型”，该特性表达一种“非拥有关系”，当目标对象遭到销毁时，属性值不会自动清空，这一点与weak有区别。
5）copy 此特性所表达的所属关系与strong类似。然而设置方法并不保留新值，而是将其“拷贝”。当属性类型为`NSString*`时，经常用此特性来保护其封装性，因为传递给设置方法的新值有可能指向一个NSMutableString类的实例。这个类是NSString的子类，表示一种可以修改其值的字符串，此时若是不拷贝字符串，那么设置完属性之后，字符串的值就可能会在对象不知情的情况下遭人更改。所以，这时就要拷贝一份“不可变”的字符串，确保对象中的字符串值不会无意间变动。只要实现属性所用的对象是“可变的”，就应该在设置新属性值时拷贝一份。
### view是怎么刷新的？
### Android中数据存储有哪些方法？
### 安卓的广播机制，要在哪里注册广播，哪个生命周期中注册广播

### android中的消息队列模型和Handler。
### android中如何加载网络数据？可以在主线程中获取吗？
### android中如何在子线程中更新UI,比如要在子线程中更新界面上的button上的文字，应该如何做？
### android中多线程间如何通信的？主线程如何知道子线程中信息是否加载好了？
### android中activity的四种加载方式？若是singtop模式，那么在一个activity的startActivity（）方法中调用自己，那么它会经历哪些生命周期？
### 在activity中finish（）方法后面的代码是否会执行？
### activity生命周期，单例模式
### string类型可以继承么，为什么？
### 写出并分析Android的activity的生命周期
### android的布局页面为什么一定要用activity，直接用view不行吗

> [呼呼呼山]()(http://code4fun.me)
> 11 Jan 2018 10:46 PM 

