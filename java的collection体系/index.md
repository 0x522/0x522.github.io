# Java的collection体系

# Collection
* Collection 是最基本的集合接口。JDK提供的类都是继承自Collection的子接口，例如List和Set。
* Collection 是集合类的根接口，是高度抽象的集合，包含了集合的基本操作：添加、删除、清空、遍历、是否为空、获取大小等。
* Collection 继承了`Iterable<E>`，所以Collection的体系都支持`iterator()`方法，该方法返回一个迭代器，使用该迭代器可以逐一访问Collection的每一个元素。
* Collection体系提供的常⽤⽅法： 
  * R: `size()/isEmpty()/contains()/for()/stream() `
  * C/U: `add()/addAll()/retainAll()`
    * 这里特别要说一下，`retainAll()` 非常容易踩坑，如果`a.retainAll(b)` 这相当于求集合a、b的交集，然后把结果集放在集合a中，最终可能会导致集合a的`size()`减少。
  * D: `clear()/remove()/removeAll()`
## Collection的继承体系
```java
Collection
    |-----List  有序(存储顺序和取出顺序一致)，可重复
        |----ArrayList  线程不安全，底层使用数组实现，查询快，增删慢，效率高。
        |----LinkedList  线程不安全，底层使用链表实现，查询慢，增删快，效率高。
        |----Vector  线程安全，底层使用数组实现，查询快，增删慢，效率低。每次容量不足时，默认自增长度的一倍（如果不指定增量的话）。
    |-----Set   元素唯一一个不包含重复元素的 Collection。更确切地讲，set 不包含满足 e1.equals(e2) 的元素对 e1 和 e2，并且最多包含一个 null 元素。
        |--HashSet  底层是由HashMap实现的，通过对象的hashCode方法与equals方法来保证插入元素的唯一性，无序(存储顺序和取出顺序不一致)，。
            |--LinkedHashSet 底层数据结构由哈希表和链表组成。哈希表保证元素的唯一性，链表保证元素有序。(存储和取出是一致)
        |--TreeSet  基于 TreeMap 的 NavigableSet 实现。使用元素的自然顺序对元素进行排序，或者根据创建 set 时提供的 Comparator 进行排序，具体取决于使用的构造方法。 元素唯一。
```
## List和Set约定
集合约定了List是有序的，而Set是无序的，有没有序是根据存储顺序和取出顺序是否一致来约定。


# Collection子接口
## List
List是一个有序的队列，每一个元素都有它的索引，第一个元素的索引值是0。
* ArrayList
  * List 中最常用的就是ArrayList，它是一个动态数组，它可以实现动态扩容，每次创建原数组容量**1.5倍**的新数组，把原数组的元素拷贝到新数组。ArrayList允许所有元素包括null，同时ArrayList也不是线程安全的。
* LinkedList
  * LinkedList实现了List接口，允许元素为空，LinkedList提供了额外的get,remove,insert方法，这些操作可以使LinkedList被用作堆栈、队列或双向队列。LinkedList并不是线程安全的，如果多个线程同时访问LinkedList，则必须自己实现访问同步，或者另外一种解决方法是在创建List时构造一个同步的List。

## 重要约定：HashCode
  * HashCode 在java中是int类型。
  * 同⼀个对象必须始终返回相同的hashCode 。
  * 两个对象的equals返回true，必须返回相同的hashCode。
  * 两个对象不等，也可能返回相同的hashCode，int最大42亿，而对象是无限的。
## Set
Set是一个不允许有重复元素的集合。
判断重复：`equals()`<br>
Set中还引入了Hash桶的概念，这是一个抽象的容器，不同的HashCode就像是一个个的桶，而Set每加入新的对象就会依次对比它们的HashCode，如果与Hash桶相匹配就丢入桶内。将一个个对象映射成HashCode的方法是 `HashCode`。
* HashSet
  * HashSet是最高效的Set实现，线程不安全。
  * HashSet依赖于HashMap，底层是通过HashMap实现的，HashMap的Key就是一个HashSet，所以HashSet能实现的HashMap都能实现，HashSet的构造器中 `new` 了一个HashMap。
  * HashMap在1.7之前使用的是数组+链表实现，在1.8+使用的数组+链表+红黑树实现。**链表长度大于8则转为红黑树**
