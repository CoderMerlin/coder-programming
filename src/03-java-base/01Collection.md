## 1.什么是集合？

**来自百度百科的回答：**
    集合类存放于java.util包中。
    集合类存放的都是对象的引用，而非对象本身，出于表达上的便利，我们称集合中的对象就是指集合中对象的引用（reference)。
    集合类型主要有3种：set(集）、list(列表）和map(映射)。
    集合接口分为：Collection和Map，list、set实现了Collection接口



## 2.JAVA中集合类型都有哪些？各有什么特点？

集合类型主要有3种：set(集）、list(列表）和map(映射)。
1、List（有序、可重复）
List里存放的对象是有序的，同时也是可以重复的，List关注的是索引，拥有一系列和索引相关的方法，查询速度快。
因为往list集合里插入或删除数据时，会伴随着后面数据的移动，所有插入删除数据速度慢。

2、Set（无序、不能重复）
Set里存放的对象是无序，不能重复的，集合中的对象不按特定的方式排序，只是简单地把对象加入集合中。

3、Map（键值对、键唯一、值不唯一）
Map集合中存储的是键值对，键不能重复，值可以重复。根据键得到值，对map集合遍历时先得到键的set集合，对set集合进行遍历，得到相应的值。



### 3.快速了解集合的父类Collection

3.1 Collection 体系结构图
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

3.2 Collection 中的主要方法

boolean add(E e) 向集合中添加元素
boolean addAll(Collection c) 向目标结合追加指定的集合
boolean equals(Object o) 清空集合
boolean contains(Object o) 判断集合中是否包含指定元素
boolean containsAll(Collection c 判断集合中是否包含指定集合的所有元素
boolean equals(Object o) 判断指定对象是否和集合相等
int hashCode() 返回对象的哈希码
boolean isEmpty() 判断集合是否为空
Iterator iterator() 返回集合的迭代器，所谓迭代器，就是用于结合的遍历输出
default Stream parallelStream() java8提供的方法，这里小编也没有研究
boolean remove(Object o) 从集合中删除指定元素，删除前会调用里面对象的equals方法进行对象的比较所以自定义的类需要尽心equals方法的重写
boolean removeAll(Collection c) 移除集合中那些也包含在指定集合c中的所有元素
default boolean removeIf(Predicate filter) Java8新增方法，不做探究
boolean retainAll(Collection c) 保留两个集合的共同元素，其他元素删除
int size() 返回集合元素的个数
default Spliterator spliterator() Java8新增方法，不做探究
default Stream stream() Java8新增方法，不做探究
Object[] toArray() 返回集合所有元素组成的对象数组
T[] toArray(T[] a) java5之后增加了泛型机制，这里更加安全，明确指定对象数组里面保存的对象类型
特别注意：List接口扩展了一个一些方法，其中最重要，也是用的最多的是：

E get(int index) 返回指定索引的元素

还有一些操作整个集合的方法，比如 :
boolean containsAll(Collection<?> c) 
是否包含指定集合 c 的全部元素
boolean addAll(Collection<? extends E> c) 
添加集合 c 中所有的元素到本集合中，如果集合有改变就返回 true
boolean removeAll(Collection<?> c) 
删除本集合中和 c 集合中一致的元素，如果集合有改变就返回 true
boolean retainAll(Collection<?> c) 
保留本集合中 c 集合中两者共有的，如果集合有改变就返回 true
void clear() 
删除所有元素
还有对数组操作的方法：
Object[] toArray() 
返回一个包含集合中所有元素的数组
<T> T[] toArray(T[] a) 
返回一个包含集合中所有元素的数组，运行时根据集合元素的类型指定数组的类型
在 JDK 8 以后，Collection 接口还提供了从集合获取连续的或者并行流：
Stream<E> stream()
Stream<E> parallelStream()
于Collection接口相关还有一个抽象类AbstractCollection：
AbstractCollection是一个抽象类，实现了Collection接口的部分功能，实现了一些最基本的通用操作，把复杂的和业务相关的延迟到子类实现。
在AbstractCollection中，主要实现了contains(), isEmpty(), toArray(), remove(), clear() 这几个操作。有兴趣的同学可以自行研究下，逻辑都比较简单。


3.3 Collection子类主要实现接口
Collection是最基本的集合接口，一个Collection代表一组Object，即Collection的元素（Elements）。一些Collection允许相同的元素而另一些不行。
一些能排序而另一些不行。Java SDK不提供直接继承自Collection的类，Java SDK提供的类都是继承自Collection的“子接口”如List和Set。 
所有实现Collection接口的类都必须提供两个标准的构造函数：无参数的构造函数用于创建一个空的Collection，有一个Collection参数的构造函数用于
创建一个新的Collection，这个新的Collection与传入的Collection有相同的元素。后一个构造函数允许用户复制一个Collection。 
如何遍历Collection中的每一个元素？不论Collection的实际类型如何，它都支持一个iterator()的方法，该方法返回一个迭代子，使用该迭代子即可逐一访问
Collection中每一个元素。典型的用法如下： 
　　　　
```
Iterator it = collection.iterator(); // 获得一个迭代子 
　　　　while(it.hasNext()) { 
　　　　　　Object obj = it.next(); // 得到下一个元素 
　　　　} 
```


　　由Collection接口派生的两个接口是List和Set。 


4.数组和集合都有哪些区别？

数组特点：

1.数组本质上就是一段连续的内存空间，用于记录多个类型相同的数据；

2.数据一旦声明完毕，则内存空间固定不变；

3.插入和删除操作不方便，可能会移动大量的元素并导致效率太低；

4.支持下标访问，可以实现随机访问；

5.数组中的元素可以是基本数据类型，也可以使用引用数据类型。

集合特点：

1.内存空间可以不连续，数据类型可以不相同；

2.集合的内存空间可以动态的调整；

3.集合的插入删除操作可以不移动大量元素；

4.部分支持下标访问，部分不支持；

5.集合中的元素必须是引用数据类型(你存储的是简单的int，它会自动装箱成Integer)；

可以看出数组和集合在数据的存储，访问，类型，长度等都有不同的地方。


5.集合中几种重要的类和接口简介
Collection接口 
List接口 
　　List是有序的Collection，使用此接口能够精确的控制每个元素插入的位置。用户能够使用索引（元素在List中的位置，类似于数组下标）来访问List中的元素，这类似于Java的数组。 
和下面要提到的Set不同，List允许有相同的元素。 
　　除了具有Collection接口必备的iterator()方法外，List还提供一个listIterator()方法，返回一个ListIterator接口，和标准的Iterator接口相比，ListIterator多了一些add()之类的方法，允许添加，删除，设定元素，还能向前或向后遍历。 
　　实现List接口的常用类有LinkedList，ArrayList，Vector和Stack。 
LinkedList类 
　　LinkedList实现了List接口，允许null元素。此外LinkedList提供额外的get，remove，insert方法在LinkedList的首部或尾部。这些操作使LinkedList可被用作堆栈（stack），队列（queue）或双向队列（deque）。 
　　注意LinkedList没有同步方法。如果多个线程同时访问一个List，则必须自己实现访问同步。一种解决方法是在创建List时构造一个同步的List： 
　　　　List list = Collections.synchronizedList(new LinkedList(...)); 

ArrayList类 
　　ArrayList实现了可变大小的数组。它允许所有元素，包括null。ArrayList没有同步。 
size，isEmpty，get，set方法运行时间为常数。但是add方法开销为分摊的常数，添加n个元素需要O(n)的时间。其他的方法运行时间为线性。 
　　每个ArrayList实例都有一个容量（Capacity），即用于存储元素的数组的大小。这个容量可随着不断添加新元素而自动增加，但是增长算法并没有定义。当需要插入大量元素时，在插入前可以调用ensureCapacity方法来增加ArrayList的容量以提高插入效率。 
　　和LinkedList一样，ArrayList也是非同步的（unsynchronized）。 

Vector类 
　　Vector非常类似ArrayList，但是Vector是同步的。由Vector创建的Iterator，虽然和ArrayList创建的Iterator是同一接口，但是，因为Vector是同步的，当一个Iterator被创建而且正在被使用，另一个线程改变了Vector的状态（例如，添加或删除了一些元素），这时调用Iterator的方法时将抛出ConcurrentModificationException，因此必须捕获该异常。 

Stack 类 
　　Stack继承自Vector，实现一个后进先出的堆栈。Stack提供5个额外的方法使得Vector得以被当作堆栈使用。基本的push和pop方法，还有peek方法得到栈顶的元素，empty方法测试堆栈是否为空，search方法检测一个元素在堆栈中的位置。Stack刚创建后是空栈。 

Set接口 
　　Set是一种不包含重复的元素的Collection，即任意的两个元素e1和e2都有e1.equals(e2)=false，Set最多有一个null元素。 
　　很明显，Set的构造函数有一个约束条件，传入的Collection参数不能包含重复的元素。 
　　请注意：必须小心操作可变对象（Mutable Object）。如果一个Set中的可变元素改变了自身状态导致Object.equals(Object)=true将导致一些问题。 

Map接口 
　　请注意，Map没有继承Collection接口，Map提供key到value的映射。一个Map中不能包含相同的key，每个key只能映射一个value。Map接口提供3种集合的视图，Map的内容可以被当作一组key集合，一组value集合，或者一组key-value映射。 

Hashtable类 
　　Hashtable继承Map接口，实现一个key-value映射的哈希表。任何非空（non-null）的对象都可作为key或者value。 
　　添加数据使用put(key, value)，取出数据使用get(key)，这两个基本操作的时间开销为常数。 
Hashtable通过initial capacity和load factor两个参数调整性能。通常缺省的load factor 0.75较好地实现了时间和空间的均衡。增大load factor可以节省空间但相应的查找时间将增大，这会影响像get和put这样的操作。 
使用Hashtable的简单示例如下，将1，2，3放到Hashtable中，他们的key分别是”one”，”two”，”three”： 
　　　　Hashtable numbers = new Hashtable(); 
　　　　numbers.put(“one”, new Integer(1)); 
　　　　numbers.put(“two”, new Integer(2)); 
　　　　numbers.put(“three”, new Integer(3)); 
　　要取出一个数，比如2，用相应的key： 
　　　　Integer n = (Integer)numbers.get(“two”); 
　　　　System.out.println(“two = ” + n); 
　　由于作为key的对象将通过计算其散列函数来确定与之对应的value的位置，因此任何作为key的对象都必须实现hashCode和equals方法。hashCode和equals方法继承自根类Object，如果你用自定义的类当作key的话，要相当小心，按照散列函数的定义，如果两个对象相同，即obj1.equals(obj2)=true，则它们的hashCode必须相同，但如果两个对象不同，则它们的hashCode不一定不同，如果两个不同对象的hashCode相同，这种现象称为冲突，冲突会导致操作哈希表的时间开销增大，所以尽量定义好的hashCode()方法，能加快哈希表的操作。 
　　如果相同的对象有不同的hashCode，对哈希表的操作会出现意想不到的结果（期待的get方法返回null），要避免这种问题，只需要牢记一条：要同时复写equals方法和hashCode方法，而不要只写其中一个。 
　　Hashtable是同步的。 

HashMap类 
　　HashMap和Hashtable类似，不同之处在于HashMap是非同步的，并且允许null，即null value和null key。，但是将HashMap视为Collection时（values()方法可返回Collection），其迭代子操作时间开销和HashMap的容量成比例。因此，如果迭代操作的性能相当重要的话，不要将HashMap的初始化容量设得过高，或者load factor过低。 

WeakHashMap类 
　　WeakHashMap是一种改进的HashMap，它对key实行“弱引用”，如果一个key不再被外部所引用，那么该key可以被GC回收。 
 