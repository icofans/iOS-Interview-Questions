# 项目架构
## 1.MVC、MVP、MVVM模式

### MVC（Model、View、Controller）

MVC是比较直观的架构模式，最核心的就是通过Controller层来进行调控，首先看一下官方提供的MVC示意图：

![mvc](https://qn.nobady.cn/iOS/mvc.png)

- Model和View永远不能相互通信，只能通过Controller传递

- Controller可以直接与Model对话（读写调用Model），Model通过NOtification和KVO机制与Controller间接通信

Controller可以直接与View对话，通过IBoutlet直接操作View，IBoutlet直接对应View的控件（例如创建一个Button：需声明一个  IBOutlet UIButton * btn），View通过action向Controller报告时间的发生(用户点击了按钮)。Controller是View的直接数据源

- 优点：对于混乱的项目组织方式，有了一个明确的组织方式。通过Controller来掌控全局，同时将View展示和Model的变化分开

- 缺点：愈发笨重的Controller，随着业务逻辑的增加，大量的代码放进Controller，导致Controller越来越臃肿，堆积成千上万行代码，后期维护起来费时费力

### MVP（Model、View、Presenter）
MVP模式是MVC模式的一个演化版本，其中Model与MVC模式中Model层没有太大区别，主要提供数据存储功能，一般都是用来封装网络获取的json数据；View与MVC中的View层有一些差别，MVP中的View层可以是viewController、view等控件；Presenter层则是作为Model和View的中介，从Model层获取数据之后传给View。

![mvp](https://qn.nobady.cn/iOS/mvp.png)

从上图可以看出，从MVC模式中增加了Presenter层，将UIViewController中复杂的业务逻辑、网络请求等剥离出来。

- 优点 模型和视图完全分离，可以做到修改视图而不影响模型；更高效的使用模型，View不依赖Model，可以说VIew能做到对业务逻辑完全分离

- 缺点 Presenter中除了处理业务逻辑以外，还要处理View-Model两层的协调，也会导致Presenter层的臃肿

### MVVM（Model、Controller/View、ViewModel）

在MVVM中，view和ViewCOntroller联系在一起，我们把它们视为一个组件，view和ViewController都不能直接引用model，而是引用是视图模型即ViewModel。
viewModel是一个用来放置用户输入验证逻辑、视图显示逻辑、网络请求等业务逻辑的地方，这样的设计模式，会轻微增加代码量，但是会减少代码的复杂性

- 优点 VIew可以独立于Model的变化和修改，一个ViewModel可以绑定到不同的View上，降低耦合，增加重用

- 缺点 过于简单的项目不适用、大型的项目视图状态较多时构建和维护成本太大

合理的运用架构模式有利于项目、团队开发工作，但是到底选择哪个设计模式，哪种设计模式更好，就像本文开头所说，不同的设计模式，只是让不同的场景有了更多的选择方案。根据项目场景和开发需求，选择最合适的解决方案。


## 2.关于RAC你有怎样运用到解决不同API依赖关系

- 信号的依赖：使用场景是当信号A执行完才会执行信号B,和请求的依赖很类似,例如请求A请求完毕才执行请求B,我们需要注意信号A必须要执行发送完成信号,否则信号B无法执行

	``` c
	//这相当于网络请求中的依赖,必须先执行完信号A才会执行信号B
	//经常用作一个请求执行完毕后,才会执行另一个请求
	//注意信号A必须要执行发送完成信号,否则信号B无法执行
	RACSignal * concatSignal = [self.signalA concat:self.signalB]
	//这里我们是对这个拼接信号进行订阅
	[concatSignal subscribeNext:^(id x) {
		NSLog(@"%@",x);
	}];
	```
## 3.@weakify和我们宏定义的WeakSelf有什么区别？

@weakify 可以多参数使用

## 4.微服务架构设想。

微服务架构具有以下优势：

- 1.灵活和独立的可扩展性
 
	灵活扩展是微服务架构的主要优势之一。与单片架构不同，每个模块都可以水平扩展并独立于其他模块。因此，微服务架构非常适合大型项目。 

- 2.独立技术堆栈
 
	在微服务架构中，软件工程师有机会使用各种工具和技术构建APP。代码可以用不同的编程语言编写，这为APP开发过程增加了更多的灵活性。

- 3.更好的故障隔离 
	
	如果一个服务失败，它不会影响其他服务的功能。与其他体系结构样式相比，在微服务中，系统继续工作，单片模式下的问题会影响整个APP。

- 4.易于部署和集成

	虽然即使是小代码更改的情况下，开发人员也必须再次部署APP，但在微服务架构中，部署变得更快更轻松。

	由于所有服务都是围绕单一业务流程构建的，因此程序员不必修改和重新部署整个APP，只需要您需要的区域。因此，改进产品也比较简单。

	微服务可以通过全自动部署机制独立部署。此外，通过使用开源持续集成工具，开发人员大大简化了与第三方服务的集成。

- 5.容易理解
 
	微服务体系结构的另一个优点是很容易理解系统是如何工作的以及它是如何开发的。当一个新的团队成员来到这个项目并且必须快速钻研它时，这特别有用。

那么在iOS中如何借鉴这种思想去构建我们的App呢？是需要我们开发者自己去不断探索的