* LinkedHashSet 继承了HashSet。实现了一个有序的HashSet。
* TreeSet
  * 同样，TreeSet依赖于TreeMap，底层通过TreeMap来实现的。
  * TreeSet不允许null值。
  * TreeSet线程不安全。
  * TreeSet实现了SortedSet接口，从而保证了添加的元素按照元素的自然顺序(递增或其他顺序)在集合中进行存储。

## Queue(FIFO)
队列数据结构，先进先出。<br>
* Deque 双端队列，继承自Queue，代替了栈(Stack)。
* PriorityQueue 优先级队列，本质上是堆。是Queue的实现类AbstractQueue的子类，实现了Serializable接口。
## Vector
类似与ArrayList的动态数组，线程安全，访问较慢。
* Stack 栈，继承自Vector，先进后出，被双端队列替代。


# 集合的另一个基本接口Map
**在《java Core 卷一 第九版》一书中的569页，13.3节 集合框架 的倒数第16行原文：“集合有两个基本的接口：Collection和Map。可以使用下列方法向集合中插入元素……”**<br>
Map 接口中键和值一一映射。可以通过键来获取值。
* 方法：
  * C/U: `put()/putAll()` 
  * R:
    * `get()/size()`
    * `containsKey()/containsValue()`
    * `keySet()/values()/entrySet()` 
  * D: `remove()/clear()`
* HashMap 
  * HashMap是Map的实现类，是最高效的Map实现。
  * HashMap的key和value都允许为null，HashMap遇到key为null的时候，调用putForNullKey方法进行处理，而对value没有处理。
  * HashMap是动态的，它的扩容类似与ArrayList，区别是HashMap是申请一个当前容量**2倍**的桶数组，再将HashMap的key/value映射逐个添加进去。
  * HashMap线程不安全。原因是HashMap在多线程扩容时可能会变成死循环。解决方案是使用 **ConcurrentHashMap**。
    1. put时导致多线程数据不一致。有线程A、B，假设HashCode相同，A找到了与之对应的Hash桶坐标准备插入数据，但是此时A的时间片用完了，A进入阻塞状态，此时B找到Hash桶成功插入数据，但是和之前A要插入数据的位置相同，而后A又被调度，就会把B的数据覆盖，最后导致数据不一致问题。
    2. 死循环，`get() -> resize()` 导致的循环链表。
* Hashtable
  * 继承自Dictionary类。Dictionary 是任何可将键映射到相应值的类的抽象父类。
  * Hashtable的key和value都不允许为null，Hashtable遇到key为null，直接返回NullPointerException。
  * Hashtable线程安全，而HashMap则不是。Hashtable几乎所有的public的方法都是**synchronized**的，而有些方法也是在内部通过**synchronized**代码块来实现。所以多线程使用Hashtable。


# Collections工具类
## 注意：工具类一般都是在实现类的名字后面加一个s,例如Guava中的Lists/Sets/Maps。
* `emptySet()`: 等返回⼀个⽅便的空集合。
* `synchronizedCollection`: 将⼀个集合变成线程安全的。
* `unmodifiableCollection`: 将⼀个集合变成不可变的(也可以
使⽤Guava的Immutable)。

# Guava
## Google Guava是Java的一组开源通用库，主要由Google工程师开发。
* 不要重复发明轮⼦！尽量使⽤经过实战检验的类库
* [Guava Github Repo](https://github.com/google/guava)<br>
* [Guava 中文教程](https://legacy.gitbook.com/book/wizardforcel/guava-tutorial/details)
