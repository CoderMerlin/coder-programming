

## 前言

本章主要介绍数据库中`group by`的用法，也是我们在使用数据库时非常基础的一个知识点。并且也会涉及Join的使用，关于Join的用法，可以看我写的上一篇文章：[带你了解数据库中JOIN的用法 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483713&idx=1&sn=d61ad0aed42dc36d64d17732db352288&chksm=96e67052a191f9445bbe3d5825ce547ad3171c3874b571a93b97977d0668413e37a164c3e0bc&token=1144933717&lang=zh_CN#rd)如有错误还请大家及时指出~ 

>以下都是采用mysql数据库

## Group By
### 概念
Group By语句从英文的字面意义上理解就是“根据(by)一定的规则进行分组(Group)”。
作用：通过一定的规则将一个数据集划分成若干个小的区域，然后针对若干个小区域进行数据处理。
>注意：group by 是先排序后分组！

### 语法
```
SELECT expression1, expression2, ... expression_n, 
       aggregate_function (aggregate_expression)
FROM tables
[WHERE conditions]
GROUP BY expression1, expression2, ... expression_n
[ORDER BY expression [ ASC | DESC ]];
```
#### 语法说明
`expression1，expression2，... expression_n`
表达式未封装在聚合函数中，必须包含在SQL语句末尾的GROUP BY子句中。

`aggregate_function`
这是一个聚合函数，例如SUM，COUNT，MIN，MAX或AVG函数。

`aggregate_expression`
这是将使用aggregate_function的列或表达式。

`tables`
您希望从中检索记录的表。FROM子句中必须至少列出一个表。

`where`
可选的。这些是要选择的记录必须满足的条件。

`ORDER BY`表达式
可选的。用于对结果集中的记录进行排序的表达式。如果提供了多个表达式，则值应以逗号分隔。

`ASC`
可选的。ASC按表达式按升序对结果集进行排序。如果没有修饰符是提供者，则这是默认行为。

`DESC`
可选的。DESC按表达式按降序对结果集进行排序。

### 常用聚合函数
- count() 计数
- sum() 求和
- avg() 平均数
- max() 最大值
- min() 最小值



### 举例
我们以下面两张表举例：

**学生表（Student）**
|ID | StudentName | StudentClassID|
|---|---|---|
|1 | 小明  |1|
|2 | 小红|2|
|3 | 小兰 | 3|
|4 | 小吕|2|
|5 | 小梓|1|

**班级表（Class）**
|ClassID| ClassName| 
|---|---|
|1 | 软件一班| 
|2 | 软件二班| 
|3 | 软件三班| 


- 计算每个班都有多少学生？

> SELECT cl.ClassName,COUNT(stu.StudentClassID) AS studentNum
FROM student stu LEFT JOIN class cl ON stu.StudentClassID = cl.ClassID
GROUP BY cl.ClassName；

---

**计算结果：**
|ClassName| StudentName | 
|---|---|
|软件一班| 2| 
|软件二班| 2| 
|软件三班| 1| 




## HAVING
- HAVING语句通常与GROUP BY语句联合使用，用来过滤由GROUP BY语句返回的记录集。
- HAVING语句的存在弥补了WHERE关键字不能与聚合函数联合使用的不足。
通俗的说：WHERE过滤行，HAVING过滤组

### 语法
```
SELECT expression1, expression2, ... expression_n, 
       aggregate_function (aggregate_expression)
FROM tables
[WHERE conditions]
GROUP BY expression1, expression2, ... expression_n
```
### 语法说明
`HAVING condition`
这是另一个条件，仅应用于聚合结果以限制返回行的组。只有那些条件评估为TRUE的组才会包含在结果集中。

### 举例
`采用上面两张数据表`

- 查询学生人数大于2人的班级？

>SELECT cl.ClassName,COUNT(stu.StudentClassID) AS studentNum
FROM student stu LEFT JOIN class cl ON stu.StudentClassID = cl.ClassID
GROUP BY cl.ClassName 
HAVING COUNT(stu.StudentClassID)>=2;

---

**计算结果：**
|ClassName| StudentName | 
|---|---|
|软件一班| 2| 
|软件二班| 2| 
## 小结
- 当group by 与聚合函数配合使用时，功能为分组后计算
- 当group by 与 having配合使用时，功能为分组后过滤，获得满足条件的分组的返回结果。
- having与where区别：where过滤行，having过滤组

---
## 文末

>本章节主要简单介绍了数据库中group by的用法，并没有详细的展开讲解，相信大家只要把基础打扎实，再复杂的查询也可以通过`分而治之`的思想来解决。
欢迎关注公众号：**Coder编程**
获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

![微信公众号](https://img-blog.csdnimg.cn/20190407231851378.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hhZWxfSE0=,size_16,color_FFFFFF,t_70)
## 推荐阅读
[带你了解数据库中JOIN的用法 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483713&idx=1&sn=d61ad0aed42dc36d64d17732db352288&chksm=96e67052a191f9445bbe3d5825ce547ad3171c3874b571a93b97977d0668413e37a164c3e0bc&token=1144933717&lang=zh_CN#rd)


[一篇让你理解进程与线程的区别与联系](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483676&idx=1&sn=9033af3cb37754446779e1fcd89e3329&chksm=96e6700fa191f91919b4c2a46a8a99a7a7dda037181e97b5377835500e99a5f66cb1d3337898&token=424171447&lang=zh_CN#rd)


[一篇带你读懂TCP之“滑动窗口”协议 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483706&idx=1&sn=8eed9d160013bd8ed6203ad511711553&chksm=96e67029a191f93fdd1543af2bf06025397d9c3bd0f0692c7fe247ab9c139cd869d69ab05498&token=1104592742&lang=zh_CN#rd)


参考文章：

https://www.techonthenet.com/sql/having.php

https://www.techonthenet.com/sql/group_by.php

https://www.cnblogs.com/8335IT/p/5850531.html


[Github个人主页目录](https://github.com/CoderMerlin/coder-programming)

[Gitee个人主页目录](https://gitee.com/573059382/coder-programming)

**欢迎大家关注并Star~**
