# Java多线程


# Java多线程
## 什么是进程 什么是线程
>《操作系统》一书中给出了一个定义：进程是程序关于某数据集合的一次执行，是系统进行资源分配和调度的基本单位。<br>
>《Java核心技术 卷 I》14章 给出了一个定义：一个程序同时执行多个任务。通常，每一个任务称为一个*线程 (thread)*，它是线程控制的简称。
## 什么是多线程
可以同时运行一个以上线程的程序称为*多线程程序 (multithreaded)*，
## 多线程与多进程有哪些区别
本质的区别在于每个进程拥有自己的一整套变量，而线程则共享数据。因为这一性质，才导致多线程中共享的变量不是“安全的”。操作系统中，线程与进程相比较，线程更加轻量级，创建和撤销一个线程的开销要远小于线程。
## 为什么需要多线程
* CPU和内存、内存和硬盘读写速度极度不匹配，CPU的执行速度要远远快于其他设备。
* 现代CPU都是多核的，比如一个八核CPU，当运行单线程的时候，另外7核都处于空闲状态，多线程可以提高CPU的利用率。
* Java的执行模型是 **同步/阻塞** 模型。在默认情况下，java就是这种传统的IO模型：
  * 单线程处理问题按部就班，按照代码顺序执行。
  * 性能差劲。

## 如何开启新的线程
* Thread类<br>
Java提供了一个Thread线程类，它实现了Runnable接口。而Runnable接口中仅有一个抽象方法`run()`,它是一个函数式接口，可以传递给Runnable一个lambda表达式。<br>

### 下面是在一个单独线程中执行一个任务的简单流程：

1. 将任务代码移到实现了Runnable接口的类的`run()`方法中。
    ```java
    public interface Runnable {
        void run();
    }
    ```
    由于Runnable接口是一个函数式接口，可以用lambda表达式建立一个实例。
    ```java
    Runnable r = () -> {/** task code **/};
    ```
2. 由Runnable创建一个Thread对象：
    ```java
    Thread thread = new Thread(r);
    ```
3. 启动线程
    ```java
    thread.start();
    ```

### 特别要注意
* 只有`start()`方法才能并发执行。
* 每多开一个线程，就会多一个执行流。
* 每个线程都有自己独立的方法栈，每运行`start()`，就会给一个新线程开辟独立的方法栈，方法栈是线程私有的。
* 静态变量/类变量是被所有线程所共享的。

## 线程状态
线程可以有如下6种状态：
* New 新创建
* Runnable 可运行
* Block 被阻塞
* Waiting 等待
* Timed Waiting 计时等待
* Terminated 被终止

1. 新创建线程<br>当使用new操作符创建一个线程，如 `new Thread()` ，该线程还没有开始运行。这意味着线程的状态是New。
2. 可运行的线程<br>一旦调用`start()`方法，线程处于Runnable状态。一个可运行的线程可能正在运行也可能没有在运行，这取决于操作系统给线程提供运行的时间。一旦一个线程开始运行，它不必始终保持运行状态。
3. 被阻塞线程和等待线程<br>当线程处于被阻塞或等待状态时，它暂时不活动。它不运行任何代码而且消耗最少的资源。直到线程调度器重新激活该线程。
   * 当一个线程试图获取一个内部对象锁，该线程进入阻塞状态。当所有其他线程释放该锁，并且线程调度器允许本线程持有它的时候，该线程将变成非阻塞状态。
   * 当线程等待另一个线程通知调度器的一个条件时，自己进入等待状态。
   * 有几个方法有一个超时参数。调用它们导致线程进入计时等待状态。这一状态将一直保持到超时期满。
4. 被终止的线程
   1. 因为`run()`方法正常退出而自然死亡。
   2. 因为一个没有捕获的异常终止了`run()`方法而意外死亡。

## 多线程执行的本质
多线程的本质就是，一段相同的代码被不同的线程以不可预知的顺序和速度执行。

## 多线程带来的性能提升
* 对于IO密集型的应用特别有用
  * 网络IO (通常包括数据库)
  * 文件IO
* 对于CPU密集型的应用稍有折扣
* 性能提升的上限：理论上CPU占用可以达到100%
  * 单核CPU：100%
  * N核CPU：N × 100%

