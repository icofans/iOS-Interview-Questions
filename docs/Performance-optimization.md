# 性能优化
## 1.造成tableView卡顿的原因有哪些？

- 1.最常用的就是cell的重用， 注册重用标识符

	如果不重用cell时，每当一个cell显示到屏幕上时，就会重新创建一个新的cell

	如果有很多数据的时候，就会堆积很多cell。

	如果重用cell，为cell创建一个ID，每当需要显示cell 的时候，都会先去缓冲池中寻找可循环利用的cell，如果没有再重新创建cell

- 2.避免cell的重新布局

	cell的布局填充等操作 比较耗时，一般创建时就布局好

	如可以将cell单独放到一个自定义类，初始化时就布局好

- 3.提前计算并缓存cell的属性及内容

	当我们创建cell的数据源方法时，编译器并不是先创建cell 再定cell的高度

	而是先根据内容一次确定每一个cell的高度，高度确定后，再创建要显示的cell，滚动时，每当cell进入凭虚都会计算高度，提前估算高度告诉编译器，编译器知道高度后，紧接着就会创建cell，这时再调用高度的具体计算方法，这样可以方式浪费时间去计算显示以外的cell

- 4.减少cell中控件的数量

	尽量使cell得布局大致相同，不同风格的cell可以使用不用的重用标识符，初始化时添加控件，

	不适用的可以先隐藏

- 5.不要使用ClearColor，无背景色，透明度也不要设置为0

	渲染耗时比较长

- 6.使用局部更新

	如果只是更新某组的话，使用reloadSection进行局部更

- 7.加载网络数据，下载图片，使用异步加载，并缓存

- 8.少使用addView 给cell动态添加view

- 9.按需加载cell，cell滚动很快时，只加载范围内的cell

- 10.不要实现无用的代理方法，tableView只遵守两个协议

- 11.缓存行高：estimatedHeightForRow不能和HeightForRow里面的layoutIfNeed同时存在，这两者同时存在才会出现“窜动”的bug。所以我的建议是：只要是固定行高就写预估行高来减少行高调用次数提升性能。如果是动态行高就不要写预估方法了，用一个行高的缓存字典来减少代码的调用次数即可

- 12.不要做多余的绘制工作。在实现drawRect:的时候，它的rect参数就是需要绘制的区域，这个区域之外的不需要进行绘制。例如上例中，就可以用CGRectIntersectsRect、CGRectIntersection或CGRectContainsRect判断是否需要绘制image和text，然后再调用绘制方法。

- 13.预渲染图像。当新的图像出现时，仍然会有短暂的停顿现象。解决的办法就是在bitmap context里先将其画一遍，导出成UIImage对象，然后再绘制到屏幕；

- 14.使用正确的数据结构来存储数据。

## 2.如何提升 tableview 的流畅度？ 
- 本质上是降低 CPU、GPU 的工作，从这两个大的方面去提升性能。

	CPU：对象的创建和销毁、对象属性的调整、布局计算、文本的计算和排版、图片的格式转换和解码、图像的绘制
	
	GPU：纹理的渲染
- 卡顿优化在 CPU 层面
	
	尽量用轻量级的对象，比如用不到事件处理的地方，可以考虑使用 CALayer 取代 UIView
	
	不要频繁地调用 UIView 的相关属性，比如 frame、bounds、transform 等属性，尽量减少不必要的修改
	
	尽量提前计算好布局，在有需要时一次性调整对应的属性，不要多次修改属性
	
	Autolayout 会比直接设置 frame 消耗更多的 CPU 资源
	
	图片的 size 最好刚好跟 UIImageView 的 size 保持一致
	
	控制一下线程的最大并发数量
	
	尽量把耗时的操作放到子线程
	
	文本处理（尺寸计算、绘制）
	
	图片处理（解码、绘制）

- 卡顿优化在 GPU层面

	尽量避免短时间内大量图片的显示，尽可能将多张图片合成一张进行显示

	GPU能处理的最大纹理尺寸是 4096x4096，一旦超过这个尺寸，就会占用 CPU 资源进行处理，所以纹理尽量不要超过这个尺寸
	
	尽量减少视图数量和层次
	
	减少透明的视图（alpha<1），不透明的就设置 opaque 为 YES
	
	尽量避免出现离屏渲染

