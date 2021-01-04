# Java的Collection体系


# Collection

- Collection 是最基本的集合接口。JDK 提供的类都是继承自 Collection 的子接口，例如 List 和 Set。
- Collection 是集合类的根接口，是高度抽象的集合，包含了集合的基本操作：添加、删除、清空、遍历、是否为空、获取大小等。
- Collection 继承了`Iterable<E>`，所以 Collection 的体系都支持`iterator()`方法，该方法返回一个迭代器，使用该迭代器可以逐一访问 Collection 的每一个元素。
- Collection 体系提供的常⽤⽅法：
  - R: `size()/isEmpty()/contains()/for()/stream() `
  - C/U: `add()/addAll()/retainAll()`
    - 这里特别要说一下，`retainAll()` 非常容易踩坑，如果`a.retainAll(b)` 这相当于求集合 a、b 的交集，然后把结果集放在集合 a 中，最终可能会导致集合 a 的`size()`减少。
  - D: `clear()/remove()/removeAll()`

## Collection 的继承体系

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

## List 和 Set 约定

集合约定了 List 是有序的，而 Set 是无序的，有没有序是根据存储顺序和取出顺序是否一致来约定。

# Collection 子接口

## List

List 是一个有序的队列，每一个元素都有它的索引，第一个元素的索引值是 0。

- ArrayList
  - List 中最常用的就是 ArrayList，它是一个动态数组，它可以实现动态扩容，每次创建原数组容量**1.5 倍**的新数组，把原数组的元素拷贝到新数组。ArrayList 允许所有元素包括 null，同时 ArrayList 也不是线程安全的。
- LinkedList
  - LinkedList 实现了 List 接口，允许元素为空，LinkedList 提供了额外的 get,remove,insert 方法，这些操作可以使 LinkedList 被用作堆栈、队列或双向队列。LinkedList 并不是线程安全的，如果多个线程同时访问 LinkedList，则必须自己实现访问同步，或者另外一种解决方法是在创建 List 时构造一个同步的 List。

## 重要约定：HashCode

- HashCode 在 java 中是 int 类型。
- 同⼀个对象必须始终返回相同的 hashCode 。
- 两个对象的 equals 返回 true，必须返回相同的 hashCode。
- 两个对象不等，也可能返回相同的 hashCode，int 最大 42 亿，而对象是无限的。

## Set

Set 是一个不允许有重复元素的集合。
判断重复：`equals()`<br>
Set 中还引入了 Hash 桶的概念，这是一个抽象的容器，不同的 HashCode 就像是一个个的桶，而 Set 每加入新的对象就会依次对比它们的 HashCode，如果与 Hash 桶相匹配就丢入桶内。将一个个对象映射成 HashCode 的方法是 `HashCode`。

- HashSet
  - HashSet 是最高效的 Set 实现，线程不安全。
  - HashSet 依赖于 HashMap，底层是通过 HashMap 实现的，HashMap 的 Key 就是一个 HashSet，所以 HashSet 能实现的 HashMap 都能实现，HashSet 的构造器中 `new` 了一个 HashMap。
  - HashMap 在 1.7 之前使用的是数组+链表实现，在 1.8+使用的数组+链表+红黑树实现。**链表长度大于 8 则转为红黑树**
- LinkedHashSet 继承了 HashSet。实现了一个有序的 HashSet。
- TreeSet
  - 同样，TreeSet 依赖于 TreeMap，底层通过 TreeMap 来实现的。
  - TreeSet 不允许 null 值。
  - TreeSet 线程不安全。
  - TreeSet 实现了 SortedSet 接口，从而保证了添加的元素按照元素的自然顺序(递增或其他顺序)在集合中进行存储。

## Queue(FIFO)

队列数据结构，先进先出。<br>

- Deque 双端队列，继承自 Queue，代替了栈(Stack)。
- PriorityQueue 优先级队列，本质上是堆。是 Queue 的实现类 AbstractQueue 的子类，实现了 Serializable 接口。

## Vector

类似与 ArrayList 的动态数组，线程安全，访问较慢。

- Stack 栈(LIFO)，继承自 Vector，先进后出，被双端队列替代。

# 集合的另一个基本接口 Map

在《java Core 卷一 第九版》一书中的 569 页，13.3 节 集合框架 的倒数第 16 行原文：“集合有两个基本的接口：Collection 和 Map。可以使用下列方法向集合中插入元素……”<br>
Map 接口中键和值一一映射。可以通过键来获取值。

- 方法：
  - C/U: `put()/putAll()`
  - R:
    - `get()/size()`
    - `containsKey()/containsValue()`
    - `keySet()/values()/entrySet()`
  - D: `remove()/clear()`
- HashMap
  - HashMap 是 Map 的实现类，是最高效的 Map 实现。
  - HashMap 的 key 和 value 都允许为 null，HashMap 遇到 key 为 null 的时候，调用 putForNullKey 方法进行处理，而对 value 没有处理。
  - HashMap 是动态的，它的扩容类似与 ArrayList，区别是 HashMap 是申请一个当前容量**2 倍**的桶数组，再将 HashMap 的 key/value 映射逐个添加进去。
  - HashMap 线程不安全。原因是 HashMap 在多线程扩容时可能会变成死循环。解决方案是使用 **ConcurrentHashMap**。
    1. put 时导致多线程数据不一致。有线程 A、B，假设 HashCode 相同，A 找到了与之对应的 Hash 桶坐标准备插入数据，但是此时 A 的时间片用完了，A 进入阻塞状态，此时 B 找到 Hash 桶成功插入数据，但是和之前 A 要插入数据的位置相同，而后 A 又被调度，就会把 B 的数据覆盖，最后导致数据不一致问题。
    2. 死循环，`get() -> resize()` 导致的循环链表。
- Hashtable
  - 继承自 Dictionary 类。Dictionary 是任何可将键映射到相应值的类的抽象父类。
  - Hashtable 的 key 和 value 都不允许为 null，Hashtable 遇到 key 为 null，直接返回 NullPointerException。
  - Hashtable 线程安全，而 HashMap 则不是。Hashtable 几乎所有的 public 的方法都是**synchronized**的，而有些方法也是在内部通过**synchronized**代码块来实现。所以多线程使用 Hashtable。

# Collections 工具类

## 注意：工具类一般都是在实现类的名字后面加一个 s,例如 Guava 中的 Lists/Sets/Maps。

- `emptySet()`: 等返回⼀个⽅便的空集合。
- `synchronizedCollection`: 将⼀个集合变成线程安全的。
- `unmodifiableCollection`: 将⼀个集合变成不可变的(也可以
  使⽤ Guava 的 Immutable)。

# Guava

## Google Guava 是 Java 的一组开源通用库，主要由 Google 工程师开发。

- 不要重复发明轮⼦！尽量使⽤经过实战检验的类库
- [Guava Github Repo](https://github.com/google/guava)<br>
- [Guava 中文教程](https://legacy.gitbook.com/book/wizardforcel/guava-tutorial/details)

