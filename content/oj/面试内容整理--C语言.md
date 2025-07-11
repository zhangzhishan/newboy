---
title: 面试内容整理--C语言
tags: jobs
categories: 校招
date: 11 Jan 2018 10:39 PM
---
### C++ 继承等顺序
[http://blog.csdn.net/bresponse/article/details/6914155]()

构造函数，显示初始化化基类（父类）的构造函数，然后再分配成员变量C的内存，最后再执行自身构造函数。
构造函数的调用顺序总是如下：
1.基类构造函数。如果有多个基类，则构造函数的调用顺序是某类在类派生表中出现的顺序，而不是它们在成员初始化表中的顺序。
2.成员类对象构造函数。如果有多个成员类对象则构造函数的调用顺序是对象在类中被声明的顺序，而不是它们出现在成员初始化表中的顺序。
3.派生类构造函数。

析构函数
析构函数不能重载。
析构函数的调用顺序与构造函数的调用顺序正好相反，将上面3个点反过来用就可以了，首先调用派生类的析构函数；其次再调用成员类对象的析构函数；最后调用基类的析构函数。
析构函数在下边3种情况时被调用：
1. 对象生命周期结束，被销毁时(一般类成员的指针变量与引用都i不自动调用析构函数)；
2. delete指向对象的指针时，或delete指向对象的基类类型指针，而其基类虚构函数是虚函数时；
3. 对象i是对象o的成员，o的析构函数被调用时，对象i的析构函数也被调用。
### 面向对象设计的原则
原则一：（SRP：Single responsibility principle）单一职责原则又称单一功能原则
原则二：开闭原则（OCP：Open Closed Principle）
核心思想：对扩展开放，对修改关闭。即在设计一个模块的时候，应当使这个模块可以在不被修改的前提下被扩展。
原则三：里氏替换原则（LSP：Liskov Substitution Principle）
原则四：依赖倒转原则(DIP：Dependence Inversion Principle)
要依赖于抽象，不要依赖于具体的实现
### 指针和引用的区别
### static全局变量与普通的全局变量的区别？局部变量呢？函数呢？
1. static全局变量与普通全局变量的区别：static全局变量只初始化一次，防止在其他文件单元中被引用。
2. static局部变量与普通局部变量的区别：static局部变量只被初始化一次，下一次依据上一次结果值。
3. static函数与普通函数的区别：static函数在内存中只有一份，普通函数在每个被调用中维持一份拷贝。
### C++中的内存管理
在C++中,内存分成5个区,分别是堆、栈、自由存储区、全局/静态区和常量存储区.
* 栈:存放函数参数以及局部变量,在出作用域时,将自动被释放.栈内存分配运算内置于处理器的指令集中,效率很高,但分配的内存容量有限.
* 堆:new分配的内存块(包括数组,类实例等),需delete手动释放.如果未释放,在整个程序结束后,OS会帮你回收掉.
* 自由存储区:malloc分配的内存块,需free手动释放.它和堆有些相似.
* 全局/静态区:全局变量(global)和静态变量(static)存于此处.(在以前的C语言中,全局变量又分为初始化的和未初始化的,C++不分)
* 常量存储区:常量(const)存于此处,此存储区不可修改.

其实C++的内存管理容易而且安全,因为当一个对象消除时,它的析构函数能够安全的释放所有分配的内存.在嵌入式系统中,内存的分配是一个常见问题,保守的使用内存分配是嵌入式环境中的第一原则.
当你需使用new/delete时,一个防止堆破碎的通用方法是从不同固定大小的内存池中分配不同类型的对象对每个类重载new和delete就提供了这样的控制.
### C++内存机制和是否会有内存泄漏什么情况下会泄漏
一般我们常说的内存泄漏是指堆内存的泄漏。堆内存是指程序从堆中分配的，大小任意的（内存块的大小可以在程序运行期决定），使用完后必须显示释放的内 存。应用程序一般使用malloc，realloc，new等函数从堆中分配到一块内存，使用完后，程序必须负责相应的调用free或delete释放该 内存块，否则，这块内存就不能被再次使用，我们就说这块内存泄漏了。

广义的说，内存泄漏不仅仅包含堆内存的泄漏，还包含系统资源的泄漏(resource leak)，比如核心态HANDLE，GDI Object，SOCKET， Interface等，从根本上说这些由操作系统分配的对象也消耗内存，如果这些对象发生泄漏最终也会导致内存的泄漏。而且，某些对象消耗的是核心态内 存，这些对象严重泄漏时会导致整个操作系统不稳定。所以相比之下，系统资源的泄漏比堆内存的泄漏更为严重。

以发生的方式来分类，内存泄漏可以分为4类：
1. 常发性内存泄漏。发生内存泄漏的代码会被多次执行到，每次被执行的时候都会导致一块内存泄漏。
2. 偶发性内存泄漏。发生内存泄漏的代码只有在某些特定环境或操作过程下才会发生。
3. 一次性内存泄漏。发生内存泄漏的代码只会被执行一次，或者由于算法上的缺陷，导致总会有一块仅且一块内存发生泄漏。比如，在类的构造函数中分配内存，在析构函数中却没有释放该内存，但是因为这个类是一个Singleton，所以内存泄漏只会发生一次。
4. 隐式内存泄漏。程序在运行过程中不停的分配内存，但是直到结束的时候才释放内存。严格的说这里并没有发生内存泄漏，因为最终程序释放了所有申请的内存。但是对于一个服务器程序，需要运行几天，几周甚至几个月，不及时释放内存也可能导致最终耗尽系统的所有内存。所以，我们称这类内存泄漏为隐式内存泄漏。
### 虚析构、模板和宏
1. 对于虚析构函数，那么就是基类和子类的析构函数都会被调用，先析构子类部分，再析构基类部分。（基类析构函数是被子类析构函数自动调用的）
2. 对于非虚析构函数，子类析构函数不会被调用，只有基类析构函数才会被调用。

模板定义：模板就是实现代码重用机制的一种工具，它可以实现类型参数化，即把类型定义为参数， 从而实现了真正的代码可重用性。模版可以分为两类，一个是函数模版，另外一个是类模版。
[模板]()

### 虚函数实现机制
[http://blog.csdn.net/neiloid/article/details/6934135]()
### vector与list的区别，map是如何实现的，查找效率是多少
1. Vector是顺序容器，是一个动态数组，支持随机存取、插入、删除、查找等操作，在内存中是一块连续的空间。在原有空间不够情况下自动分配空间，增加为原来的两倍。vector随机存取效率高，但是在vector插入元素，需要移动的数目多，效率低下。
注意：vector动态增加大小时，并不是在原空间之后持续新空间（因为无法保证原空间之后尚有可供配置的空间），而是以原大小的两倍另外配置一块较大的空间，然后将原内容拷贝过来，然后才开始在原内容之后构造新元素，并释放原空间。因此，对vector的任何操作，一旦引起空间重新配置，指向原vector的所有迭代器就都失效了。

2. Map是关联容器，以键值对的形式进行存储，方便进行查找。关键词起到索引的作用，值则表示与索引相关联的数据。以红黑树的结构实现，插入删除等操作都在O(logn)时间内完成。
注意：map的下标操作，其行为与vector很不相同：使用一个不在容器中关键字作为下标，会添加一个具有此关键字的元素到map中。一般使用find函数代替下标操作。
```cpp
map<string,   int>   my_Map; 
my_Map["a"]   =   1; 
my_Map.insert(map<string,   int>::value_type("b",2)); 
my_Map.insert(pair<string,int>("c",3)); 
my_Map.insert(make_pair<string,int>("d",4)); 
int   i   =   my_Map["a"];  my_Map["a"]   =   i; 
MY_MAP::iterator my_Itr; my_Itr.find("b"); int   j   =   my_Itr->second; my_Itr->second   =   j; 
my_Map.erase(my_Itr); 
my_Map.erase("c"); 
for   (my_Itr=my_Map.begin();   my_Itr!=my_Map.end();   ++my_Itr)   {} 
my_Map.size()               返回元素数目 
my_Map.empty()       判断是否为空 
my_Map.clear()           清空所有元素 
```


3. Set是关联容器，set中每个元素只包含一个关键字。set支持高效的关键字查询操作——检查一个给定的关键字是否在set中。set也是以红黑树的结构实现，支持高效插入、删除等操作。
4. vector数据结构
vector和数组类似，拥有一段连续的内存空间，并且起始地址不变。
因此能高效的进行随机存取，时间复杂度为o(1);
但因为内存空间是连续的，所以在进行插入和删除操作时，会造成内存块的拷贝，时间复杂度为o(n)。
另外，当数组中内存空间不够时，会重新申请一块内存空间并进行内存拷贝。
5. list数据结构
list是由双向链表实现的，因此内存空间是不连续的。
只能通过指针访问数据，所以list的随机存取非常没有效率，时间复杂度为o(n);
但由于链表的特点，能高效地进行插入和删除。
### extern 关键字有什么用
extern可以置于变量或者函数前，以标示变量或者函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。此外extern也可用来进行链接指定。
C语言中extern的作用，extern “C”的作用？
1）extern可以置于变量或者函数前，以标示变量或函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。
2）C++语言在编译的时候为了解决函数的多态问题，会将函数名和参数联合起来生成一个中间的函数名称，而C语言则不会，因此会造成链接时找不到对应函数的情况，此时C函数就需要用extern “C”进行链接指定，这告诉编译器，请保持我的名称，不要给我生成用于链接的中间函数名。
### malloc和new的区别，能否malloc(1.2G)
### C++多线程编程  网络编程
### C语言中的static变量和static函数有什么作用？
1）表示变量是静态存储变量，表示变量存放在静态存储区。
2）加在函数前面的时候表示该函数是内部连接，只在本文件中有效，别的文件中不能使用该函数。
### C++面向对象的几个特征及其实现
面向对象的三个基本特征是：封装、继承、多态。
#### 封装
封装最好理解了。封装是面向对象的特征之一，是对象和类概念的主要特性。
封装，也就是把客观事物封装成抽象的类，并且类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏。
#### 继承
面向对象编程 (OOP) 语言的一个主要功能就是“继承”。继承是指这样一种能力：它可以使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。
通过继承创建的新类称为“子类”或“派生类”。
被继承的类称为“基类”、“父类”或“超类”。
继承的过程，就是从一般到特殊的过程。
要实现继承，可以通过“继承”（Inheritance）和“组合”（Composition）来实现。
在某些 OOP 语言中，一个子类可以继承多个基类。但是一般情况下，一个子类只能有一个基类，要实现多重继承，可以通过多级继承来实现。
继承概念的实现方式有三类：实现继承、接口继承和可视继承。
+ 实现继承是指使用基类的属性和方法而无需额外编码的能力；
+ 接口继承是指仅使用属性和方法的名称、但是子类必须提供实现的能力；
+ 可视继承是指子窗体（类）使用基窗体（类）的外观和实现代码的能力。
在考虑使用继承时，有一点需要注意，那就是两个类之间的关系应该是“属于”关系。例如，Employee 是一个人，Manager 也是一个人，因此这两个类都可以继承 Person 类。但是 Leg 类却不能继承 Person 类，因为腿并不是一个人。
抽象类仅定义将由子类创建的一般属性和方法，创建抽象类时，请使用关键字 Interface 而不是 Class。
OO开发范式大致为：划分对象→抽象类→将类组织成为层次化结构(继承和合成) →用类与实例进行设计和实现几个阶段
#### 多态
多态性（polymorphisn）是允许你将父对象设置成为和一个或更多的他的子对象相等的技术，赋值之后，父对象就可以根据当前赋值给它的子对象的特性以不同的方式运作。简单的说，就是一句话：允许将子类类型的指针赋值给父类类型的指针。
实现多态，有二种方式，覆盖，重载。
覆盖，是指子类重新定义父类的虚函数的做法。
重载，是指允许存在多个同名函数，而这些函数的参数表不同（或许参数个数不同，或许参数类型不同，或许两者都不同）。
其实，重载的概念并不属于“面向对象编程”，重载的实现是：编译器根据函数不同的参数表，对同名函数的名称做修饰，然后这些同名函数就成了不同的函数（至少对于编译器来说是这样的）。如，有两个同名函数：`function func(p:integer):integer;`和`function func(p:string):integer;`。那么编译器做过修饰后的函数名称可能是这样的：`int_func、str_func`。对于这两个函数的调用，在编译器间就已经确定了，是静态的（记住：是静态）。也就是说，它们的地址在编译期就绑定了（早绑定），因此，重载和多态无关！真正和多态相关的是“覆盖”。当子类重新定义了父类的虚函数后，父类指针根据赋给它的不同的子类指针，动态（记住：是动态！）的调用属于子类的该函数，这样的函数调用在编译期间是无法确定的（调用的子类的虚函数的地址无法给出）。因此，这样的函数地址是在运行期绑定的（晚邦定）。结论就是：重载只是一种语言特性，与多态无关，与面向对象也无关！引用一句Bruce Eckel的话：“不要犯傻，如果它不是晚邦定，它就不是多态。”
那么，多态的作用是什么呢？我们知道，封装可以隐藏实现细节，使得代码模块化；继承可以扩展已存在的代码模块（类）；它们的目的都是为了——代码重用。而多态则是为了实现另一个目的——接口重用！多态的作用，就是为了类在继承和派生的时候，保证使用“家谱”中任一类的实例的某一属性时的正确调用。
### 虚函数的内存管理（常见问题，建议把虚函数表、多态的实现过程这些都讲一讲）
[http://blog.csdn.net/haoel/article/details/1948051]()
### 全局与局部静态变量区别
#### 从作用域看：
1. 全局变量具有全局作用域。全局变量只需在一个源文件中定义，就可以作用于所有的源文件。当然，其他不包含全局变量定义的源文件需要用extern关键字再次声明这个全局变量。
2. 静态局部变量具有局部作用域，它只被初始化一次，自从第一次被初始化直到程序运行结束都一直存在，它和全局变量的区别在于全局变量对所有的函数都是可见的，而静态局部变量只对定义自己的函数体始终可见。
3. 局部变量也只有局部作用域，它是自动对象（auto），它在程序运行期间不是一直存在，而是只在函数执行期间存在，函数的一次调用执行结束后，变量被撤销，其所占用的内存也被收回。
4. 静态全局变量也具有全局作用域，它与全局变量的区别在于如果程序包含多个文件的话，它作用于定义它的文件里，不能作用到其它文件里，即被static关键字修饰过的变量具有文件作用域。这样即使两个不同的源文件都定义了相同名字的静态全局变量，它们也是不同的变量。
#### 从分配内存空间看：
1. 全局变量，静态局部变量，静态全局变量都在静态存储区分配空间，而局部变量在栈里分配空间
2. 全局变量本身就是静态存储方式，静态全局变量当然也是静态存储方式。这两者在存储方式上并无不同。这两者的区别虽在于非静态全局变量的作用域是整个源程序，当一个源程序由多个源文件组成时，非静态的全局变量在各个源文件中都是有效的。而静态全局变量则限制了其作用域，即只在定义该变量的源文件内有效，在同一源程序的其它源文件中不能使用它。由于静态全局变量的作用域局限于一个源文件内，只能为该源文件内的函数公用，因此可以避免在其它源文件中引起错误。
1)静态变量会被放在程序的静态数据存储区(全局可见)中，这样可以在下一次调用的时候还可以保持原来的赋值。这一点是它与堆栈变量和堆变量的区别。
2)变量用static告知编译器，自己仅仅在变量的作用范围内可见。这一点是它与全局变量的区别。
从以上分析可以看出，把局部变量改变为静态变量后是改变了它的存储方式即改变了它的生存期。把全局变量改变为静态变量后是改变了它的作用域，限制了它的使用范围。因此static这个说明符在不同的地方所起的作用是不同的。应予以注意。
### 空类默认生成哪些函数
构造函数、析构函数、拷贝构造函数、以及拷贝赋值函数
### explicit关键字的作用
修饰构造函数，用于抑制隐式转换，“原则上应该在所有的构造函数前加explicit” 
### 类如何实现只能静态分配和只能动态分配
前者是把new、delete运算符重载为private属性。后者是把构造、析构函数设为protected属性，再用子类来动态创建
### 面向对象编程里面的多态？
多态的定义：如果编译时和运行时类项不一样就会出现所谓的多态（polymorphism），多态可以向上转型。
• 编译时类项：声明该变量时使用的类型决定
• 运行时类项：实际赋给该变量的对象决定
实现多态的技术称为：动态绑定（dynamic binding），是指在执行期间判断所引用对象的实际类型，根据其实际的类型调用其相应的方法。
多态存在的三个必要条件：
一、要有继承； 
二、要有重写； 
三、父类引用指向子类对象。
父类引用生成子类对象，那这个引用只能调用在父类中已经定义过的属性和方法，而对子类自己新定义的属性和方法则不能访问。
### struct和union的区别？union应用场景是什么？
1.在存储多个成员信息时，编译器会自动给struct多个成员分配存储空间，struct 可以存储多个成员信息，而Union每个成员会用同一个存储空间，只能存储最后一个成员的信息。
2.都是由多个不同的数据类型成员组成，但在任何同一时刻，Union只存放了一个被先选中的成员，而结构体的所有成员都存在。
3.对于Union的不同成员赋值，将会对其他成员重写，原来成员的值就不存在了，而对于struct 的不同成员赋值 是互不影响的。
注：在很多地方需要对结构体的成员变量进行修改。只是部分成员变量，那么就不能用联合体Union，因为Union的所有成员变量占一个内存。eg：在链表中对个别数值域进行赋值就必须用struct.
### C++构造函数调用虚函数有什么问题
程序不会报错，但不会有动态绑定的效果了。
### static静态变量
在C++中，静态成员变量是属于整个类的，它被该类的所有对象共享。而对于普通的成员变量，每个对象都拥有自己单独的成员变量。
### 重载、覆盖和隐藏。
重载和重写的区别：
（1）范围区别：重写和被重写的函数在不同的类中，重载和被重载的函数在同一类中。
（2）参数区别：重写与被重写的函数参数列表一定相同，重载和被重载的函数参数列表一定不同。
（3）virtual的区别：重写的基类必须要有virtual修饰，重载函数和被重载函数可以被virtual修饰，也可以没有。
隐藏和重写，重载的区别：
（1）与重载范围不同：隐藏函数和被隐藏函数在不同类中。
（2）参数的区别：隐藏函数和被隐藏函数参数列表可以相同，也可以不同，但函数名一定同；当参数不同时，无论基类中的函数是否被virtual修饰，基类函数都是被隐藏，而不是被重写。
### 重写和重载有什么区别
重载就是：是函数名相同，参数列表不同 重载只是在类的内部存在。
重写：也叫做覆盖。子类重新定义父类中有相同名称和参数的虚函数。说白了，就是在继承关系中出现的 。
一句话就是重载是同一层次函数名相同。覆盖是在继承层次中成员函数的函数原型完全相同。
重写主要标志特征：
1. 被重写的函数不是static的。必须是virtual的。
2 重写函数必须有相同的类型，名称和参数列表
### 什么是字节对齐？怎么修改字节对齐的规则？
现代计算机中内存空间都是按照byte划分的，从理论上讲似乎对任何类型的变量的访问可以从任何地址开始，但实际情况是在访问特定类型变量的时候经常在特 定的内存地址访问，这就需要各种类型数据按照一定的规则在空间上排列，而不是顺序的一个接一个的排放，这就是对齐。
    对齐的作用和原因：各个硬件平台对存储空间的处理上有很大的不同。一些平台对某些特定类型的数据只能从某些特定地址开始存取。比如有些架构的CPU在访问 一个没有进行对齐的变量的时候会发生错误,那么在这种架构下编程必须保证字节对齐.其他平台可能没有这种情况，但是最常见的是如果不按照适合其平台要求对 数据存放进行对齐，会在存取效率上带来损失。比如有些平台每次读都是从偶地址开始，如果一个int型（假设为32位系统）如果存放在偶地址开始的地方，那 么一个读周期就可以读出这32bit，而如果存放在奇地址开始的地方，就需要2个读周期，并对两次读出的结果的高低字节进行拼凑才能得到该32bit数 据。显然在读取效率上下降很多。

`#pragma pack (value)`时的指定对齐值value
### 序列化和反序列化？
 消息序列化
    将具有一定结构的数据转换成可以存取或者可以传输的形式，比如转换成字符流的形式(`char *`)。通过序列化后，消息可以在系统各逻辑模块或者网络中传输。
 
消息反序列化
    顾名思义，反序列化与序列化的过程相反。例如，将网络中传过来的放在缓冲区的字节流数据(`char *`)解码成具有一定含义和结构的数据，然后便可以对接出来的数据进行处理。

> [呼呼呼山]()(http://code4fun.me)
> 11 Jan 2018 10:39 PM 

