
## 前言
> 欢迎关注公众号：**Coder编程**
> 获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

本章主要介绍数据库中Join的的用法，也是我们在使用数据库时非常基础的一个知识点。本次会介绍数据库中的`inner join`、`left join`、`right join` 的用法以及它们之间的区别。 文章如有错误还请大家及时指出~ 

>以下都是采用mysql数据库

## Join
相信大家在学习数据库的使用时，都有使用过Join，对数据库中的两张或两张以上表进行连接操作。  
**Join** 分为：
 - 内连接(inner join)
 - 外连接(outer join)
 
其中**外连接**分为：
- 左外连接(left outer join)
- 右外连接(right outer  join)
- 全外连接(full outer join)

>**说明：**
>>1.其中外连接的“OUTER”关键字可以省略不写。
>>2.内连接用于返回满足连接条件的记录；而外连接则是内连接的扩展，它不仅会满足连接条件的记录，而且还会返回不满足连接条件的记录。



## 笛卡尔积
>在我们进行多表联合查询的时候会出现的一种情况——**笛卡尔积现象**

我们以下面两张表举例：

**学生表（Student）**
|ID | StudentName | StudentClassID|
|---|---|---|
|1 | 小明| 1|
|2 | 小红| 2|
|3 | 小兰 | 3|
|4 | 小吕 | 2|
|5 | 小梓 | 1|

**班级表（Class）**
|ClassID| ClassName| 
|---|---|
|1 | 软件一班| 
|2 | 软件二班| 
|3 | 软件三班| 

当我们进行查询操作的时候：
> select * from Student,Class;

---
![查询](https://img-blog.csdnimg.cn/20190407222148953.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hhZWxfSE0=,size_16,color_FFFFFF,t_70)
就会出现上面的情况，也就是笛卡尔现象，表Student中有5条记录，表Class中有3条记录，那么对于表Student而言有5种选择，对于表Class来说有3种选择。所以一共有 5 * 3 = 15种选择了，也就是**笛卡尔积**。

---

## 内连接——inner join
  内连接查询返回满足条件的所有记录，默认情况下没有指定任何连接则为内连接。
  例如：查询xx学生在xx班级 `  沿用上面的数据表`

  >select stu.StudentName,cl.ClassName from Student stu `inner join` Class cl on stu.StudentClassID=cl.ClassID;  
  
  ---
  **查询结果**
![inner join](https://img-blog.csdnimg.cn/20190407224142330.png)


## 左外连接——left join 
左外连接查询不仅返回满足条件的所有记录，而且还会返回不满足连接条件的连接操作符左边表的其他行。
我们在原Student 表中新增学生：小美
![新增学生-小美](https://img-blog.csdnimg.cn/20190407225114143.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hhZWxfSE0=,size_16,color_FFFFFF,t_70)
例如：  查询xx学生在xx班级   `  沿用上面的数据表`
>select stu.StudentName,cl.ClassName from Student stu `left join` Class cl on stu.StudentClassID=cl.ClassID;  

---
  **查询结果**
  ![左外连接](https://img-blog.csdnimg.cn/20190407225326466.png)


## 右外连接——right join
右外连接查询不仅返回满足条件的所有记录，而且还会返回不满足连接条件的连接操作符右边表的其他行。
我们在原Class表中新增班级：软件四班

![新增班级_软件四班](https://img-blog.csdnimg.cn/20190407225710404.png)
例如：  查询xx学生在xx班级   `  沿用上面的数据表`
>select stu.StudentName,cl.ClassName from Student stu right join Class cl on stu.StudentClassID=cl.ClassID;  

---
  **查询结果**
  ![查询结果](https://img-blog.csdnimg.cn/20190407225819462.png)

## 全连接——full join
全连接查询不仅返回满足条件的所有记录，而且还会返回不满足连接条件的其他行。
>注：mysql默认不支持full join。

这里我们就不做介绍了。

---
## 文末

>本章节主要介绍了数据库中JOIN的用法，以及其他方面的小知识点。
欢迎关注公众号：**Coder编程**
获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190407231851378.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hhZWxfSE0=,size_16,color_FFFFFF,t_70)
## 推荐阅读

[一篇让你理解进程与线程的区别与联系](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483676&idx=1&sn=9033af3cb37754446779e1fcd89e3329&chksm=96e6700fa191f91919b4c2a46a8a99a7a7dda037181e97b5377835500e99a5f66cb1d3337898&token=424171447&lang=zh_CN#rd)

[通过“表白”的方式，让我们快速了解网络协议](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483701&idx=1&sn=b21d65f8ba4ae7f861a6e6175be2303c&chksm=96e67026a191f930c540a8c823c6ad5355dc4cb92824eadc9485aa195167768560dc506af358&token=1104592742&lang=zh_CN&scene=21#wechat_redirect)

[一篇带你读懂TCP之“滑动窗口”协议 ](https://mp.weixin.qq.com/s?__biz=MzIwMTg3NzYyOA==&mid=2247483706&idx=1&sn=8eed9d160013bd8ed6203ad511711553&chksm=96e67029a191f93fdd1543af2bf06025397d9c3bd0f0692c7fe247ab9c139cd869d69ab05498&token=1104592742&lang=zh_CN#rd)


参考文章：

https://www.cnblogs.com/jackson-zhangjiang/p/7819634.html

http://www.360doc.com/content/14/1229/21/7635_436727229.shtml


[Github个人主页目录](https://github.com/CoderMerlin/coder-programming)
[Gitee个人主页目录](https://gitee.com/573059382/coder-programming)

**欢迎大家关注并Star~**
