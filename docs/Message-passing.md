# 消息传递的方式

## 1.KVC实现原理

- KVC，键-值编码，使用字符串直接访问对象的属性。

- 底层实现，当一个对象调用setValue方法时，方法内部会做以下操作：

	1.检查是否存在相应key的set方法，如果存在，就调用set方法

	2.如果set方法不存在，就会查找与key相同名称并且带下划线的成员属性，如果有，则直接给成员属性赋值

	3.如果没有找到_key，就会查找相同名称的属性key，如果有就直接赋值

	4.如果还没找到，则调用valueForUndefinedKey：和setValue：forUndefinedKey：方法

## 2.KVO的实现原理

![KVO](https://qn.nobady.cn/iOS/kvo.png)

KVO-键值观察机制，原理如下：

- 1.当给A类添加KVO的时候，runtime动态的生成了一个子类NSKVONotifying_A，让A类的isa指针指向NSKVONotifying_A类，重写class方法，隐藏对象真实类信息

- 2.重写监听属性的setter方法，在setter方法内部调用了Foundation 的 _NSSetObjectValueAndNotify 函数

- 3._NSSetObjectValueAndNotify函数内部
	
	a) 首先会调用 willChangeValueForKey 

	b) 然后给属性赋值 

	c) 最后调用 didChangeValueForKey 

	d) 最后调用 observer 的 observeValueForKeyPath 去告诉监听器属性值发生了改变 .

- 4.重写了dealloc做一些 KVO 内存释放 

## 3.如何手动触发KVO方法

- 手动调用willChangeValueForKey和didChangeValueForKey方法

- 键值观察通知依赖于 NSObject 的两个方法: willChangeValueForKey: 和 didChangeValueForKey。在一个被观察属性发生改变之前， willChangeValueForKey: 一定会被调用，这就 会记录旧的值。而当改变发生后， didChangeValueForKey 会被调用，继而 observeValueForKey:ofObject:change:context: 也会被调用。如果可以手动实现这些调用，就可以实现“手动触发”了
有人可能会问只调用didChangeValueForKey方法可以触发KVO方法，其实是不能的，因为willChangeValueForKey: 记录旧的值，如果不记录旧的值，那就没有改变一说了

## 4.通知和代理有什么区别

- 通知是观察者模式，适合一对多的场景

- 代理模式适合一对一的反向传值

- 通知耦合度低，代理的耦合度高

## 5.block和delegate的区别

- delegate运行成本低，block的运行成本高
	
	block出栈需要将使用的数据从栈内存拷贝到堆内存，当然对象的话就是加计数，使用完或者block置nil后才消除。delegate只是保存了一个对象指针，直接回调，没有额外消耗。就像C的函数指针，只多做了一个查表动作。

- delegate更适用于多个回调方法（3个以上），block则适用于1，2个回调时。

## 6.为什么Block用copy关键字

Block在没有使用外部变量时，内存存在全局区，然而，当Block在使用外部变量的时候，内存是存在于栈区，当Block copy之后，是存在堆区的。存在于栈区的特点是对象随时有可能被销毁，一旦销毁在调用的时候，就会造成系统的崩溃。所以Block要用copy关键字。
