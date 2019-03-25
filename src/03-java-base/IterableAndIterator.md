## 前言
> 欢迎关注公众号：**Coder编程**
> 获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

在上一篇文章[通过面试题，让我们来了解Collection]()，我们会发现Collection接口之上还有一个接口**Iterable**，
Iterable接口里面又有Iterator接口，那他们到底有什么区别呢？我们接下来就来了解下Iterable与Iterator相关内容，也就是本章的主要内容了，说不定
在我们面试过程中，也会遇到一些问题呢。那我们就开始吧~

涉及面试题：
- 1.说一说Iterable？
- 2.Iterable结构？
- 3.说一说Iterator？
- 4.Iterator结构？
- 5.forEachRemaining()与forEach()方法的区别？
- 6.Iterator如何使用？
- 7.Iterable与Iterator之间的区别与联系？


>上面的面试题可以看出，其实都是一回事，只是换了一种提问方式，只要我们能掌握核心要点，随便面试官怎么提问，我们都能轻松应对！

> **来源百度百科：**

**Iterable:** 百度的时候，我却只看到了百度翻译：可迭代的; 可重复的; 迭代的; 因此我们可以知道，实现了这个接口的集合对象支持迭代，是可迭代的。
**Iterator:** Iterator我们一般叫迭代器，它就是提供迭代机制的对象，具体如何迭代，都是Iterator接口规范的。通过UML图，我们也可以看出Iterable接口是Java集合框架的顶级接口,
实现此接口使集合对象可以通过迭代器遍历自身元素。同时在Java设计模式中也有一种模式——**迭代器模式**.(在后续的文章我们会介绍相关设计模式,敬请关注~)
可以看出Iterator是迭代器模式最好的一个应用例子！

[CollectionUML图](images/Collection.png)

## 1.说一说Iterable？

[Iterable方法图](images/Iterable方法.png)

由源码图可以看出，Iterable有三个方法，分别是
 - Iterator<T> iterator();
 - default void forEach(Consumer<? super T> action){}; **JDK 1.8后新增**
 - default Spliterator<T> spliterator(){}; **JDK 1.8后新增**

接下来我们简单介绍下这里面的方法。

### 1.1 Iterable接口中的iterator() 方法

```
Iterator<T> iterator();

```

该接口主要是返回T类型的元素上的一个迭代器。下面再详细介绍Iterator。

### 1.2 Iterable接口中的forEach() 方法

```
default void forEach(Consumer<? super T> action) {
    	// 验证action是否为null，如果action为null，则抛出NullPointerException
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }
```
该方法是循环输出，对内部元素进行遍历,并对元素进行指定的操作。例如：

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
 numbers.forEach(integer -> System.out.println(integer));
```
**对源码注释翻译：**
对这个Iterable的每一个元素执行给定的动作指导所有元素都被处理或者动作抛出一个异常
为止。除非被实现类指定，动作将以迭代的顺序执行（如果一个迭代的顺序被指定。）被动作
抛出的异常将被传递给调用者



### 1.3 Iterable接口中的spliterator() 方法


```
default Spliterator<T> spliterator() {
    return Spliterators.spliteratorUnknownSize(iterator(), 0);
}
```
该方法提供了一个可以并行遍历元素的迭代器,以适应现在cpu多核时代并行遍历的需求。简单说：分割，增加并行处理能力
**对源码注释翻译：**
创建一个被这个Iterable接口描述的元素上Spliterator。默认实现从iterable的Iterator中创建一个早期绑定的spliterator。这个spliterator
继承iterable的iterator的fail-fast性质。 
默认实现应该总是被重写。被默认实现返回的spliterator拥有不好分解能力，是不依据指定
大小定制的，而且不报告任何spliterator的性质。实现类差不多总是能提供更好的实现。



## 2.说一说Iterator？

### 2.1 Iterator是什么？
Iterator被称之为顺序遍历迭代器，jdk中默认对集合框架中数据结构做了实现。
Iterator在实际应用中有一个比较好的点就是，可以一边遍历一遍删除元素。后面我会通过ArrayList中的Iterator()方法进行说明。

### 2.2 Iterator结构？
[Iterable方法图](images/Iterator方法.png)

由源码图Iterator接口中定义了四个方法，分别是

- boolean hasNext()：如果被迭代遍历的集合还没有被遍历完，返回True
- Object next()：返回集合里面的下一个元素
- remove()：删除集合里面上一次next()方法返回的元素
- void forEachRemaining(Consumer action)：**JDK 1.8后新增默认方法** 使用Lambda表达式来遍历集合元素

### 2.3 Iterator接口中的forEachRemaining() 方法

在JDK1.8之后Iterator中增加的一个默认方法

```
//使用方法
List<String> arr=new ArrayList<>();
arr.add("hello");
arr.add(("world"));
arr.iterator().forEachRemaining(str-> System.out.println(str));
```

#### 2.3.1 forEachRemaining()与forEach()方法之间的区别？

**forEachRemaining()源码：**

```
default void forEachRemaining(Consumer<? super E> action) {
    Objects.requireNonNull(action);
    while (hasNext())
        action.accept(next());
}
```

**forEach()源码：**

```
default void forEach(Consumer<? super T> action) {
    Objects.requireNonNull(action);
    for (T t : this) {
        action.accept(t);
    }
}
```

通过源码，我们可以看出他们之间的区别与联系。
**相同点：**
- 都可以遍历集合
- 都是接口的默认方法
- 都是1.8版本引入的

**区别：**
- forEachRemaining()方法内部是通过使用迭代器Iterator的所有元素，forEach()方法内部使用的是增强for循环。
- forEach()方法可以多次调用，forEachRemaining()方法第二次调用不会做任何操作，因为不会有下一个元素。

## 3.Iterator如何使用？
### 简单举个小栗子

```
List list = new ArrayList();
list.add("公众号");
list.add("Coder编程");
for (Iterator iter = l.iterator(); iter.hasNext();) {
String str = (String)iter.next();
System.out.println(str);
}
/*迭代器用于while循环
Iterator iter = l.iterator();
while(iter.hasNext()){
String str = (String) iter.next();
System.out.println(str);
}
*/
```

## 推荐阅读
[带你了解Collection相关知识！](03-java-base/Collection.md)
[一篇让你理解进程与线程的区别与联系！](03-java-base/ProcessAndThread.md)

## 文末

>本章节主要介绍了Iterable与Iterator之间的区别与联系，以及其他方面的小知识点，也是面试过程中会出现的内容点。
欢迎关注公众号：**Coder编程**
获取最新原创技术文章和相关免费学习资料，随时随地学习技术知识！

[Github个人主页目录](https://github.com/CoderMerlin/coder-programming)
[Gitee个人主页目录](https://gitee.com/573059382/coder-programming)

**欢迎大家关注并Star~**
