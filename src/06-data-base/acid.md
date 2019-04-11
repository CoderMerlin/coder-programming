## 前言

前面我们介绍过数据库中 [带你了解数据库中JOIN的用法 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483713&idx=1&sn=d61ad0aed42dc36d64d17732db352288&chksm=96e67052a191f9445bbe3d5825ce547ad3171c3874b571a93b97977d0668413e37a164c3e0bc&token=1144933717&lang=zh_CN#rd) 与 [ 带你了解数据库中group by的用法 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483717&idx=1&sn=157a8a021c29043a10480d0294b39ca0&chksm=96e67056a191f940668812ebb092fe9984b22eb619a18339cc052e1051c659a7e9d907c48814&token=1144933717&lang=zh_CN#rd)的相关用法。本章节主要来介绍下数据库中一个非常重要的知识点`事务`，也是我们项目中或面试中经常会遇到的知识点。如有错误还请大家及时指出~ 

涉及面试题：
- 请讲下你对事务的理解？
- 讲下事务有那几大特性？


>以下都是采用mysql数据库

## 事务

事务(Transaction)，是我们关系型数据库中非常重要的一个概念，它要符合ACID特性。是由一组SQL语句组成的一个程序执行单元(Unit)，该执行单元要么成功Commit,要么失败Rollback。

![事务](https://img-blog.csdnimg.cn/20190409223141756.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hhZWxfSE0=,size_16,color_FFFFFF,t_70)
## ACID特性

- Atomicity
- Consistency
- Isolation
- Durability


### Atomicity(原子性)
`原子性`：指事务是一个不可再分割的工作单元，事务中的操作要么都发生，要么都不发生。
通俗的说：我们有一堆的事情，它要么全做，要么全都不做，不能只做一半。比如我们的银行转账。我把钱转给你，把我的钱扣掉，然后把你的钱加上去。不能只做一半，只把我的钱扣掉，你的钱没有加上去。

![原子性](https://img-blog.csdnimg.cn/20190409223612544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hhZWxfSE0=,size_16,color_FFFFFF,t_70)
### Consistency(一致性)

`一致性`指事务开始之前和事务结束以后，数据库的完整性约束没有被破坏。
通俗的说：我和你的钱加起来一共是2000，那么不管我和你之间如何转账，转几次账，事务结束后我们的钱相加起来应该还得是2000，这就是事务的一致性。
![一致性](https://img-blog.csdnimg.cn/20190409223903482.png)


### Isolation(隔离性)

`隔离性`指多个事务并发访问时，事务之间是隔离的，一个事务不应该影响其它事务运行效果。
通俗的说：多个用户并发访问操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。

数据库定义了4个隔离的级别：

- READ_UNCOMMITTED
- READ_COMMITTED
- REPEATABLE_READ
- SERIALIZABLE

>下篇文章我们将专门介绍事务的隔离性


### Durability(持久性)

`持久性`指事务所对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。
通俗的说：比如我将事务做完之后，这个结果是能持久下去的并能一直存下去。不管断电还是其他情况。
![持久性](https://img-blog.csdnimg.cn/20190409224425827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hhZWxfSE0=,size_16,color_FFFFFF,t_70)

>关系型数据库都实现了ACID这样的一些事务特性。其中最关键的一点是Isolation(隔离性)，互相不影响。


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
