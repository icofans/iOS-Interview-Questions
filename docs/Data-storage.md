# 数据存储
## 1.iOS 开发中数据持久性有哪几种? 

iOS本地数据保存有多种方式,比如NSUserDefaults、归档、文件保存、数据库、CoreData、KeyChain(钥匙串)等多种方式。其中KeyChain(钥匙串)是保存到沙盒范围以外的地方，也就是与沙盒无关。

## 2.FMDB数据结构变化升级
	
- 1.使用columnExists:inTableWithName方法判断数据表中是否存在字段

- 2.如果不存在，则添加, 如：向bbb表中添加aaa字段 -> ALTER TABLE bbb ADD 'aaa' TEXT
