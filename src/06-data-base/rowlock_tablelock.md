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