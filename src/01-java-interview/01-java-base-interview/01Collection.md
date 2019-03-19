## 前言
> 欢迎关注微信公众号：*Coder编程*
> 获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

本章主要介绍Collection集合相关知识，结合面试中会提到的相关问题进行知识点的梳理。希望能帮到大家~

**基于JDK1.8，如有错误，还望大家能够指出！**

涉及的Collection集合相关面试题
- [1.什么是集合？](#1.什么是集合？)
- [2.JAVA中集合类型都有哪些？各有什么特点？](#2.JAVA中集合类型都有哪些？各有什么特点？)
- [3.说一说集合的父类Collection？](#3.说一说集合的父类Collection？)
- [4.数组和集合都有哪些区别？](#4.数组和集合都有哪些区别？) 
- [5.说一说迭代器Iterator？](#5.说一说迭代器Iterator？)
- [6.Collection接口中几种重要的类和接口简介？](#6.Collection接口中几种重要的类和接口简介？)


### 1.什么是集合？

**来自百度百科的回答：**
    集合类存放于java.util包中。
    集合类存放的都是对象的引用，而非对象本身，出于表达上的便利，我们称集合中的对象就是指集合中对象的引用（reference)。
    集合类型主要有3种：set(集）、list(列表）和map(映射)。
    集合接口分为：Collection和Map，list、set实现了Collection接口



### 2.JAVA中集合类型都有哪些？各有什么特点？

Collection两大体系：链表List、集合Set
List特点：元素有序；元素可以重复；元素都有索引（角标）

List里存放的对象是有序的，同时也是可以重复的，List关注的是索引，拥有一系列和索引相关的方法，查询速度快。
因为往list集合里插入或删除数据时，会伴随着后面数据的移动，所有插入删除数据速度慢。

Set特点：元素无序；元素不可以重复；

Set里存放的对象是无序，不能重复的，集合中的对象不按特定的方式排序，只是简单地把对象加入集合中。

同时集合中还有另外一种类型：Map(映射)。

Map特点：键值对；键不可以重复；值可以重复；

Map集合中存储的是键值对，键不能重复，值可以重复。根据键得到值，对map集合遍历时先得到键的set集合，对set集合进行遍历，得到相应的值。



### 3.说一说集合的父类Collection？

#### 3.1 Collection 体系结构图

![](![](https://github.com/CoderMerlin/coder-programming/blob/master/src/01-java-interview/01-java-base-interview/images/01Collection.jpg?raw=true)
)

Collection
├List （有序集合，允许相同元素和null）
│├LinkedList （非同步，允许相同元素和null，遍历效率低插入和删除效率高）
│├ArrayList （非同步，允许相同元素和null，实现了动态大小的数组，遍历效率高，用的多）
│└Vector（同步，允许相同元素和null，效率低）
│　└Stack（继承自Vector，实现一个后进先出的堆栈）
└Set （无序集合，不允许相同元素，最多有一个null元素）
     |-HashSet(无序集合，不允许相同元素，最多有一个null元素)

Map （没有实现collection接口，key不能重复，value可以重复，一个key映射一个value）
├Hashtable （实现Map接口，同步，不允许null作为key和value，用自定义的类当作key的话要复写hashCode和eques方法，）
├HashMap （实现Map接口，非同步，允许null作为key和value，用的多）
└WeakHashMap（实现Map接口）

#### 3.2 Collection 中的主要方法

（1）添加
boolean add(E o);
boolean add(Collection<? extends E> c);

（2）删除
boolean remove(Object o);
boolean removeAll(Collection<? extends E> c)
void clear()；

（3）判断  
a.判断集合中是否有元素：boolean isEmpty();
b.判断集合中是否包含某个元素：boolean contains(Object o);
c.判断集合中是否包含某些元素：boolean contains(Collection<?> c);
        
（4）获取
a.获取集合中元素个数：int size();
b.遍历集合中所有元素：Iterator<E> iterator();
c.判断两个集合中是否存在相同的元素并保留两个集合中相同的元素删除不同的元素：boolean retainAll(Collection<?> c);

（5）其他
将集合中元素转为数组: 

a.    Ojbect[] toArray();
b.    <T>  T[] toArray();   泛型
Java8新增方法：
在 JDK 8 以后，Collection 接口还提供了从集合获取连续的或者并行流：
Stream<E> stream()
Stream<E> parallelStream()
于Collection接口相关还有一个抽象类AbstractCollection：
AbstractCollection是一个抽象类，实现了Collection接口的部分功能，实现了一些最基本的通用操作，把复杂的和业务相关的延迟到子类实现。
在AbstractCollection中，主要实现了contains(), isEmpty(), toArray(), remove(), clear() 这几个操作。有兴趣的同学可以自行研究下，逻辑都比较简单。
特别注意：List接口扩展了一个一些方法，其中最重要，也是用的最多的是：
E get(int index) 返回指定索引的元素


### 4.数组和集合都有哪些区别？

**数组特点：**

1.数组本质上就是一段连续的内存空间，用于记录多个类型相同的数据；
2.数据一旦声明完毕，则内存空间固定不变；
3.插入和删除操作不方便，可能会移动大量的元素并导致效率太低；
4.支持下标访问，可以实现随机访问；
5.数组中的元素可以是基本数据类型，也可以使用引用数据类型。

**集合特点：**

1.内存空间可以不连续，数据类型可以不相同；
2.集合的内存空间可以动态的调整；
3.集合的插入删除操作可以不移动大量元素；
4.部分支持下标访问，部分不支持；
5.集合中的元素必须是引用数据类型(你存储的是简单的int，它会自动装箱成Integer)；

可以看出数组和集合在数据的存储，访问，类型，长度等都有不同的地方。


### 5.说一说迭代器 Iterator？
 （1）通过集合对象获取其对应的Iterator对象
 （2）判断是否存在下一个元素
 （3）取出该元素并将迭代器对象指向下一个元素

 Iterator iterator():取出元素的方式：迭代器。
     该对象必须依赖于具体容器，因为每一个容器的数据结构都不同。
     所以该迭代器对象是在容器中进行内部实现的。
     对于使用容器者而言，具体的实现不重要，只要通过容器获取到该实现的迭代器的对象即可，也就是iterator方法。
**扩展知识：**
ArrayList里面的iterator方法采用了设计模式中的——工厂方法模式！
有兴趣的同学可以后续看到我另外的文章“设计模式精讲”专栏。

### 6.Collection接口中几种重要的类和接口简介？
该问题与第二个问题类似~ 后续文章会有更详细的介绍！
Collection两大体系：链表List、集合Set 另映射Map

List接口及子类介绍
　　List是有序的Collection，使用此接口能够精确的控制每个元素插入的位置。用户能够使用索引（元素在List中的位置，类似于数组下标）来访问List中的元素，
这类似于Java的数组。 和下面要提到的Set不同，List允许有相同的元素。 除了具有Collection接口必备的iterator()方法外，List还提供一个listIterator()方法，返回一个ListIterator接口，和标准的Iterator接口相比，
ListIterator多了一些add()之类的方法，允许添加，删除，设定元素，还能向前或向后遍历。 
　　实现List接口的常用类有：
- LinkedList类  （底层数据结构是链表，线程不安全）
- ArrayList类 （底层数据结构是数组，线程不安全）
- Vector类 　（底层数据结构是数组，线程安全）
- Stack类   （顾名思义：栈，继承至Vector类并进行扩展）
在后续的文章中我们将一一详细介绍这些类的相关特性！

Set接口及子类介绍
Set是一种不包含重复的元素的Collection，即任意的两个元素e1和e2都有e1.equals(e2)=false，Set最多有一个null元素。很明显，Set的构造函数有一个约束条件，传入的Collection参数不能包含重复的元素。 　　
注意：必须小心操作可变对象（Mutable Object）。如果一个Set中的可变元素改变了自身状态导致Object.equals(Object)=true将导致一些问题。
实现Set接口的常用类有：
- HashSet类 （底层数据结构是数组+单链表+红黑树，无序）
- LinkedHashSet （底层数据结构是数组+单链表+红黑树+双向链表，无序）
- TreeSet类 （底层数据结构红黑树，有序）
在后续的文章中我们将一一详细介绍这些类的相关特性！

Map接口及子类介绍　　
注意：Map没有继承Collection接口，Map提供key到value的映射。一个Map中不能包含相同的key，每个key只能映射一个value。
Map接口提供3种集合的视图，Map的内容可以被当作一组key集合，一组value集合，或者一组key-value映射。
- Hashtable类 
- HashMap类 　　
在后续的文章中我们将一一详细介绍这些类的相关特性！

## 文末
本章节介绍了Collection接口中的大部分可能在面试过程中会出现的内容，
并没有详细去介绍其子类及其实现相关的原理。这方面的内容会放在后续的章节中去详细介绍。

> 欢迎关注微信公众号：*Coder编程*
> 获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！
