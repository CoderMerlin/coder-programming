
https://blog.csdn.net/qq_34337272/article/details/81072874#comments
https://blog.csdn.net/xiaokang123456kao/article/details/75268240
https://blog.csdn.net/claram/article/details/53959367

## 前言

我们在上一章节中介绍过数据库的[带你了解数据库中事务的ACID特性 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483722&idx=1&sn=e8bc8bd82c559e0cfe7f35cf46100af3&chksm=96e67059a191f94fe8948e5b5e4ef177b77fa7707d86d945b153f67e7f2e76b83ed0c768ef27&token=1701970188&lang=zh_CN#rd)与[带你了解数据库中事务隔离]()的相关用法。本章节主要来介绍下数据库中一个非常重要的知识点`事务的隔离级别`。如有错误还请大家及时指出~ 

问题：
- 介绍一下数据库中悲观锁与乐观锁？

>以下都是采用mysql数据库


定义：
悲观锁（Pessimistic Lock）： 
每次获取数据的时候，都会担心数据被修改，所以每次获取数据的时候都会进行加锁，确保在自己使用的过程中数据不会被别人修改，使用完成后进行数据解锁。由于数据进行加锁，期间对该数据进行读写的其他线程都会进行等待。

乐观锁（Optimistic Lock）： 
每次获取数据的时候，都不会担心数据被修改，所以每次获取数据的时候都不会进行加锁，但是在更新数据的时候需要判断该数据是否被别人修改过。如果数据被其他线程修改，则不进行数据更新，如果数据没有被其他线程修改，则进行数据更新。由于数据没有进行加锁，期间该数据可以被其他线程进行读写操作。

适用场景：
悲观锁：比较适合写入操作比较频繁的场景，如果出现大量的读取操作，每次读取的时候都会进行加锁，这样会增加大量的锁的开销，降低了系统的吞吐量。

乐观锁：比较适合读取操作比较频繁的场景，如果出现大量的写入操作，数据发生冲突的可能性就会增大，为了保证数据的一致性，应用层需要不断的重新获取数据，这样会增加大量的查询操作，降低了系统的吞吐量。

总结：两种所各有优缺点，读取频繁使用乐观锁，写入频繁使用悲观锁。






## 文末

>本章节主要简单介绍了数据库中事务的ADID特性，下一章我们将详细介绍事务`隔离`。
欢迎关注公众号：**Coder编程**
获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

![微信公众号](https://img-blog.csdnimg.cn/20190407231851378.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hhZWxfSE0=,size_16,color_FFFFFF,t_70)
## 推荐阅读
[带你了解数据库中JOIN的用法 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483713&idx=1&sn=d61ad0aed42dc36d64d17732db352288&chksm=96e67052a191f9445bbe3d5825ce547ad3171c3874b571a93b97977d0668413e37a164c3e0bc&token=1144933717&lang=zh_CN#rd)

[ 带你了解数据库中group by的用法 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483717&idx=1&sn=157a8a021c29043a10480d0294b39ca0&chksm=96e67056a191f940668812ebb092fe9984b22eb619a18339cc052e1051c659a7e9d907c48814&token=1144933717&lang=zh_CN#rd)



[一篇带你读懂TCP之“滑动窗口”协议 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483706&idx=1&sn=8eed9d160013bd8ed6203ad511711553&chksm=96e67029a191f93fdd1543af2bf06025397d9c3bd0f0692c7fe247ab9c139cd869d69ab05498&token=1104592742&lang=zh_CN#rd)




[Github个人主页目录](https://github.com/CoderMerlin/coder-programming)

[Gitee个人主页目录](https://gitee.com/573059382/coder-programming)

**欢迎大家关注并Star~**
