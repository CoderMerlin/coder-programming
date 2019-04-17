
![悲观锁与乐观锁](https://user-gold-cdn.xitu.io/2019/4/16/16a26835c73ea038?w=900&h=383&f=png&s=178029)
## 前言
在上一个章节[5分钟带你读懂事务隔离性与隔离级别 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483728&idx=1&sn=cdc5dc4708e48051e56b8e7d2a9fe5a8&chksm=96e67043a191f955b93e7228b88572beb486e6fac3308a1b69f5ee83c9e9ced6957e30b12d58&token=1787261466&lang=zh_CN#rd)的最后，其实我们已经提到了锁的概念。本章节接下来将主要介绍以下数据库`悲观锁与乐观锁`的相关知识。如有错误还请大家及时指出~ 

>本文已同步至 [GitHub](https://github.com/CoderMerlin/coder-programming)/[Gitee](https://gitee.com/573059382/coder-programming)/公众号，感兴趣的同学帮忙点波关注~

问题：

- 为什么需要锁？
- 什么是悲观锁？
- 什么是乐观锁？
- 悲观锁与乐观锁区别与联系？
- 悲观锁与乐观锁的使用场景？


## 为什么需要锁？
在并发环境下，如果多个客户端访问同一条数据，此时就会产生数据不一致的问题，如何解决，通过加锁的机制，常见的有两种锁，乐观锁和悲观锁，可以在一定程度上解决并发访问。



## 1. 悲观锁（Pessimistic Lock）

### 1.1 定义

**百度百科：** 

悲观锁，正如其名，具有强烈的独占和排他特性。它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度，因此，在整个数据处理过程中，将数据处于锁定状态。悲观锁的实现，往往依靠数据库提供的锁机制（也只有数据库层提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）。

**其他知识点**

悲观锁主要是`共享锁`或`排他锁`
`共享锁`又称为读锁，简称S锁，顾名思义，共享锁就是多个事务对于同一数据可以共享一把锁，都能访问到数据，但是只能读不能修改。
`排他锁`又称为写锁，简称X锁，顾名思义，排他锁就是不能与其他所并存，如一个事务获取了一个数据行的排他锁，其他事务就不能再获取该行的其他锁，包括共享锁和排他锁，但是获取排他锁的事务是可以对数据就行读取和修改。


###  1.2 案例分析

>使用场景举例：以MySQL InnoDB为例

作为演示，我们继续使用之前的数据库表：product表

|productId | productName| productPrice| productCount|
|---|---|---|---|
|1 | 小米 |1999|100|
|2| 魅族 |1999|100|

首先我们需要`set autocommit=0`，即不允许自动提交

有看过上一篇文章[5分钟带你读懂事务隔离性与隔离级别 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483728&idx=1&sn=cdc5dc4708e48051e56b8e7d2a9fe5a8&chksm=96e67043a191f955b93e7228b88572beb486e6fac3308a1b69f5ee83c9e9ced6957e30b12d58&token=1787261466&lang=zh_CN#rd)的同学，可以看到最后我们使用事务隔离级别时，所引申出来的根本问题就是可以通过锁机制解决。

#### 问题
在并发情况下回导致数据一致性的问题：
如果有A、B两个用户需要抢productId =1的小米手机，A、B用户都查询小米手机数量是100，A购买后修改商品的数量为99，B购买后修改数量为99。

####  用法
每次获取小米手机时，对该商品加排他锁。也就是在用户A获取获取 id=1 的小米手机信息时对该行记录加锁，期间其他用户阻塞等待访问该记录。代码如下：

```
start transaction；
 
select p.productCount from product p where p.productId = 1 for update;
 
update product p set p.productCount=p.productCount-1 where p.productId=1 ;
 
commit;

```


**操作**

下面同时打开两个窗口模拟2个用户并发访问数据库

|时间轴| 事务A| 事务B|
|---|---|---|
|T1 | start transaction;|  |
|T2| select p.productCount from product p where p.productId = 1 for update;|  |
|T3| | start transaction; |
|T4| | select p.productCount from product p where p.productId = 1 for update;(等待中...) |

**流程说明**

1. 用户A start transaction开启一个事物。前一步我们关闭了mysql的autocommit，所以需要手动控制事务的提交。 
2. 在获得小米手机信息（productId = 1 ）时，进行数据加锁操作（for update）。与普通查询方式不同，我们使用了`select…for update`的方式，这样就通过数据库实现了悲观锁。在这个update事务提交之前其他外界是不能修改这条数据的，但是这种处理方式效率比较低，一般不推荐使用。 
3. 用户B start transaction开启一个事物。
4. 用户B 也进行查询操作，此时处于等待中（阻塞状态）。ps:需要等待用户A事务提交后，才会执行。


> 注意：在事务中，只有select…for update(排他锁) 或lock in share mode(共享锁) 操作同一个数据时才会等待其它事务结束后才执行，一般select... 则不受此影响。例如在 T3中执行select p.productCount from product p where p.productId = 1;则能正常查询出数据，不会受第一个事务的影响。


## 2. 乐观锁（Optimistic Lock）

### 2.1 定义

**百度百科：** 

乐观锁机制采取了更加宽松的加锁机制。乐观锁是相对悲观锁而言，也是为了避免数据库幻读、业务处理时间过长等原因引起数据处理错误的一种机制，但乐观锁不会刻意使用数据库本身的锁机制，而是依据数据本身来保证数据的正确性。


**其他知识点**

实现乐观锁一般来说有以下2种方式：

- 使用版本号
  使用数据版本（Version）记录机制实现，这是乐观锁最常用的一种实现方式。何谓数据版本？即为数据增加一个版本标识，一般是通过为数据库表增加一个数字类型的 “version” 字段来实现。当读取数据时，将version字段的值一同读出，数据每更新一次，对此version值加一。当我们提交更新的时候，判断数据库表对应记录的当前版本信息与第一次取出来的version值进行比对，如果数据库表当前版本号与第一次取出来的version值相等，则予以更新，否则认为是过期数据。
    
- 使用时间戳
  乐观锁定的第二种实现方式和第一种差不多，同样是在需要乐观锁控制的table中增加一个字段，名称无所谓，字段类型使用时间戳（timestamp）, 和上面的version类似，也是在更新提交的时候检查当前数据库中数据的时间戳和自己更新前取到的时间戳进行对比，如果一致则OK，否则就是版本冲突。


###  2.2 案例分析

>使用场景举例：以MySQL InnoDB为例

作为演示，我们继续使用之前的数据库表：product表

|productId | productName| productPrice| productCount| version|
|---|---|---|---|---|
|1 | 小米 |1999|100|1|
|2| 魅族 |1999|100|2|

我们以`版本号`实现的方式进行说明。

**操作**

查询当前事务隔离级别：

```

SELECT @@tx_isolation;


结果：
REPEATABLE-READ

```


下面同时打开两个窗口模拟2个用户并发访问数据库

**第一种测试**

|时间轴| 用户A| 用户B|
|---|---|---|
|T1 | start transaction;|  |
|T2| select * from product p where p.productId = 1;(productCount=100)|  |
|T3|update product p set p.productCount = 99,version=version+1 where p.productId = 1 and version = 1;(受影响的行: 1) | |
|T4| | start transaction; |
|T5| | select * from product p where p.productId = 1;(productCount=100) |
|T6| | update product p set p.productCount = 99,version=version+1 where p.productId = 1 and version = 1;(等待中...) |
|T7|commit; |  |
|T8| | T6执行(受影响的行: 0) |
|T9| |commit;  |


**流程说明**
1. 事务A开启事务。
2. 事务A查询当前小米手机数量为100。
3. 事务A购买小米手机，小米手机数量更新为99。(此时并未提交事务)。
4. 事务B开启事务。
5. 事务B查询当前小米手机数量为100。
6. 事务B购买小米手机，小米手机数量更新为99。注意：此时处于阻塞状态。
7. 事务A提交事务。
8. 此时第六步执行完毕，但并未成功(受影响的行: 0)。
9. 事务B提交事务。

**第二种测试**

|时间轴| 用户A| 用户B|
|---|---|---|
|T1 | select * from product p where p.productId = 1;(productCount=100)|  |
|T2| update product p set p.productCount = 99,version=version+1 where p.productId = 1 and version = 1;(受影响的行: 1)|  |
|T3| | select * from product p where p.productId = 1;(productCount=100) |
|T4| | update product p set p.productCount = 99,version=version+1 where p.productId = 1 and version = 1;(受影响的行: 0) |

**乐观锁小结**

- 用户B修改数据的时候，受影响行数为0，对业务来说，及更新失败。这时候我们只需要告诉用户购买失败，重新查询一遍即可。
- 对比第一种和第二种测试，我们会发现第一种测试，将update语句放入事务中会出现阻塞的情况，而第二种测试不会出现阻塞情况。这是为什么呢？update其实在不在事务中都无所谓，在内部是这样的：update是单线程的，及如果一个线程对一条数据进行update操作，会获得锁，其他线程如果要对同一条数据操作会阻塞，直到这个线程update成功后释放锁。

> 乐观锁不需要数据库底层的支持！



## 3. 适用场景

**悲观锁**

比较适合写入操作比较频繁的场景，如果出现大量的读取操作，每次读取的时候都会进行加锁，这样会增加大量的锁的开销，降低了系统的吞吐量。

**乐观锁**

比较适合读取操作比较频繁的场景，如果出现大量的写入操作，数据发生冲突的可能性就会增大，为了保证数据的一致性，应用层需要不断的重新获取数据，这样会增加大量的查询操作，降低了系统的吞吐量。



## 文末

本章节主要简单介绍了数据库中`乐观锁与悲观锁`的相关知识，后续我们将会继续介绍数据库中的其他锁以及相关知识。例如行锁、表锁、死锁、

>欢迎关注公众号：**Coder编程**
获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

参考文章：

https://chenzhou123520.iteye.com/blog/1860954

https://chenzhou123520.iteye.com/blog/1863407


![微信公众号](https://user-gold-cdn.xitu.io/2019/4/16/16a26835c75c12fc?w=300&h=390&f=png&s=18217)

## 推荐阅读

[带你了解数据库中JOIN的用法 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483713&idx=1&sn=d61ad0aed42dc36d64d17732db352288&chksm=96e67052a191f9445bbe3d5825ce547ad3171c3874b571a93b97977d0668413e37a164c3e0bc&token=1144933717&lang=zh_CN#rd)

[ 带你了解数据库中事务的ACID特性 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483722&idx=1&sn=e8bc8bd82c559e0cfe7f35cf46100af3&chksm=96e67059a191f94fe8948e5b5e4ef177b77fa7707d86d945b153f67e7f2e76b83ed0c768ef27&token=128531458&lang=zh_CN#rd)

[5分钟带你读懂事务隔离性与隔离级别 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483728&idx=1&sn=cdc5dc4708e48051e56b8e7d2a9fe5a8&chksm=96e67043a191f955b93e7228b88572beb486e6fac3308a1b69f5ee83c9e9ced6957e30b12d58&token=1787261466&lang=zh_CN#rd)