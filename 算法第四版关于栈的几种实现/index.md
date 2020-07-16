# 算法第四版关于栈和队列


## 栈和队列

许多基础数据类型都和对象的集合有关。具体来说，数据类型的值就是一组对象的集合，所有操作都是关于添加、删除或者是访问集合中的对象。这里主要讨论的是栈（stack）和队列（queue）的几种实现。

### 链表

**定义：**链表是一种递归的数据结构，它或者为空（null），或者是指向一个结点（node）的引用，该结点含有一个泛型元素和一个指向另一条链表的引用。

**注意：**之所以在这里定义，是因为后续我们的栈和队列的实现都要使用链表来构造。**在结构化存储数据集时，链表是数组的一种重要的代替方式**。

```java
//定义Node，且将Node声明为私有嵌套类之后，我们可以将Node的方法和实例变量的访问范围限制在包含它的类中
private class Node
{
	Item item;
	Node next;
}
```

```java
数据结构							优点								缺点
数组						通过索引可以直接访问任意元素				在初始化时就需要知道元素的数量
链表						使用的空间大小和元素数量成正比				需要通过引用访问任意元素
```



### 栈

下压栈或简称栈，是一种基于后进先出（LIFO）策略的集合类型。

----

```java
下压栈（后进先出，LIFO）

public class Stack<Item> implements Iterable<Item>

Stack() //创建一个空栈
void push() //添加一个元素
Item pop()	//删除最近添加的元素
boolean isEmpty() //栈是否为空
int size()	//栈中元素的数量
```



**下压(LIFO)栈（能够动态的调整数组大小）**

```java
import java.util.Iterator;
import java.util.Objects;
public class ResizingArrayStack<Item> implements Iterable<Item> {
    private Item[] a = (Item[]) new Objects[1];//栈元素
    private int N = 0;//元素数量
    public boolean isEmpty() {
        return N == 0;
    }
    public int size() {
        return N;
    }
    private void resize(int max) {
        //将栈移动到一个大小为max的新数组
        Item[] temp = (Item[]) new Objects[max];
        for (int i = 0; i < N; i++) {
            temp[i] = a[i];
        }
        a = temp;
    }
    public void push(Item item) {
        //将元素添加到栈顶
        if (N == a.length)
            resize(2 * a.length);
        a[N++] = item;
    }
    public Item pop() {
        //从栈顶删除元素
        Item item = a[--N];
        a[N] = null;//避免对象游离
        if (N > 0 && N == a.length / 4) resize(a.length / 2);
        return item;
    }
    @Override
    public Iterator<Item> iterator() {
        return (Iterator<Item>) new ReverseArrayIterator();
    }
    private class ReverseArrayIterator implements Iterable<Item> {
        //支持后进先出的迭代
        private int i = N;
        public boolean hasnext() {
            return i > 0;
        }
        public Item next() {
            return a[--i];
        }
        public void remove() {
        }
        @Override
        public Iterator<Item> iterator() {
            return null;
      }
    }
}
```

这份泛型的可迭代的`Stack API`的实现是所有集合类抽象数据类型实现的模板。它将所有元素保存在数组中，并动态调整数组的大小以保持数组大小和栈大小之比小于一个常数。

特别要注意的是

- `push()`操作中，检查数组是否太小。具体来说，我们会通过检查栈大小N和数组大小`a.length`是否相等来检查数组是否能够容纳新的元素。如果没有多余的空间，我们会将数组的长度加倍。然后就可以像以前一样使用`a[N++] = item`插入新元素了。
- 类似，`pop()`操作中，首先删除栈顶元素，然后如果数组太大我们就将它的长度减半。只要稍加思考，你就明白正确的检测条件是栈的大小是否小于数组的四分之一。在数组长度被减半之后，它的状态大约为半满，在下次需要改变数组大小之前 仍然能够进行多次`push()`和`pop()`操作。
- 在以上实现中，栈永远不会溢出，使用率也永远不会低于四分之一（除非栈为空，那种情况下数组的大小为1）。

#### 应用

