![图片](xxxx.png)

> 每章一点正能量：每当你想要放弃的时候，就想想是为了什么才一路坚持到现在。


## 前言

本章主要向大家介绍下Java中一个常见的特性:`自动装箱与拆箱`，在我们开发过程中经常会使用一些基本数据类型或者是封装数据类型，但是对于他们之间的一些转换等特性可能不是特别清楚。也可能出现在我们的面试中。本章我们将从源码中解读其中自动装箱与拆箱的原理，以及会出现的一些陷阱已经性能等。如有错误还请大家及时指出~ 

>本文已同步至 [GitHub](https://github.com/CoderMerlin/coder-programming)/[Gitee](https://gitee.com/573059382/coder-programming)/公众号，感兴趣的同学帮忙点波关注~

问题：

- 基本数据类型与封装数据类型有哪些区别？
- 什么是装箱？什么是拆箱？
- 装箱和拆箱都是如何实现的？
- 其中会存在哪些问题？
- 

## 1.基础知识回顾

1. Java把内存划分成两种：一种是栈内存，另一种是堆内存。

2. int是基本类型，直接存数值；而 Integer是类，产生对象时用一个引用指向这个对象。

3. 封装类位于java.lang包中。

4. 封装类是引用传递而基本类型是值传递

5. 基本类型的变量和对象的引用变量都是在函数的栈内存中分配 ，而实际的对象是在存储堆内存中


### 1.1 基本数据类型和封装类型的区别

我们来看下他们之间有哪些区别：

[图片1]()

八种基本数据类型分别是：byte、char、boolean、int、short、float、double、long;
对应的封装类型分别是：Byte、Character、Boolean、Integer、Short、Float、Double、Long。

| 基本类型| 封装类型| 字节长度 | 默认值 |
|---|---|---|---|
|boolean|Boolean|1|false|
|byte|Byte|1|0|
|char|Character|2|u0000|
|short|Short|2|0|
|int|integer|4|0|
|long|Long|8|0l或0L|
|float|Float|4|0.0f或0.0F|
|double|Double|8|0.0|

5."=="和"equal（）"方法：

1）基本型和基本型封装型进行“==”运算符的比较，基本型封装型将会自动拆箱变为基本型后再进行比较，因此Integer(0)会自动拆箱为int类型再进行比较，显然返回true。

2）两个Integer类型进行“==”比较，如果其值在-128至127，那么返回true，否则返回false, 这跟Integer.valueOf()的缓冲对象有关，这里不进行赘述。

3）两个基本型的封装型进行equals()比较，首先equals()会比较类型，如果类型相同，则继续比较值，如果值也相同，返回true。

4）基本型封装类型调用equals(),但是参数是基本类型，这时候，先会进行自动装箱，基本型转换为其封装类型，再进行3中的比较。


例如：

```
//直接在栈中分配空间
int i = 2;

//对象是在堆内存中,而i(引用变量)是在栈内存中
Integer i = new Integr(2);

```




## 2.什么是装箱和拆箱

基本数据(Primitive)类型的自动装箱(autoboxing)、拆箱(unboxing)是自J2SE 5.0开始提供的功能。Java语言规范中说道：在许多情况下包装与解包装是由编译器自行完成的（在这种情况下包装称为装箱，解包装称为拆箱）。

### 2.1 自动装箱（Autoboxing）



例如：
```
//声明一个Integer对象
Integer num = 10;

```
以上的声明就是用到了自动的装箱,解析为:
Integer num = new Integer(10);
以上就是一个很好的体现，因为10是属于基本数据类型的，原则上它是不能直接赋值给一个对象Integer的，但jdk1.5后你就可以进行这样的声明，这就是自动装箱的魅力,自动将基本数据类型转化为对应的封装类型。成为一个对象以后就可以调用对象所声明的所有的方法
自动拆箱：故名思议就是将对象重新转化为基本数据类型：*/
 
wo men zhelici 
//装箱
Integer num_1 = 10;
//拆箱
int num_2 = num_1;
/*自动拆箱有个很典型的用法就是在进行运算的时候：因为对象时不能直接进行运算的，而是要转化为基本数据类型后才能进行加减乘除*/
 
Integer num_3 = 10;
//进行计算时隐含的有自动拆箱
System.out.print(num_3--);
/*哈哈 应该感觉很简单吧，下面我再来讲点稍微难点的.*/
 
//在-128~127 之外的数
Integer num_4 = 297; Integer num_5 = 297;
System.out.println("num_4==num_5: "+(num_4==num_5));
// 在-128~127 之内的数
Integer num_6 = 97; Integer num_7 = 97;
System.out.println("num_6==num_7: "+(num_6==num_7));
/*打印的结果是：
    num_4==num_5: false 
    num_6==num_7: true 
*/
//此处的解释在下方


基本数据类型的自动装箱(autoboxing)、拆箱(unboxing)是自J2SE 5.0开始提供的功能。 

一般我们要创建一个类的对象实例的时候，我们会这样：

```
Class a = new Class(parameter);
```


当我们创建一个Integer对象时，却可以这样：

Integer i = 100; (注意：不是 int i = 100; )

实际上，执行上面那句代码的时候，系统为我们执行了：Integer i = Integer.valueOf(100); （感谢@黑面馒头 和 @MayDayIT 的提醒）

此即基本数据类型的自动装箱功能。


## 3.案例分析




## 文末

本章节主要简单介绍了xxx 的相关知识，后续我们将会继续xxx。

>欢迎关注公众号：**Coder编程**
获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

参考文章：

https://blog.csdn.net/u013309870/article/details/70229983

https://blog.csdn.net/jairuschan/article/details/7513045

https://www.cnblogs.com/dolphin0520/p/3780005.html

https://blog.csdn.net/fanxiaobin577328725/article/details/52431508

https://www.jb51.net/article/123726.htm

https://www.cnblogs.com/jaysir/p/5399086.html


![微信公众号](https://user-gold-cdn.xitu.io/2019/4/16/16a26835c75c12fc?w=300&h=390&f=png&s=18217)

## 推荐阅读

xxxxx

xxxxx

xxxxx





a==b并不能判断a等于b，而是判断是否为同一个Object

如果我们要判断他们的值怎么做呢？用equal或者Objects.equals(JDK1.7之后新加 的语法)

Objects.equals有什么好处呢？

如果用a.equals(b) 如果a是null 的话，还会抛出空指针异常。

但是用Objects.equals就没有问题。

因此我们在使用引用类型的时候需要注意，当我们在赋值的时候，两个变量都是引用同一个Object



引用类型——装箱、拆箱



装箱

Integer a = 2; //Boxing

将2装在一个箱子里，这个箱子的类型是Integer 。箱子这里面装的数值就是2，我们就完成了一次装箱操作。并把a指向2这个箱子。



Integer b = new Integer(2);//Boxing

显示装箱。生成一个新的箱子 new Integer(); 并且这个箱子的值为2.而且让b指向这个箱子。



拆箱

int v = a.intValue(); //Unboxing

将里面int的值取出来。



面试中，经常出现的问题：

- new Integer(2) == 2?
- new Integer(2) == new Integer(2) ? 
- Integer.valueOf(2) == Integer.valueOf(2)?
- Integer.valueOf(2).intValue() == 2？
- new Integer(2).equals(new Integer(2))?

前两个之前介绍过，那么Integer.valueOf(2)是什么意思呢？

它会返回一个箱子给我们，箱子里面的值是2。但是在返回这个箱子给我们的时候，可能会新建一个新的箱子给我们，也可能会使用现有的一个箱子给我们。

所以Integer.valueOf(2) == Integer.valueOf(2)。什么情况下才会相等呢？只有当系统已经将2这个箱子建立好了，并且缓存起来的情况下。会把箱子的引用同时发给等号的左边与右边。这样的情况，他们才会互相相等。

接下来，我们在编译器上看看上面问题的结果。





输出结果：







问题一：new Integer(2) == 2?

等号的左边是一个Object右边是一个数值，Object和数值怎么会相等的呢？

答案：Java的编译器很聪明，它会自己去做装箱和拆箱的操作。这边它将new Integer(2)做的是Unboxing，它会里面的value取出来，这时候发现取出来的2等于右边，所以就为true。



问题二：new Integer(2) == new Integer(2) ? 

之前说过。new Integer(2) 就是新建一个箱子，这个箱子的值就是2。 == 是判断这两个箱子是不是同一个箱子，不是说里面的值是不是一样.所以是false。因为他们不是同一个箱子。



问题三：Integer.valueOf(2) == Integer.valueOf(2)?

Integer.valueOf() 是系统给我们分配的一个箱子，我们发现，每次调我们的箱子时候，系统都给了同一个箱子。这个我们的 Integer.valueOf(2) == Integer.valueOf(2)

点进源码：



在low和high之间，它会返回一个系统已经生产的cache，否则它会生产一个新的出来。看源码可以看到low = -128  high = 127



我们不用2改用1000试一试。



变为了false.说明系统对小的数字会使用系统分配的箱子，对于大的数字，系统会重新new一个箱子。



面试的时候，可以回答，他们可能相等，也可能不相等。是有系统决定的。



问题四：Integer.valueOf(2).intValue() == 2？

intValue()做了一个拆箱的操作，将里面的值2取出来，值2等于2，所以是true。



问题五：new Integer(2).equals(new Integer(2))?

这里我们没有用==而是用equals，equals判断相等是判断里面的值是不是相等，而不是判断这个箱子是不是同一个，所以我们的答案是true。我们来看看equals的源码。





判断里面的值是不是相等。
