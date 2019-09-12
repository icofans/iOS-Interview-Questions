# UIKit
## 1.UIView 和 CALayer 是什么关系？ 

- UIView 继承 UIResponder，而 UIResponder 是响应者对象，可以对iOS 中的事件响应及传递，CALayer 没有继承自 UIResponder，所以 CALayer 不具备响应处理事件的能力。CALayer 是 QuartzCore 中的类，是一个比较底层的用来绘制内容的类，用来绘制UI

- UIView 对 CALayer 封装属性，对 UIView 设置 frame、center、bounds 等位置信息时，其实都是UIView 对 CALayer 进一层封装，使得我们可以很方便地设置控件的位置；例如圆角、阴影等属性， UIView 就没有进一步封装，所以我们还是需要去设置 Layer 的属性来实现功能。

- UIView 是 CALayer 的代理，UIView 持有一个 CALayer 的属性，并且是该属性的代理，用来提供一些 CALayer 行的数据，例如动画和绘制。

## 2.Bounds 和 Frame 的区别?

- Bounds：一般是相对于自身来说的，是控件的内部尺寸。如果你修改了 Bounds，那么子控件的相对位置也会发生改变。

- Frame ：是相对于父控件来说的，是控件的外部尺寸。

## 3.setNeedsDisplay 和 layoutIfNeeded 两者是什么关系？

UIView的setNeedsDisplay和setNeedsLayout两个方法都是异步执行的。而setNeedsDisplay会自动调用drawRect方法，这样可以拿到UIGraphicsGetCurrentContext进行绘制；而setNeedsLayout会默认调用layoutSubViews，给当前的视图做了标记；layoutIfNeeded 查找是否有标记，如果有标记及立刻刷新。

只有setNeedsLayout和layoutIfNeeded这二者合起来使用，才会起到立刻刷新的效果。

## 4.谈谈对UIResponder的理解

UIResponder类是专门用来响应用户的操作处理各种事件的，包括触摸事件(Touch Events)、运动事件(Motion Events)、远程控制事件(Remote Control Events)。我们知道UIApplication、UIView、UIViewController这几个类是直接继承自UIResponder，所以这些类都可以响应事件。当然我们自定义的继承自UIView的View以及自定义的继承自UIViewController的控制器都可以响应事件。

## 5.loadView的作用？

loadView方法会在每次访问UIViewController的view(比如controller.view、self.view)而且view为nil时会被调用，此方法主要用来负责创建UIViewController的view(重写loadView方法，并且不需要调用[super loadView])

这里要提一下 [super loadView]，[super loadView]做了下面几件事。

- 它会先去查找与UIViewController相关联的xib文件，通过加载xib文件来创建UIViewController的view，如果在初始化UIViewController指定了xib文件名，就会根据传入的xib文件名加载对应的xib文件，如果没有明显地传xib文件名，就会加载跟UIViewController同名的xib文件

- 如果没有找到相关联的xib文件，就会创建一个空白的UIView，然后赋值给UIViewController的view属性

综上，在需要自定义UIViewController的view时，可以通过重写loadView方法且不需要调用[super loadView]方法。

## 6.使用 drawRect有什么影响？

drawRect 方法依赖 Core Graphics 框架来进行自定义的绘制
缺点：它处理 touch 事件时每次按钮被点击后，都会用 setNeddsDisplay 进行强制重绘；而且不止一次，每次单点事件触发两次执行。这样的话从性能的角度来说，对 CPU 和内存来说都是欠佳的。特别是如果在我们的界面上有多个这样的UIButton 实例，那就会很糟糕了。这个方法的调用机制也是非常特别. 当你调用 setNeedsDisplay 方法时, UIKit 将会把当前图层标记为 dirty,但还是会显示原来的内容,直到下一次的视图渲染周期,才会将标记为 dirty 的图层重新建立 Core Graphics 上下文,然后将内存中的数据恢复出来, 再使用 CGContextRef 进行绘制

## 7.keyWindow 和 delegate的window有何区别

- delegate.window 程序启动时设置的window对象。

- keyWindow 这个属性保存了[windows]数组中的[UIWindow]对象，该对象最近被发送了[makeKeyAndVisible]消息

一般情况下 delegate.window 和 keyWindow 是同一个对象，但不能保证keyWindow就是delegate.window，因为keyWindow会因为makeKeyAndVisible而变化，例如，程序中添加了一个悬浮窗口，这个时候keywindow就会变化。
