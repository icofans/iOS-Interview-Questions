# 源码理解

## 1.YYKit
	
YYKit 是一组庞大、功能丰富的 iOS 组件。

我们需要从组件架构，源码，设计思路，解决问题的方案策略等多方面去学习。


## 2.SDWebImage加载图片过程

- 0、首先显示占位图

- 1、在webimagecache中寻找图片对应的缓存，它是以url为数据索引先在内存中查找是否有缓存；

- 2、如果没有缓存，就通过md5处理过的key来在磁盘中查找对应的数据，如果找到就会把磁盘中的数据加到内存中，并显示出来；

- 3、如果内存和磁盘中都没有找到，就会向远程服务器发送请求，开始下载图片；

- 4、下载完的图片加入缓存中，并写入到磁盘中；

- 5、整个获取图片的过程是在子线程中进行，在主线程中显示。


## 3.YYWebImage和SDWebImage对比

。。。

## 4.AFNetworking 底层原理分析

AFNetworking是封装的NSURLSession的网络请求，由五个模块组成：分别由NSURLSession,Security,Reachability,Serialization,UIKit五部分组成

- NSURLSession：网络通信模块（核心模块） 对应 AFNetworking中的 AFURLSessionManager和对HTTP协议进行特化处理的AFHTTPSessionManager,AFHTTPSessionManager是继承于AFURLSessionmanager的

- Security：网络通讯安全策略模块  对应 AFSecurityPolicy

- Reachability：网络状态监听模块 对应AFNetworkReachabilityManager

- Seriaalization：网络通信信息序列化、反序列化模块 对应 AFURLResponseSerialization

- UIKit：对于iOS UIKit的扩展库

