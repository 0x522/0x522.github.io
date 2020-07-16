# 双向循环链表


## 双向循环链表

双向循环链表是在循环链表的基本思想下，新增加了一个头指针，指向前一个元素。

特别要注意的是创建每一个独立的双向节点时，它的前后指针都指向自己。

如果做插入元素操作，从当前元素的后一个开始插入，首先保存当前元素的下一个元素(如果当前元素后没有其他元素，那么当前元素的尾指针总是指向第一个元素)



**实现类：**

```java
import java.util.zip.DeflaterOutputStream;

/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in 2020/3/10
 * @Modified by:
 */
public class DoubleNode {
    private int data;
    private DoubleNode pre = this;
    private DoubleNode next = this;

    public DoubleNode(int data) {
        this.data = data;
    }

    public DoubleNode getNext() {
        return this.next;
    }

    public DoubleNode getPre() {
        return this.pre;
    }

    public int getData() {
        return this.data;
    }

    //尾插：创建每一个独立的双向节点时，它的前后指针都指向自己，从当前元素的后一个开始插入，首先保存当前元素的下一个元素(如果当前元素后没有其他元素，那么当前元素的尾指针总是指向第一个元素)
    public void insert(DoubleNode node) {
        DoubleNode Nextnext = this.next;
        this.next = node;
        node.pre = this;
        node.next = Nextnext;
        Nextnext.pre = node;
    }
	//删除当前节点，并返回当前节点
    public DoubleNode removeNode() {
        DoubleNode currentNode = this.next;
        this.pre.next = currentNode;
        currentNode.pre = this.pre;
        return this;
    }
}
```



**测试类：**

```java
/**
 * @author: YunTaoChen
 * @description:
 * @Date:
 * @Modified by:
 */
public class DoubleNodeTest {
    public static void main(String[] args) {
        DoubleNode n1 = new DoubleNode(1);
        DoubleNode n2 = new DoubleNode(2);
        DoubleNode n3 = new DoubleNode(3);
        n1.insert(n2);
        n2.insert(n3);
        System.out.println(n1.getNext().getData());
        System.out.println(n2.getNext().getData());
        System.out.println(n3.getNext().getData());
        System.out.println(n3.getPre().getData());
    }
}
```


