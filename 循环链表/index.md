# 循环链表


## 循环链表

循环链表的实现和单链表基本相同，只是它的结构改变了，无头尾节点，整个链表是一个循环的结构。

循环链表的构造：每创建一个新的节点就让他的下一个元素指向它自己，这样来构成循环。

因为没有头尾节点，所以循环链表的追加元素方法变成了插入元素的方法(尾插法)。

**实现类：**

```java
public class LoopNode {
    private int data;
    //每一个独立的节点的next都指向它自己
    private LoopNode next = this;

    public LoopNode(int data) {
        this.data = data;
    }

    //取出当前节点的下一个节点
    public LoopNode getNext() {
        return this.next;
    }

    //删除下一个节点,先找到当前节点的下下个节点，保存一下，然后将保存的节点赋值给当前节点的下一个节点
    public void RemoveNext() {
        LoopNode Nextnext = this.next.next;
        this.next = Nextnext;
    }

    //循环链表没有头和尾，所以只有插入节点，在当前节点的后面插入一个节点
    //插入节点的思想是：先取到当前节点的下一个节点保存，然后将要插入的节点赋值给当前节点的下一个节点，最后将保存的节点赋值给插入节点的下一个节点
    public void insert(LoopNode node) {
        LoopNode Nextnext = this.next;
        this.next = node;
        node.next = Nextnext;

    }

    //获取当前节点的data值
    public int getData() {
        return this.data;
    }

}
```

**测试类：**

```java
public class LoopNodeTest {
    public static void main(String[] args) {
        LoopNode n1 = new LoopNode(1);
        LoopNode n2 = new LoopNode(2);
        LoopNode n3 = new LoopNode(3);
        n1.insert(n2);
        n2.insert(n3);
        System.out.println(n1.getNext().getData());
        System.out.println(n3.getNext().getData());
    }
}
```