## 线程非常昂贵
* 能不能使用线程达到无穷无尽的提升
  * 不能
* 线程的昂贵性在于
  * CPU切换上下文很慢
  * 线程需要占用内存等系统资源
* 如果你的应用一天只有几个用户
  * 使用 `new Thread().start()`
* 如果你的应用负载很高，有很多用户访问
  * 使用JUC包



## 线程不安全的表现
* 数据错误
  * i++
    * 使用多线程对变量i从0开始累加1000次，最后得到的结果不是1000，问题在于对变量累加不是原子操作：
    * i++;被做了如下处理：
  ```java
    (1) 取i值，将i加载到寄存器。
    (2) 自增i。
    (3) 将结果写回i的内存位置。
    这里就会出现问题:
    我们假设i的初值为0，
    第一个线程1执行了步骤1、2。
    但是CPU给线程1执行的时间片完了，就会调度第二个线程2执行，
    但是此时i值为0并没有改变(线程1没有执行变量写回内存操作)，
    线程2继续执行完了步骤1、2、3，把此时的i值为1写回了内存，
    线程1又被调度获得了CPU，就继续之前没有进行的步骤3操作，
    线程1将之前执行步骤1、2的i值为1又写回了内存，
    覆盖了线程2写入的i值为1，此时的i值还是为1。就导致了数据错误。
  ```

  * if-then-do
    * 类似于i++的错误，也是因为多线程操作不是原子操作。
* 死锁
  >《操作系统》定义了死锁：如果一组进程中的每一个进程都在等待仅有该组进程中的其他进程才能引发的事件，那么成这组进程是死锁的。
  * 著名的HashMap的死循环问题。
  * 预防死锁产生的原则：
    * 所有线程都按照相同的顺序来获取锁。
  * 死锁问题的排查
    * jps/jstack
  * 多线程经典问题：哲学家用餐问题。

