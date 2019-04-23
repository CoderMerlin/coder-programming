
![图片](xxxx.png)
## 前言
在上一个章节xxx,本章节接下来将主要介绍xxxx的相关知识。如有错误还请大家及时指出~ 

>本文已同步至 [GitHub](https://github.com/CoderMerlin/coder-programming)/[Gitee](https://gitee.com/573059382/coder-programming)/公众号，感兴趣的同学帮忙点波关注~

问题：

- xxxxx？
- xxxxx？


## 正文


## 文末

本章节主要简单介绍了xxx 的相关知识，后续我们将会继续xxx。

>欢迎关注公众号：**Coder编程**
获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

参考文章：
https://blog.csdn.net/why15732625998/article/details/80439315

https://blog.csdn.net/zgcr654321/article/details/82345087

https://blog.csdn.net/qq_36071795/article/details/83869042

https://blog.csdn.net/xts5701046/article/details/81395958



![微信公众号](https://user-gold-cdn.xitu.io/2019/4/16/16a26835c75c12fc?w=300&h=390&f=png&s=18217)

## 推荐阅读

xxxxx

xxxxx

xxxxx


####  行锁（Row Lock）与表锁（Table Lock）

使用select…for update会把数据给锁住，这是通过数据库实现的悲观锁。接下来我们来介绍下行锁与表锁。

##### 行锁

作为演示，我们继续使用之前的数据库表：product表

|productId | productName| productPrice| productCount|
|---|---|---|---|
|1 | 小米 |1999|100|
|2| 魅族 |1999|100|

1. 只根据主键进行查询，并且查询到数据，主键字段产生行锁。

**操作**

下面同时打开两个窗口模拟2个用户并发访问数据库

|时间轴| 事务A| 事务B|
|---|---|---|
|T1 | start transaction;|  |
|T2| select p.productCount from product p where p.productId = 1 for update;(productName = 小米)|  |
|T3 | | start transaction; |
|T4 | | select p.productCount from product p where p.productId = 2 for update;(productName = 魅族) |
|T5 | | select p.productCount from product p where p.productId = 1 for update;(等待中...)|

**说明**
productId是主键，当在事务A上查询productId=1的数据时候，在事务B上查询productId=2的数据没问题，但在事务B上查询productId=1的数据时阻塞，说明此时的锁时行锁。当事务A执行commit时，事务B查询的productId=1的命令立即返回数据。

2. 根据非主键含索引（productName）进行查询，并且查询到数据，productName字段产生行锁，将整行锁住，其他条件查询该数据阻塞。

**操作**

下面同时打开两个窗口模拟2个用户并发访问数据库

|时间轴| 事务A| 事务B|
|---|---|---|
|T1 | start transaction;|  |
|T2| select p.productCount from product p where p.productId = 1 for update;(productName = 小米)|  |
|T3 | | start transaction; |
|T4 | | select p.productCount from product p where p.productId = 2 for update;(productName = 魅族) |
|T5 | | select p.productCount from product p where p.productId = 1 for update;(等待中...)|




##### 表锁