- iOS 保持界面流畅的技巧

	1.预排版，提前计算
	
	在接收到服务端返回的数据后，尽量将 CoreText 排版的结果、单个控件的高度、cell 整体的高度提前计算好，将其存储在模型的属性中。需要使用时，直接从模型中往外取，避免了计算的过程。

	尽量少用 UILabel，可以使用 CALayer 。避免使用 AutoLayout 的自动布局技术，采取纯代码的方式

	2.预渲染，提前绘制
	
	例如圆形的图标可以提前在，在接收到网络返回数据时，在后台线程进行处理，直接存储在模型数据里，回到主线程后直接调用就可以了

	避免使用 CALayer 的 Border、corner、shadow、mask 等技术，这些都会触发离屏渲染。

	3.异步绘制
	
	4.全局并发线程
	
	5.高效的图片异步加载

## 3.APP启动时间应从哪些方面优化？
	
App启动时间可以通过xcode提供的工具来度量，在Xcode的Product->Scheme-->Edit Scheme->Run->Auguments中，将环境变量DYLD_PRINT_STATISTICS设为YES，优化需以下方面入手

- dylib loading time

	核心思想是减少dylibs的引用
	
	合并现有的dylibs（最好是6个以内）
	
	使用静态库

- rebase/binding time

	核心思想是减少DATA块内的指针
	
	减少Object C元数据量，减少Objc类数量，减少实例变量和函数（与面向对象设计思想冲突）
	
	减少c++虚函数
	
	多使用Swift结构体（推荐使用swift）

- ObjC setup time
	
	核心思想同上，这部分内容基本上在上一阶段优化过后就不会太过耗时
	
	initializer time

- 使用initialize替代load方法
	
	减少使用c/c++的attribute((constructor))；推荐使用dispatch_once() pthread_once() std:once()等方法
	
	推荐使用swift
	
	不要在初始化中调用dlopen()方法，因为加载过程是单线程，无锁，如果调用dlopen则会变成多线程，会开启锁的消耗，同时有可能死锁
	
	不要在初始化中创建线程

## 4.如何降低APP包的大小

降低包大小需要从两方面着手

- 可执行文件

	编译器优化：Strip Linked Product、Make Strings Read-Only、Symbols Hidden by Default 设置为 YES，去掉异常支持，Enable C++ Exceptions、Enable Objective-C Exceptions 设置为 NO， Other C Flags 添加 -fno-exceptions
利用 AppCode 检测未使用的代码：菜单栏 -> Code -> Inspect Code

	编写LLVM插件检测出重复代码、未被调用的代码

- 资源（图片、音频、视频 等）

	优化的方式可以对资源进行无损的压缩
	
	去除没有用到的资源： https://github.com/tinymind/LSUnusedResources

## 5.如何检测离屏渲染与优化

- 检测，通过勾选Xcode的Debug->View Debugging-->Rendering->Run->Color Offscreen-Rendered Yellow项。

- 优化，如阴影，在绘制时添加阴影的路径

## 6.怎么检测图层混合

1、模拟器debug中color blended layers红色区域表示图层发生了混合

2、Instrument-选中Core Animation-勾选Color Blended Layers

避免图层混合：

- 确保控件的opaque属性设置为true，确保backgroundColor和父视图颜色一致且不透明

- 如无特殊需要，不要设置低于1的alpha值

- 确保UIImage没有alpha通道

UILabel图层混合解决方法：

iOS8以后设置背景色为非透明色并且设置label.layer.masksToBounds=YES让label只会渲染她的实际size区域，就能解决UILabel的图层混合问题

iOS8 之前只要设置背景色为非透明的就行

为什么设置了背景色但是在iOS8上仍然出现了图层混合呢？

UILabel在iOS8前后的变化，在iOS8以前，UILabel使用的是CALayer作为底图层，而在iOS8开始，UILabel的底图层变成了_UILabelLayer，绘制文本也有所改变。在背景色的四周多了一圈透明的边，而这一圈透明的边明显超出了图层的矩形区域，设置图层的masksToBounds为YES时，图层将会沿着Bounds进行裁剪 图层混合问题解决了

## 7.日常如何检查内存泄露？

- 目前我知道的方式有以下几种

	Memory Leaks
	
	Alloctions
	
	Analyse
	
	Debug Memory Graph
	
	MLeaksFinder

- 泄露的内存主要有以下两种：

	Laek Memory 这种是忘记 Release 操作所泄露的内存。
	
	Abandon Memory 这种是循环引用，无法释放掉的内存。