## 线程安全
* 线程的默认实现几乎都是线程不安全的，而且线程的操作不是原子性操作，导致共享变量的数据可能出错。
* 实现线程安全的基本手段：
  * 不可变的类：数据出错的根本原因就是并发的修改数据。
    * Integer/String
  * synchronized同步块
    * ```java
      1. public synchronized method(){/**Concurrent code**/}
      2. public static synchronized method(){/**Concurrent code**/}
      3. synchronized((Object) Lock){/**Concurrent code**/}
    * 同步块同步了什么东西？
      * synchronized(Object) 把这个对象当成锁
      * static synchronized方法 把Class对象当成锁
      * 实例的synchronized方法 把该实例当成锁
  * 普通Collection转ConcurrentCollection
    * Collections.synchronized
  * JUC
    * Atomic类
    * ConcurrentHashMap
      * 任何HashMap有线程不安全的地方都使用ConcurrentHashMap
      * 或者无脑使用
  * ReentrantLock 可重入锁
    * 语法：
      ```java
      ReentrantLock lock = new ReentrantLock();
      lock.lock();//加锁
      
      /**Concurrent code**/

      lock.unlock();//解锁
      ```
    * 条件对象
      ```java
      Condition newCondtion = Lock.newCondition();
      newCondition.await();//不满足条件，阻塞，放弃锁
      newCondition.signal();//随机唤醒其他线程
      ```
  * 读写锁ReentrantReadWriteLock
  * 注意synchronized也是可重入的
    * 可重入锁也叫递归锁，意思是当一个线程中的某个对象持有锁的时候可以再次持有锁。

## Object类里的线程方法
* Java从一开始就把线程作为语言特性，提供了语言级别的支持。
* 为什么Java中的所有对象都能成为锁
  * Object中有 `wait()/notify()/notifyAll()`方法

## 线程池
构建一个新的线程是要付出一定代价的，因为涉及与操作系统的交互。如果程序中创建了大量的生命周期很短的线程，就应该使用线程池(thread pool);一个线程池中包含许多准备运行的空闲线程。将Runnable对象交给线程池，就会有一个线程调用run方法。当run方法退出，线程不会死亡，而是在线程池中准备下一次运行。<br>
### 执行器(Executor)
执行器类中有许多静态工场方法构建线程池。
```java
newFixedThreadPool	该池包含固定数量的线程，空闲线程会被一直保留
newCachedThreadPool	必要时创建新线程，空闲线程会被保留60秒
newSingleThreadPool	只有一个线程的线程池，该线程顺序执行每一个提交的任务(类似于Swing事件分配线程)
newScheduledThreadPool	用于预定执行而构建的固定线程池，替代java.util.Timer
newSingleThreadScheduledPool	用于预定执行而构建的单线程池
```

* 下面总结了在使用连接池时应该做的事情：
  1. 调用Executors类中的静态方法newCacheThreadPool或newFixedThreadPool。
  2. 调用submit提交Runnable或者Callable对象。
  3. 如果想要取消一个任务，或如果提交Callable对象，就要保存好返回的Future对象。
  4. 当不再提交任何任务时，调用shotdown()。该方法启动该线程池的关闭序列。被关闭的执行器不再接受新的任务。当所有任务完成以后，线程池中的线程死亡。

## ThreadLocal
### 概念
>ThreadLocal 用于提供线程局部变量，在多线程环境可以保证各个线程里的变量独立于其它线程里的变量。也就是说 ThreadLocal 可以为每个线程创建一个单独的变量副本，相当于线程的 private static 类型变量。<br>
ThreadLocal 的作用和同步机制有些相反：同步机制是为了保证多线程环境下数据的一致性；而 ThreadLocal 是保证了多线程环境下数据的独立性。<br>
对于 ThreadLocal 类型的变量，在一个线程中设置值，不影响其在其它线程中的值。也就是说 ThreadLocal 类型的变量的值在每个线程中是独立的。
### ThreadLocal实现
ThreadLocal 是构造函数只是一个简单的无参构造函数，并且没有任何实现。<br>
#### Set(T value)
```java
public void set(T value) {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null)
        map.set(this, value);
    else
        createMap(t, value);
}
set(T value) 方法中，首先获取当前线程，然后在获取到当前线程的 ThreadLocalMap，如果 ThreadLocalMap 不为 null，则将 value 保存到 ThreadLocalMap 中，并用当前 ThreadLocal 作为 key；否则创建一个 ThreadLocalMap 并给到当前线程，然后保存 value。
ThreadLocalMap 相当于一个 HashMap，是真正保存值的地方。
```
#### get()
```java
public T get() {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        ThreadLocalMap.Entry e = map.getEntry(this);
        if (e != null) {
            @SuppressWarnings("unchecked")
            T result = (T)e.value;
            return result;
        }
    }
    return setInitialValue();
}
```
同样的，在 get() 方法中也会获取到当前线程的 ThreadLocalMap，如果 ThreadLocalMap 不为 null，则把获取 key 为当前 ThreadLocal 的值；否则调用 setInitialValue() 方法返回初始值，并保存到新创建的 ThreadLocalMap 中。

#### initialValue()
```java
private T setInitialValue() {
    T value = initialValue();
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null)
        map.set(this, value);
    else
        createMap(t, value);
    return value;
}
```
initialValue() 是 ThreadLocal 的初始值，默认返回 null，子类可以重写改方法，用于设置 ThreadLocal 的初始值。
#### remove()
```java
public void remove() {
    ThreadLocalMap m = getMap(Thread.currentThread());
    if (m != null)
        m.remove(this);
}
```
ThreadLocal 还有一个 remove() 方法，用来移除当前 ThreadLocal 对应的值。同样也是通过当前线程的 ThreadLocalMap 来移除相应的值。
### 当前线程的 ThreadLocalMap
>&nbsp;&nbsp;&nbsp;&nbsp;在 set，get，initialValue 和 remove 方法中都会获取到当前线程，然后通过当前线程获取到 ThreadLocalMap，如果 ThreadLocalMap 为 null，则会创建一个 ThreadLocalMap，并给到当前线程。<br>
>&nbsp;&nbsp;&nbsp;&nbsp;每一个线程都会持有有一个 ThreadLocalMap，用来维护线程本地的值。
在使用 ThreadLocal 类型变量进行相关操作时，都会通过当前线程获取到 ThreadLocalMap 来完成操作。每个线程的 ThreadLocalMap 是属于线程自己的，ThreadLocalMap 中维护的值也是属于线程自己的。这就保证了 ThreadLocal 类型的变量在每个线程中是独立的，在多线程环境下不会相互影响。