- 算数表达式求值

  - 将操作数压入操作数栈；
  - 将运算符压入运算符栈；
  - 忽略左括号；
  - 在遇到右括号时，弹出一个运算符，弹出所需数量的操作数，并将运算符和操作数的运算结果压入操作数栈。

  **Dijkstra的双栈算数表达式求值算法**

  ```java
  public class Evaluate {
      public static void main(String[] args) {
          Stack<String> ops = new Stack<String>();
          Stack<Double> vals = new Stack<Double>();
          while (!StdIn.isEmpty()) {//读取字符，如果是运算符则压入栈
              String s = StdIn.readString();
              if (s.equals("(")) ;
              else if (s.equals("+"))
                  ops.push(s);
              else if (s.equals("-"))
                  ops.push(s);
              else if (s.equals("*"))
                  ops.push(s);
              else if (s.equals("/"))
                  ops.push(s);
              else if (s.equals("sqrt"))
                  ops.push(s);
              else if (s.equals(")")) {//如果字符是“）”则弹出运算符和操作数，计算结果并压入栈
                  String op = ops.pop();
                  double v = vals.pop();
                  if (op.equals("+")) v = vals.pop() + v;
                  else if (op.equals("-")) v = vals.pop() - v;
                  else if (op.equals("*")) v = vals.pop() * v;
                  else if (op.equals("/")) v = vals.pop() / v;
                  else if (op.equals("sqrt")) v = Math.sqrt(v);
                  vals.push(v);
              }//如果字符既不是运算符也不是运算数，将它作为double压入栈
              else vals.push(Double.parseDouble(s));
          }
          StdOut.println(vals.pop());
      }
}```




**下压堆栈（链表实现）**

```java
public class Stack<Item> implements Iterable<Item>{
	private Node first;//栈顶，最近添加的元素
	private int N;//元素的数量
	private class Node{
		//定义了结点的嵌套
		Item item;
		Node next;
	}
	public boolean isEmpty(){return first == null;}//或N == 0
	public int size(){return N;}
	public void push(Item item){
		Node oldfirst = first;//将栈顶元素变成第二个最近添加的元素
		first = new Node();//申请一个新的结点
		first.item = item;//新结点的item赋值
		first.next = oldfirst;//连接,此时新结点变成栈顶的元素
		N++;
	}
	public Item pop(){
		Item item = first.item;//保存栈顶元素的item值
		first = first.next;//修改栈顶元素,first的后一个元素变为栈顶元素
		N--;
		return item;//返回栈顶元素
	}
}
```




### 队列

先进先出队列或简称队列，是一种基于先进先出（FIFO）策略的集合类型。

---

```java
先进先出（FIFO）队列

public class Queue<Item> implements Iterable<Item>

Queue() //创建空队列
void enqueue() //添加一个元素
Item dequeue() //删除最早添加的元素
boolean isEmpty() //判断队列是否为空
int size() //队列中元素的数量`
```



**先进先出队列(链表实现)**

```java
public class Queue<Item> implements Iterable<Item>{
	private Node first;//指向最早添加的结点链接
	private Node last;//指向最近添加的结点链接
	private int N;//队列中的元素数量
	private class Node{
		Item item;
		Node next;
	}
	public boolean isEmpty(){return N == 0;}//或者first == null
	public int size(){return N;}
	public void enqueue(){
		//向表尾添加元素
		Node oldlast = last;
		last = new Node();
		last.item = item;
		last.next = null;
		if(isEmpty()){//判断队列是否之前是空队列，如果是那么此时队列里只有刚刚添加的一个元素
			first = last;
		}
		else
			oldlast.next = last;
		N++;
	}
	public Item dequeue(){
		//从表头删除元素
		Item item = first.item;
		first = first.next;
		if(isEmpty()){
			last = null;
		}
		N--;
		return item;
	}
}
```



### 练习

编写一个函数，接受一条链表的首结点作为参数，将链表反转并返回结果链表的首结点。

迭代的方式解答：为了完成这个任务，我们需要记录链表中的三个连续的结点：`reverse、first、second`。
在每一轮迭代中，我们从原始链表中提取结点first并将它插入到逆链表的开头。我们需要一直保持`first`指向原来链表中所有剩余结点的首结点，`second`指向原来链表的所有剩余结点的第二个结点，`reverse`指向结果链表中的首结点。

```java
迭代
public Node reverse(Node x){
    Node first = x;
    Node reverse = null;
    while(first!=null){
        Node second = first.next;
        first.next = reverse;
        reverse = first;
        first = second;
    }
    return reverse;
}

递归:假设链表含有N个结点，我们先递归颠倒最后N-1个结点，然后小心地将原链表中的首结点插入到结果链表的末端。
public Node reverse(Node first){
    if(first = null)
        return null;
    if(first.next == null)
        return first;
    Node second = first.next;
    Node rest = reverse(second);
    second.next = first;
    first.next = null;
    return rest;
}
```


