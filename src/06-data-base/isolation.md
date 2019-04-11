https://www.cnblogs.com/fjdingsd/p/5273008.html
http://www.cnblogs.com/xdp-gacl/p/3984001.html
http://dev.mysql.com/doc/refman/5.6/en/set-transaction.html
https://blog.csdn.net/lipeng88888888/article/details/78523489
https://blog.csdn.net/qq_34337272/article/details/89177511
https://www.cnblogs.com/charlesblc/p/5913914.html
https://comedsh.iteye.com/blog/698733

## 前言

我们在上一章节中介绍过数据库的[带你了解数据库中事务的ACID特性 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483722&idx=1&sn=e8bc8bd82c559e0cfe7f35cf46100af3&chksm=96e67059a191f94fe8948e5b5e4ef177b77fa7707d86d945b153f67e7f2e76b83ed0c768ef27&token=1701970188&lang=zh_CN#rd)的相关用法。本章节主要来介绍下数据库中一个非常重要的知识点`事务的隔离级别`。如有错误还请大家及时指出~ 

问题：
- 事务的隔离级别有哪些？
- 如果并发事务没有进行隔离，会出现什么问题？

>以下都是采用mysql数据库

在多个事务并发做数据库操作的时候，如果没有有效的避免机制，就会出现种种问题。大体上有四种问题，归结如下。

## 引发的问题
在并发事务没有进行隔离的情况下，会发生如下问题。


### 问题一：脏读

`脏读`指一个事务读取了另外一个事务未提交的数据。

`具体看后文案例介绍`

### 问题二：不可重复读

`不可重复读`指在一个事务内读取表中的某一行数据，多次读取结果不同。
不可重复读和脏读的区别是，脏读是读取前一事务未提交的脏数据，不可重复读是重新读取了前一事务已提交的数据。

`具体看后文案例介绍`

### 问题三：幻读(虚读)

`幻读(虚读)`指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致。

`具体看后文案例介绍`

## 概念

### 事务的隔离级别分为：

- Read uncommitted(读未提交)
- Read Committed(读已提交)
- Repeatable Reads(可重复读)
- Serializable(串行化)

#### Read uncommitted

`读未提交`：隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是脏读（Dirty Read）。简单点说：别人事务做到一半还没提交，这时候被我们读到的值。
会引发脏读、不可重复读和幻读

#### Read Committed

读到的都是别人提交后的值。这种隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。
会引发不可重复读和幻读，但避免了脏读

#### Repeatable Reads
这种隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。
幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

会引发幻读，但避免了脏读、不可重复读

#### Serializable

是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行。

脏读、不可重复读、幻读都不会出现。

## 查看事务隔离级别

`SHOW VARIABLES LIKE 'tx_isolation';`

查看全局的事务隔离级别

`SHOW GLOBAL VARIABLES LIKE 'tx_isolation';`

使用系统变量查询

`SELECT @@global.tx_isolation;`
`SELECT @@session.tx_isolation;`
`SELECT @@tx_isolation;`

## 设置MysQL的事务隔离级别

### 语法
```
SET [GLOBAL | SESSION] TRANSACTION ISOLATION LEVEL
  {
       REPEATABLE READ
     | READ COMMITTED
     | READ UNCOMMITTED
     | SERIALIZABLE
   }
```
`GLOBAL`：设置全局的事务隔离级别
`SESSION`：设置当前session的事务隔离级别，如果语句没有指定GLOBAL或SESSION，默认值为SESSION

### 使用系统变量设置事务隔离级别

`SET GLOBAL tx_isolation='REPEATABLE-READ';`
`SET SESSION tx_isolation='SERIALIZABLE';`

## 案例分析

下面实际操作中使用到的一些并发控制语句:
`START TARNSACTION |BEGIN`： 显式地开启一个事务。
`COMMIT`：提交事务，使得对数据库做的所有修改成为永久性。
`ROLLBACK`： 回滚会结束用户的事务，并撤销正在进行的所有未提交的修改。
另：
mysql数据库默认的事务隔离级别是：Repeatable read(可重复读)
mysql数据库设置事务隔离级别：set transaction isolation level 隔离级别名


作为演示：product表

|productId | productName| productPrice| productCount|
|---|---|---|---|
|1 | xiaomi|1999|100|


带着上面的我们来看一下，事务在没有隔离性的情况下，会引发哪些问题？

同时打开两个窗口模拟2个用户并发访问数据库

### 1.事务隔离级别设置为read uncommitted
　　A窗口
　　　　set transaction isolation level  read uncommitted;--设置A用户的数据库隔离级别为Read uncommitted(读未提交)
　　　　start transaction;--开启事务
　　　　select * from account;--查询A账户中现有的钱，转到B窗口进行操作
　　　　select * from account--发现a多了100元，这时候A读到了B未提交的数据（脏读）

　　B窗口
　　　　start transaction;--开启事务
　　　　update account set money=money+100 where name='A';--不要提交，转到A窗口查询

   会引发脏读、不可重复读和幻读

### 2.事务隔离级别设置为Read Committed

　　A窗口
　　　　set transaction isolation level  read committed;
　　　　start transaction;
　　　　select * from account;--发现a帐户是1000元，转到b窗口
　　　　select * from account;--发现a帐户多了100,这时候，a读到了别的事务提交的数据，两次读取a帐户读到的是不同的结果（不可重复读）
　　B窗口
　　　　start transaction;
　　　　update account set money=money+100 where name='aaa';
　　　　commit;--转到a窗口
  会引发不可重复读和虚读，但避免了脏读

### 3.事务隔离级别设置为Repeatable Read

> mysql默认级别

　　A窗口
　　　　set transaction isolation level repeatable read;
　　　　start transaction;
　　　　select * from account;--发现表有4个记录，转到b窗口
　　　　select * from account;--可能发现表有5条记录，这时候发生了a读取到另外一个事务插入的数据（虚读）
　　B窗口
　　　　start transaction;
　　　　insert into account(name,money) values('ggg',1000);
　　　　commit;--转到a窗口
   会引发虚读，但避免了脏读、不可重复读
   
### 4.事务隔离级别设置为Serializable
　　A窗口
　　　　set transaction isolation level Serializable;
　　　　start transaction;
　　　　select * from account;--转到b窗口

　　B窗口
　　　　start transaction;
　　　　insert into account(name,money) values('ggg',1000);--发现不能插入，只能等待a结束事务才能插入

   会避免所有问题
   
查看MySQL的事务隔离级别


## 文末

>本章节主要介绍了数据库中事务的ADID特性中的`隔离性`，在没有隔离的情况下会发生什么问题，相信大家通过本章，对数据库事务中的`隔离性`有了一定的了解，下篇文章我们将介绍数据库中的`悲观锁与乐观锁`。
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
