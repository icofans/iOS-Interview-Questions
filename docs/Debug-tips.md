# 调试技巧

## 1.LLDB常用的调试命令？

- po：print object的缩写，表示显示对象的文本描述，如果对象不存在则打印nil。

- p：可以用来打印基本数据类型。

- call：执行一段代码 如：call NSLog(@"%@", @"yang")

- expr：动态执行指定表达式

- bt：打印当前线程堆栈信息 （bt all 打印所有线程堆栈信息）

- image：常用来寻找栈地址对应代码位置 如：image lookup --address 0xxxx

## 2.断点调试

- 条件断点

	打上断点之后，对断点进行编辑，设置相应过滤条件。下面简单的介绍一下条件设置：

	Condition：返回一个布尔值，当布尔值为真触发断点，一般里面我们可以写一个表达式。

	Ignore：忽略前N次断点，到N+1次再触发断点。

	Action：断点触发事件，分为六种：

	- AppleScript：执行脚本。

	- Capture GPU Frame：用于OpenGL ES调试，捕获断点处GPU当前绘制帧。

	- Debugger Command：和控制台中输入LLDB调试命令一致。

	- Log Message：输出自定义格式信息至控制台。

	- Shell Command：接收命令文件及相应参数列表，Shell Command是异步执行的，只有勾选“Wait until done”才会等待Shell命令执行完在执行调试。

	- Sound：断点触发时播放声音。

	Options(Automatically continue after evaluating actions选项)：选中后，表示断点不会终止程序的运行。

- 异常断点

	异常断点可以快速定位不满足特定条件的异常，比如常见的数组越界，这时候很难通过异常信息定位到错误所在位置。这个时候异常断点就可以发挥作用了。

	Exception：可以选择抛出异常对象类型：OC或C++。

	Break：选择断点接收的抛出异常来源是Throw还是Catch语句。

- 符号断点

	符号断点的创建方式和异常断点一样一样的，在符号断点中可以指定要中断执行的方法：

	Symbol:[类名 方法名]可以执行到指定类的指定方法中开始断点。

## 3.iOS 常见的崩溃类型有哪些？

- unrecognized selector crash

- KVO crash

- NSNotification crash

- NSTimer crash

- Container crash

- NSString crash

- Bad Access crash （野指针）

- UI not on Main Thread Crash

